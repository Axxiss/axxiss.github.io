---
layout: post
title: "Unit Testing with RxJava"
date: 2015-05-20
comments: true
tags: Java
---
When we are testing our [RxJava][1] code we can distinguish three main subjects to be tested:

- Observables
- Subscribers
- Notifications

Observables
-----------
Testing an [Observable][2] is pretty simple: create the observable, subscribe and assert! In order to execute all operations on the test thread we have to define subscriber's and observable's scheduler as [Schedulers.immediate()][5]. If we use other scheduler we'll end up with flaky tests.

Subscribers
-----------
When it comes to [Subscribers][3] we can differentiate two scenarios:

 1. Named subscriber
 2. Anonymous subscriber

Despite both implementations carry out the same function, the way of testing each one is totally different. A named subscriber can be constructed directly on our test by doing `new MySubscriber()`. As that subscriber extends `Subscriber<T>` we can access its public methods: onCompleted, onError and onNext; because of that just instantiate a new subscriber, call each method and do the assertion/verification for each case.

In contrast to named subscribers we're unable to create the same anonymous subscriber on our test case. Therefore to test it properly we have to go through the full chain of notifications, this will require working with different threads. We will go into further details on the next section.

Notifications
-----
This is the most complex part about testing with RxJava, as usually this could involve three threads:

 - observer: where the observable is running
 - subscriber: where observable results are pushed
 - current: where the test is running

Synchronizing all those threads solely for unit testing could be painful. We can override default schedulers instead handling thread's synchronization. To accomplish this 	we have to implement [RxJavaSchedulersHook][4] then tell [RxJavaPlugins][6] to use that hook for schedulers.

The first step is to override RxJavaSchedulersHook, overriding schedulers hooks allow us to return a custom scheduler for io, computation and new thread requests. To execute both observer and subscriber on the current thread we need to return `Schedulers.immediate()` for all Schdulers request.

``` java
    private class SchedulerHook extends RxJavaSchedulersHook{
        @Override
        public Scheduler getIOScheduler() {
            return Schedulers.immediate();
        }

        @Override
        public Scheduler getNewThreadScheduler() {
            return Schedulers.immediate();
        }

        @Override
        public Scheduler getComputationScheduler() {
            return Schedulers.immediate();
        }
    }
```

Besides scheduler hook we can also use [RxJavaObservableExecutionHook][7], this hook can be really helpful to obtain information about the execution order in our test. In my cases I implemented to know the actual order of execution and the execution thread. This is the log from one of my tests:

    [main] DEBUG RxJavaResetRule - onCreate
    [main] DEBUG RxJavaResetRule - onCreate
    [main] DEBUG RxJavaResetRule - onSubscribeStart
    [main] DEBUG RxJavaResetRule - onSubscribeStart
    [main] DEBUG RxJavaResetRule - onSubscribeReturn
	[main] DEBUG RxJavaResetRule - onSubscribeReturn

Are we ready to start testing? [Uh... NO][8]

The first problem comes when we have more than one test method on out test case, setup and teardown will be executed several times (one for each test) hence setting schedulers hook on each execution. The error we will get is an `IllegalStateException` saying "Another strategy was already registered". So let's take a look at `RxJavaPlugins` until we find `registerSchedulersHook` method.

``` java
public void registerSchedulersHook(RxJavaSchedulersHook impl) {
    if (!schedulersHook.compareAndSet(null, impl)) {
        throw new IllegalStateException("Another strategy was already registered: " + schedulersHook.get());
    }
}
```
That's the origin of the error we're seeing but if we keep looking we will find a package protected reset method that reset the current implementations of available hooks.

``` java
/* package accessible for unit tests */
void reset() {
    INSTANCE.errorHandler.set(null);
    INSTANCE.observableExecutionHook.set(null);
    INSTANCE.schedulersHook.set(null);
}
```

In order to access reset method we have to define a class under the same package as RxJavaPlugins. For reusability sake I ended up using a JUnit rule to reset the plugins and define the hooks.

``` java
package rx.plugins;

public class RxJavaResetRule implements TestRule {

    private static final Logger LOG = LoggerFactory.getLogger(RxJavaResetRule.class);

    @Override
    public Statement apply(Statement base, Description description) {
        return new Statement() {
            @Override
            public void evaluate() throws Throwable {
                //before: plugins reset, execution and schedulers hook defined
                RxJavaPlugins.getInstance().reset();
                RxJavaPlugins.getInstance().registerSchedulersHook(new SchedulerHook());
                RxJavaPlugins.getInstance().registerObservableExecutionHook(new ExecutionHook());

                base.evaluate();

                //after: clean up
                RxJavaPlugins.getInstance().reset();
            }
        };
    }

	//Hooks implementations
```

And we're finally there, we just have to add the rule to ALL our test that implies RxJava because onces the schedulers factory is initilized we cannot change its schedulers instances despite how many times we change the hook.


There is an [issue][9] on GitHub about making reset method public.


[1]: https://github.com/ReactiveX/RxJava
[2]: http://reactivex.io/RxJava/javadoc/rx/Observable.html
[3]: http://reactivex.io/RxJava/javadoc/rx/Subscriber.html
[4]: http://reactivex.io/RxJava/javadoc/rx/plugins/RxJavaSchedulersHook.html
[5]: http://reactivex.io/RxJava/javadoc/rx/schedulers/Schedulers.html#immediate()
[6]: http://reactivex.io/RxJava/javadoc/rx/plugins/RxJavaPlugins.html
[7]: http://reactivex.io/RxJava/javadoc/rx/plugins/RxJavaObservableExecutionHook.html
[8]: https://youtu.be/BKqtYkXIaG8
[9]: https://github.com/ReactiveX/RxJava/issues/2297
