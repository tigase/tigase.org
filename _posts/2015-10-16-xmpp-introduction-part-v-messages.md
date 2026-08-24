---
title: XMPP - An Introduction Part V - Messages
tags:
  - server
  - xmpp-intro
  - pro-tip
date: "2015-10-16T22:40:32.169Z"
description: Understanding XMPP from beginner perspective - how can we actually chat?
categories:
  - blog
header:
  overlay_image: /assets/posts/xmpp-logo-460.png
---

Now that it is established who is online, and what capabilities they or the server has, it is now time to tackle the last stanza type for XMPP: Messages. Despite its lowly name, message stanzas can contain a huge variety of information, more than just a simple “hello”. Before we get that deep, however, let’s take a look at the general structure of a message stanza: in this case user 1 is sending user 2 a brief salutation.

```xml
<message from='user1@example.com/mobile' id='kaa2cv49' to='user2@example.com' type='chat' xml:lang='en'>
     <body>Hello, are you there?</body>
</message>
```

This is about as simple as you can get for an XMPP stanza, with two elements, `<message>` and a short `<body>`. There are some factors to unpack here however. For basics, we have from and to addresses, which is self-explanatory, and a unique ID to help keep track of the stanza. We also have an xml:lang property which will tell the server that, in this case, the messages are in English. Lastly there is a type of message, which carries some weight as to how it is sent. These attributes are optional, but are typically seen in use. Chat type messages are typically one-on-one messages between two users. Groupchat is typical of a message being sent to a MUC, although users may send groupchat messages elsewhere. An Error type messages is not typically sent from a user, but from the server or a client that contains some error messages. Headline messages are reserved for alerts where there is no expectation of a reply, think of a message of the day for instance. The default type, normal, serves as a standalone message without any history attached to it, but it is expected that a response will be made.

The `<body>` element, like the message type is also optional. As you can see, it contains the text portion of the message. By default, the message is xml data formatted for the language property specified. There can be many body elements, and many children of body elements which helps XMPP really flex its flexibility muscle. In this case we just have the text “hello, are you there?” a pretty simple message. Out of the box, XMPP supports two more elements: `<subject>` and `<thread>`. The `<subject>` element defines the subject for a conversation, although optional, it can help to differentiate different conversations and make searching through archives easier. Most clients, however, ignore the subject line. The `<thread>` element can also be used to identify associated conversations like the `<subject>` element. However, unlike the subject element, multiple messages can be part of the same thread, similar to how forum threads work. `<thread>` may be applied to normal or headline messages, as well as multi-user groupchat messages, and even machine communication for plugins or other non-human components. Messages may only have a single `<thread>` element, however they can have parent or child attributes that can attach them to other threads. Note that `<thread>` elements are not meant for human consumption as they are typically assigned a long and random value.

Now with the basic stuff out of the way, we can tackle what is known as extended content. Messages can be extended to have different formatting, custom content, forms, file transfers all as long as those children element are qualified by a namespace that is supported and defined by the server. That may sound like a lot, but it means that as long as the server is configured to handle it, messages can deliver it. Let’s say for example, we want to use HTML formatting in your messages so your messages can be richer, and even contain images. A basic message example would look like this:

```xml
<message from=’user2@example.com/mobile’ to=’user1@example.com’ type=’chat’>
     <body>I’m so mad I am red!</body>
          <html xmlns='http://jabber.org/protocol/xhtml-im'>
               <body xmlns='http://www.w3.org/1999/xhtml'>
               <p style='font-size:large'> I’m so<em>mad</em>I am<span style='color:red'>red</span>!</p>
              </body>
          </html>
</message>
```

We can now get some rich text from the sender. In our chat window, it should look like this:

I’m so mad, I am red!

A conversation between two users does not carry a lot of extra elements, and barring any special plugins or extensions, is very easy to follow when looking at the raw XML going between each user. See for yourself:

User1 sends:

```xml
<message type="chat" to="user2@example.com" id="aac9a">
     <body>Hello, are you free to meet at 12:00 today?</body>
</message>
```

User2 responds:

```xml
<message from="user2@example.com" type="chat" to="user1@example.com" id="aac1a">
     <body>No I can't, I will be at a lunch appointment. Can you meet at 5?</body>
</message>
```

User1 sends:

```xml
<message type="chat" to="user2@example.com" id="aacda">
     <body>Yes, I should be able to meet you then.</body>
</message>
```

User2 responds:

```xml
<message from="user2@example.com" type="chat" to="user1@example.com" id="aac3a">
     <body>Good, it's confirmed then. See you at 5.</body>
</message>
```
