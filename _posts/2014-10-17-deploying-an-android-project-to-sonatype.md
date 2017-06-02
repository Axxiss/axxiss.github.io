---
layout: post
title: "Deploying an Android project to Sonatype Nexus"
date: 2014-10-17 21:20:00 -0200
comments: true
tags: Android
---
When we try to deploy an Android to Sonatype Nexus using [Gradle Sonatype Nexus][1] plugin, we will encounter (at least in my case) two errors.

The first one is because nexus plugin can't find `install` task on the project.

    A problem occurred configuring project ':app'.
    > Failed to notify project evaluation listener.
       > Task with name 'install' not found in project ':app'.
       > Could not find property 'poms' on project ':app'.

And the second one seems to be that plugin can't find where source are located.

    A problem occurred configuring project ':library'.
    > Failed to notify project evaluation listener.
        > Could not find property 'main' on SourceSet container.
        > Task with name 'install' not found in project ':library'.
        > Could not find property 'poms' on project ':library'.


The easiest solution I have found by now, is using Sonatype Nexus plugin in combination with [Gradle Android Maven][2] plugin.

First we need to add both plugins to the buildscript closure


``` groovy
buildscript {
    repositories {
        mavenCentral()
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:0.13.2'
        classpath 'org.gradle.api.plugins:gradle-nexus-plugin:0.7.1'
        classpath 'com.github.dcendents:android-maven-plugin:1.1'
    }
}
```


Now we're ready to go, just apply both plugins and define sources location.


``` groovy
    apply plugin: 'android-maven'
    apply plugin: 'nexus'

    sourceSets {
        main {
            java.srcDirs = ['src/main/java']
        }
    }
```

Finally pom's data needs to be defined, you can find a sample on Gradle Sonatype Nexus page. For more details on how to configure each plugin, please go the their respective pages.


[1]: https://github.com/bmuschko/gradle-nexus-plugin
[2]: https://github.com/dcendents/android-maven-plugin
