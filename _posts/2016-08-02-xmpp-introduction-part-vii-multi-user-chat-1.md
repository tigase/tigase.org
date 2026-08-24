---
title: XMPP - An Introduction Part VII - Multi-User Chat (1)
tags:
  - server
  - xmpp-intro
  - pro-tip
date: "2016-08-02T22:40:32.169Z"
description: Understanding XMPP from beginner perspective - let's add more people to our conversation!
categories:
  - blog
header:
  overlay_image: /assets/posts/xmpp-logo-460.png
---

Now that we’ve seen some basics of an XMPP server on motion, it’s time to start looking at components. To start, we’re going to look at a fairly simple, but very important component: Multi-user chat or MUC. As its name implies, MUC provides a one-to-many type of message delivery, but it also provides a good look at how components can work inside an XMPP server.

To differentiate components from the core XMPP server, they are assigned a specific JID so stanzas go directly to the component. In MUC’s case, it is merely a domain. For this blog, let’s use muc.example.com. A JID is actually assigned when a room is made, so we should create a room and call it publicroom. That room can now be directly addressed by sending stanzas to publicroom@muc.example.com. To join the room, a directed presence stanza is sent to the room. Note that a resource is attached, this is your nickname within the chatroom and for interactions between users inside the chatroom will serve as your contact address or room JID.

```xml
<presence from="demouser@example.com" to="publicroom@muc.example.com/user1"/>
```

All the participants in the room will then receive a presence stanza that will notify them of the join. Note that the MUC component will send the presences to JIDs.

```xml
<presence from="publicroom@muc.example.com/user1" to="Jeffery@example.com"/>
<presence from="publicroom@muc.example.com/user1" to="Mike@example.com"/>
<presence from="publicroom@muc.example.com/user1" to="Donna@example.com"/>
```

These stanzas are sent from the MUC component to each user, so user1 will not see these outgoing messages.

The component will then send presence stanzas to user1 from all parties in the room.

```xml
<presence from="publicroom@muc.example.com/Jeffman" to="publicroom@muc.example.com/user1"/>
<presence from="publicroom@muc.example.com/Mikeymike" to="publicroom@muc.example.com/user1">
<presence from="publicroom@muc.example.com/DD" to="publicroom@muc.example.com/user1"
      <type="away"/>
</presence>
```

As you can see, all the users are identified by their Room JIDs and not their user JID.

Sending a message to the room, and subsequently all occupants in the room will look like this:

```xml
<message type="groupchat" to="publicroom@muc.example.com" id="af2ef">
    <body>hello</body>
</message>
```

Note that all messages sent to MUCs should be of the groupchat type. Message sent to the MUC will be rebroadcasted to all members of the room from the component.

```
<message type="groupchat" from="publicroom@muc.example.com/user1" to="publicroom@muc.example.com/Jeffman">
    <body>hello</body>
</mesage>

<message type="groupchat" from="publicroom@muc.example.com/user1" to="publicroom@muc.example.com/Mikeymike">
    <body>hello</body>
</mesage>

<message type="groupchat" from="publicroom@muc.example.com/user1" to="publicroom@muc.example.com/DD">
    <body>hello</body>
</mesage>

<message type="groupchat" from="publicroom@muc.example.com/user1" to="publicroom@muc.example.com/user1">
    <body>hello</body>
</mesage>
```

Since each recipient will only receive one stanza, each user will only see the stanza bound for them. Typically, MUC messages are live and contain no delay elements. Note that the sender will also receive the message.

It is also possible, upon connection, for the user logging in to receive prior chat history between participants that took place prior to login. These will show as groupchat messages with a timestamp under a delay element.

```xml
<message type="groupchat" from="publictoom@muc.example.com/Mikeymike" to="publicroom@muc.example.com/user1">
    <body>Quiet, somebody is coming!</body>
    <delay xmlns="urn:xmpp:delay" stamp="2016-07-22T13:50:02z"/>
</message>
```

Typically a client will arrange any previous messages according to the timestamp in chronological order. Note that this is an optional feature and may or may not occur with every instance. Also too, the number of messages shown is also configured by the server.

Next week, we’ll look at some security features in MUC, and roles different users can take.
