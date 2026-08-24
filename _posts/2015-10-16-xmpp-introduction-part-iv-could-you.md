---
title: XMPP - An Introduction Part IV - Could you…?
tags:
  - server
  - xmpp-intro
  - pro-tip
date: "2015-10-16T22:40:32.169Z"
description: Understanding XMPP from beginner perspective - what can you do for me?
categories:
  - blog
header:
  overlay_image: /assets/posts/xmpp-logo-460.png
---

Now for a hefty stanza, the IQ or Info/Query stanza. Of the three type of standard stanzas, this is perhaps the most critical. An IQ stanza can change server and user settings, provide detail on components and features, and can even carry capabilities for a particular client or server. Although there are many more uses for an IQ stanza, we will just cover some basics, structure, and what they mean. IQ stanzas provide for a request and response mechanism, something much like this conversation:

User: What kind of services do you provide?
Server: I have these services available: chat, multi-user chat, offline messages, vcard-temp, and user nicknames.

With that said, let’s see what that looks like in XMPP stanzas.

```xml
<iq type="get" to="xmpp.server.org" id="aad5a">
    <query xmlns="http://jabber.org/protocol/disco#info"/>
</iq>
```

This is a “get” type IQ stanza used for retrieving information. Note that this has been given an ID by the client, IDs are required for ALL IQ stanzas. Next it lists a query child element. Queries can come in a large number of varieties, each with their own individual xmlns or XML Namespace. This allows any part of an XMPP server to differentiate itself from other similar components. In this case, we are invoking the disco#info namespace which will provide us basic information from the server and top level components. The server then responds with the following:

```xml
<iq from="xmpp.server.org" type="result" id="aad5a" to="user@xmpp.server.org">
    <query xmlns="http://jabber.org/protocol/disco#info">
        <identity category="server" type="im" name="Tigase ver. 7.1.0-SNAPSHOT-b4049/a774f326 (2015-10-08/16:02:20)"/>
        <feature var=”jabber:client”/>
        <feature var=”http://jabber.org/protocol/muc”/>
        <feature var=”http://jabber.org/protocol/offline”/>
        <feature var=”vcard-temp”/>
        <feature var=”http://jabber.org/protocol/nick/”/>
    </query>
</iq>
```

As seen in the last stanza, it’s an IQ stanza of a result type, which means it’s a response designated for the query id aad5a. There is one child for this stanza, Query, and it has multiple children for this stanza. First child repeats the query with its XML namespace. Its children include one for listing the server information, and a number of feature children outlining the basic features or components the server has to offer. Typically, a server will have a lot more features, but for simplicity’s sake, we have only included a few basic components. This is a very basic example of the request and response mechanism, and can continue in a similar fashion if a user decides to ask for more information about a particular feature, component, or user.

The second type of IQ stanza is the set type, which as its name implies, is used to change settings, nicknames, or other information. Let’s say user@xmpp.server.org does not want to just be user, maybe he is a cool user and wants to set his nickname to let everybody else on the server know. So to set a nickname, a set type IQ stanza is sent to the server like the following:

```xml
<iq type="set" id="aac1a">
    <vCard xmlns="vcard-temp">
        <NICKNAME>COOL USER</NICKNAME>
    </vCard>
</iq>
```

The response from the server will be another result stanza, but in this case it does not contain any specific information, just a blank result meaning that the set was successful.

```xml
<iq type="result" id="aac1a" to="user@xmpp.server.org"/>
```

The last IQ stanza type available is the error type. These are errors attributed specifically to an IQ stanzas and can be in response to requesting a feature that is not implemented, or a setting that returns an error. IQ Error stanzs do not, for example, report undeliverable messages, or an invalid presence type. Let’s have a look at the above server, and say that the user wants to know more information about user blocking, which is not listed above.

```xml
<iq type='get' id='blocklist1'>
  <blocklist xmlns='urn:xmpp:blocking'/>
</iq>
```

The server will return an IQ error.:

```xml
<iq type="error" id="blocklist1" to="user@xmpp.server.org">
    <blocklist xmlns="urn:xmpp:blocking"/>
    <error type="cancel" code="501">
        <feature-not-implemented xmlns="urn:ietf:params:xml:ns:xmpp-stanzas"/>
        <text xmlns="urn:ietf:params:xml:ns:xmpp-stanzas" xml:lang="en">Feature not supported yet.</text>
    </error>
</iq>
```

Although the details of the particular error are custom, the IQ stanza results with an error type to the specific ID, and then proceeds to list what caused the error, and information that is attached. In this case the server reports that that feature is not implemented yet, and is unavailable for use.

There are a few general rules to keep in mind about IQ stanzas as provided by RFC#3921. These may be more for keeping in the standard, but it also helps understand the expected flow.

1. Id attribute is required for ALL IQ stanzas.
2. Type attribute is required for all IQ stanzas (either get, set, result or error).
3. IQ stanzas with get or set type must be responded to with an IQ stanza with result or error types
4. IQ stanzas with error or result types should not generate a response unless it is another get or set stanza.
5. IQ stanzas with get or set types must contain one child element that satisfies the semantics of that element. (i.e. you may not send a single get stanza for disco#info for multiple components.)
6. An IQ stanza must include 0-1 child elements. (that child may have more children elements)
7. Error IQ stanzas should include the child element associated with the get or set and must include an `<error/>` child.

For more specific information from the XMPP foundation, have a look at these links:

- [Extensible Messaging and Presence Protocol (XMPP): Core](http://xmpp.org/rfcs/rfc6120.html)
- [Extensible Messaging and Presence Protocol (XMPP): Instant Messaging and Presence])(http://xmpp.org/rfcs/rfc6121.html)
- [Jabber/XMPP Protocol Namespaces](http://xmpp.org/registrar/namespaces.html)
