---
title: A look at Halcyon 
tags:
  - client
  - library
  - halcyon
  - kotlin
  - kotlin-multiplatform
  - native
  - js
  - javascript
  - android
date: "2020-09-02"
description: New XMPP library written in Kotlin with multiplatform goals.
categories:
  - blog
header:
  overlay_image: /assets/posts/halcyon-idea.png
---
New XMPP library written in Kotlin with multiplatform goals.

# A look at Halcyon

Some time ago, we started developing multiplatform XMPP Library called [Halcyon](https://github.com/tigase/halcyon) based on [Kotlin Multiplatform](https://kotlinlang.org/docs/reference/multiplatform.html) by [Jetbrains](https://www.jetbrains.com/).
Our plan is to allow using the same library in different target environments: JVM, Android, JavaScript and Native.
Currently we are focused on JVM and JavaScript targets.

In this post we will try to show library design and example of usage.

![halcyon-idea]({{ site.url }}{{ site.baseurl }}/assets/posts/halcyon-idea.png)

## Before you start

Because Halcyon isn't published in any Maven repository (yet), you need to compile it yourself. We believe, it will not be a problem.
The only two things you need to do is to clone repository and compile library:

```shell script
git clone https://github.com/tigase/halcyon.git
cd halcyon
./gradlew publishToMavenLocal
```

Thats all. Now Halcyon is in your local Maven repository.

## Let's do something

We recommend using Gradle to build everything (except for towers and bridges maybe). You can also use Maven, it doesn't matter. Just use one of them, to prevent problems with dependencies. 
Here is sample `build.gradle.kts` file, the most important this is to enable kotlin plugin and include Hayclon in the list of dependencies:

```kotlin
plugins {
    java
    kotlin("jvm") version "1.3.61"
}

repositories {
    mavenLocal()
    jcenter()
    mavenCentral()
}

dependencies {
    implementation(kotlin("stdlib-jdk8"))
    implementation("tigase.halcyon:halcyon-core-jvm:0.0.1")
    testCompile("junit", "junit", "4.12")
}

configure<JavaPluginConvention> {
    sourceCompatibility = JavaVersion.VERSION_1_8
}
tasks {
    compileKotlin {
        kotlinOptions.jvmTarget = "1.8"
    }
    compileTestKotlin {
        kotlinOptions.jvmTarget = "1.8"
    }
}
``` 

Let's add some Kotlin code:

```kotlin
fun main(args: Array<String>) {
    val client = Halcyon()
    client.configure {
        userJID = "user@sampleserver.org".toBareJID()
        password = "secret"
    }
    client.connectAndWait()
    client.disconnect()
}
```

This simple code creates XMPP client, connects to XMPP server and then disconnects.

To show how to work with Halcyon library, we will by adding code to this small code base.

## Events

Halcyon is events-driven library. It means, that each part of library may publish event to *event bus* and all registered listeners will receive it.

Lets add some code to see what is being send and received over XMPP stream:

```kotlin
client.eventBus.register<ReceivedXMLElementEvent>(ReceivedXMLElementEvent.TYPE) { event ->
    println(">> ${event.element.getAsString()}")
}
client.eventBus.register<SentXMLElementEvent>(SentXMLElementEvent.TYPE) { event ->
    println("<< ${event.element.getAsString()}")
}
```  

To listen for all events since the connection is started, we have to add this code before `client.connectAndWait()`.

All events extend class `tigase.halcyon.core.eventbus.Event`, so you can easily find them all in your favourite IDE.

Each module may have it's own set of events, so please check documentation or source code of modules of interest.

## Request

Now we will look at one of the most interesting things in XMPP: requests and responses.

XMPP protocol allows sending request to another entity and receive response.
Why is it so exciting? Because we can ping other clients, or ask for their local time!
Ok, stop joking. Of course above examples are true, but with request-response we can do much more than simple sending messages: we can manage our contacts list, we can manage Multi User Chatrooms, we can execute remote methods on server or other clients.

As an example we will ping other XMPP entity (it may be server or other client).
First we need to get `PingModule` to be able to use its request builder:

```kotlin
val pingModule = client.getModule<PingModule>(PingModule.TYPE)!!
```

Ping module has method `ping()` which creates a **request builder** (note, that it doesn't send anything yet!) configured to understand response and return it as object.
Method `ping()` has optional parameter `jid`. If is not provided, then ping will be send to the server to which the client is connected. 

```kotlin
val request = pingModule.ping()
request.response { result ->
    when (result) {
        is IQResult.Success -> println("Pong in ${result.get()!!.time} ms")
        is IQResult.Error -> println("Oops! Error ${result.error}")
    }
}
request.send()
```

There is also a different way to add response handler to the request:

```kotlin
request.handle {
    success { request, iq, result -> println("Pong in ${result!!.time} ms") }
    error { request, iq, errorCondition, message -> println("Oops! Error $errorCondition") }
}
```

Use the one that you prefer.

One more example: we will check list of features of our server:

```kotlin
val discoveryModule = client.getModule<DiscoveryModule>(DiscoveryModule.TYPE)!!
discoveryModule.info("sampleserver.org".toJID()).handle {
    error { request, iq, errorCondition, message -> println("Oops! Error $errorCondition") }
    success { request, iq, result ->
        println("Server JID: ${result!!.jid}")
        println("Features:")
        result!!.features.forEach { println(" - $it") }
    }
}.send()
```

## Messages

This chapter will be very hard, mostly because `MessageModule` isn't finished yet.
We haven't made a design decision yet - how this module should work. It is good for you though, because we can create message stanza from scratch! And it's cool!

This is how message stanza look like:

```xml
<message
      from='juliet@example.com/balcony'
      id='ktx72v49'
      to='romeo@example.net'>
    <body>Art thou not Romeo, and a Montague?</body>
</message>
```

Let's try to create this stanza in Kotlin and send it.

```kotlin
var messageRequest = client.request.message {
    to = "romeo@example.net".toJID()
    body = "Art thou not Romeo, and a Montague?"
}
messageRequest.send()
```

The only thing currently implemented in `MessageModule` is `MessageReceivedEvent`, useful to handle all incoming message stanzas:

```kotlin
client.eventBus.register<MessageReceivedEvent>(MessageReceivedEvent.TYPE) { event ->
    println("Message from ${event.fromJID}: ${event.stanza.body}")
}
```

## Roster and presence

Ok, we can send a message to anybody, but most of the time we want to send them to our friends.
We need a list of our friends. Luckily such list is available out-of-box in XMPP protocol: it is called Roster.

It shouldn't be a surprise, but to manage your roster you need `RosterModule`:

```kotlin
var rosterModule = client.getModule<RosterModule>(RosterModule.TYPE)!!
```

We can add (or update, with the same method) roster items, remove and list them.

```kotlin
val allRosterItems = rosterModule.store.getAllItems()
```

`RosterItem` contains JabberID of the contact, list of groups being assigned to, status of subscription (if contact is allowed to see our presence or not, and if we are allowed to see it's presence).

Presence is "status of contact". You can see if your contacts are online, offline or maybe you shouldn't send any message to someone because he has "Do Not Disturb" status.

As an example, we will list all contacts from the roster and their presence:

```kotlin
rosterModule.store.getAllItems().forEach { rosterItem ->
    val presenceStanza = presenceModule.getBestPresenceOf(rosterItem.jid)
    println("${rosterItem.name} <${rosterItem.jid}> : ${presenceStanza?.show ?: "Offline"}")
}
```

## Thanks… 

…for being here up to this point.
We hope you enjoyed reading about Halcyon library, and you liked it even though it is not finished yet.

Please share you thoughts and ideas at our group chat [tigase@muc.tigase.org](xmpp:tigase@muc.tigase.org?join) or on [library GitHub page](https://github.com/tigase/halcyon). 