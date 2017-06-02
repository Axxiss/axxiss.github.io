---
layout: post
title: Contact form backed by Parse
date: 2013-11-30
comments: true
tags: [JavaScript, Parse, HTML]
---
This is the first post of my brand new blog! <i class="fa fa-smile-o"></i>

What a better opportunity to write about something related to the setup of this site. On the contact page there is a contact form, as you should know when a form is submitted its send to a server where its data is processed. But lets be honest, I'm lazy, the DRY-KISS type (it could be worse). So I didn't want to setup a server, hire a hosting only to handle a stupid contact form then [Parse][2] came to the rescue.

<!-- more -->

For those who doesn't know it, Parse is [BaaS][3] platform who was recently acquired by Facebook; it offer a cloud backend for our software and I say software because it doesn't care if it's Android, iOS, JavaScript whatever. I have been testing it in a personal Android project and its really easy to use, leaving database/server administration to Parse itself.


That enough, lets go to the point.

The form
========

The form has nothing special, for completion sake, here is it:

``` html
	<form id="contactForm" role="form">
		<label for="name">Name</label>
		<input type="text" class="form-control" id="name">

		<label for="email">Email</label>
		<input type="email" class="form-control" id="email">

		<label for="website">Website</label>
		<input type="url" class="form-control" id="website">

		<label for="message">Message</label>
		<textarea rows="3" class="form-control" id="message"></textarea>

		<input type="hidden" name="save" value="contact">
			<button type="submit">
				Send Message
			</button>
	</form>
```


The backend
===========

First of all you will need to signup in Parse and create an application. Once the app is created copy the APP ID and CLIENT KEY you will need it later.

The backend have two main functionalities: first save the message received from client then send an email with that message. For saving the message nothing needs to be done on the server, mailing will be done by Mailgun. If you don't have an account go to [Mailgun][1] and create one.


``` javascript
	var Mailgun = require('mailgun');
	Mailgun.initialize('myDomainName', 'myAPIKey');

	Parse.Cloud.beforeSave("Message", function(request, response){
		var msg = request.object;

		var body =
		"Name: " + msg.get("name") + "\n" +
		"Website: " + msg.get("website")+ "\n --- \n" +
		msg.get("message");

		Mailgun.sendEmail({
		  to: "hello@alexismas.com",
		  from: msg.get("email"),
		  subject: "Message from alexismas.com",
		  text: body
		}, {
		  success: function(httpResponse) {
			console.log(httpResponse);
			response.success();
		  },
		  error: function(httpResponse) {
			console.error(httpResponse);
			response.error("Uh oh, something went wrong");
		  }
		});
	});
```



The glue
========
The form needs to communicate somehow with the backend, nothing that a small script couldn't solve.

As I said before Parse is quite simple, in this case we need to create a Parse Object called Message. First we define the class name so we instantiate it. Once the object is created, we can fill it up with the data obtained from the form, and finally save it.

``` javascript
	var Message = Parse.Object.extend("Message");

	function saveMessage(){
		var msg = new Message();

		msg.set("name", $("#name").val());
		msg.set("email", $("#email").val());
		msg.set("website", $("#website").val());
		msg.set("message", $("#message").val());

		var callback = {
			success: function(){
				alert("Thanks!");
			},
			error: function(){
				alert(":S");
			}
		};

		msg.save(null, callback);
	}
```


The only missing part is how to execute this function when the form is submitted, that can be done in four lines.

``` javascript
	$("#contactForm").on("submit", function(e) {
		e.preventDefault();
		saveMessage();
	});
```

That's it.

[1]: https://mailgun.com/
[2]: https://parse.com/
[3]: http://en.wikipedia.org/wiki/Backend_as_a_service
