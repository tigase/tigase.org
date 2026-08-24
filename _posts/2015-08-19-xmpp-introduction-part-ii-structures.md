---
title: XMPP - An Introduction Part II - Structures
tags:
  - server
  - xmpp-intro
  - pro-tip
date: "2015-08-19T22:40:32.169Z"
description: Understanding XMPP from beginner perspective - what are the basic entities used to communicate?
categories:
  - blog
header:
  overlay_image: /assets/posts/xmpp-logo-460.png
---

Every structure needs a foundation, and before I could spend time seeing what XMPP can do, it was best to find out how XMPP works. You may find that after looking at some basic message data, XMPP communication is framed much like the way an HTML page would look. In fact, by taking a look at the raw information stream going between a client and the server (or vice-versa) it is surprisingly readable! Be forewarned, there will be some code here, but it’s easily digestible, and there will be terms you might not have seen yet, but I’ll try to cover them without getting too long winded. Here the idea is to keep it about structure.

When a client attempts to connect to an XMPP server, two streams of data are opened, one for incoming and one for outgoing. These two connections enable duplex communications, meaning that two parties can send and receive at the same time. Although this means that for every user two connections must be made, you won’t have to wait for transmission to stop before you begin communication in a simplex system like HTTP. Let’s look at some of the messaging between a client and a server as they two become connected.

We send:

```xml
<?xml version="1.0"?>

<stream:stream xmlns:stream="http://etherx.jabber.org/streams"version="1.0"xmlns="jabber:client" to="tigase.org" xml:lang="en"xmlns:xml="http://www.w3.org/XML/1998/namespace">
```

Server responds with:

```xml
<?xml version='1.0'?>

<stream:stream xmlns='jabber:client'xmlns:stream='http://etherx.jabber.org/streams' from='tigase.org'id=’9a44633a-acd1-4db8-90d4-b401d99e70a8’ version='1.0' xml:lang='en'>
```

Here almost all information is between `<>` characters, and basically states some simple information. First the client states that it is using XML version 1.0, that it wishes to open up streams, and to whom it will open those streams up. The server response is more or less an acknowledgement. Then the server gives some details about its features.

```xml
<stream:features>
    <auth xmlns="http://jabber.org/features/iq-auth"/>
    <register xmlns="http://jabber.org/features/iq-register"/>
    <mechanisms xmlns="urn:ietf:params:xml:ns:xmpp-sasl">
        <mechanism>PLAIN</mechanism>
        <mechanism>ANONYMOUS</mechanism>
    </mechanisms>
    <ver xmlns="urn:xmpp:features:rosterver"/>
    <compression xmlns="http://jabber.org/features/compress">
        <method>zlib</method>
    </compression>
</stream:features>
```

Now things are starting to look like HTML somewhat. The server will list its features and perhaps some details of each. Note that single-line or single field parameters are in a single `<>`. For example, the register feature is listed as `<register xmlns=http://jabber.org/features/iq-register/>`. This lists the XML Namespace as well keeping in line with the protocol format. [Although that URL may no longer work (as the move to XMPP and XEP standards have changed) it is still considered a valid namespace.]

Next the Compression feature is listed with multiple lines:

```xml
<compression xmlns='http://jabber.org/features/compress'>
    <method>zlib</method>
</compression>
```

The Compression tag lists that data stream is supported, and lists the sub-tags of zlib for the type of compression. Note that each separate tag must have a `/` at the end. If there is no corresponding end to each tag, the server will throw a malformed XML error stopping the flow of data.

From this short exchange we can see that communications are short (in this case) XML snippets containing relevant tags and fields that both the server and client can interpret. Not only this, but by adding tags and data it can be extended to include almost anything!
