---
title: XMPP - An Introduction Part VI - Service Discovery
tags:
  - server
  - xmpp-intro
  - pro-tip
date: "2016-03-25T22:40:32.169Z"
description: Understanding XMPP from beginner perspective - how servers know what is available?
categories:
  - blog
header:
  overlay_image: /assets/posts/xmpp-logo-460.png
---

Now that we’ve covered the basic stanza forms, and how we can communicate with XMPP servers and other clients, it’s time to dig deeper. Previously, in the IQ stanza blog, we dove a little bit into service discovery. Here we will look at the service discovery tree in some more detail, exploiting its capabilities and using it to find out who is online, and what services are available.

Service discovery in XMPP comes in two flavors: disco#info and disco#items. Disco#info provides information about the entity in question like software version, or available features. Disco#items provides a list of available items, such as supported features, or members in a chat room. These two XML Namespaces can help you navigate the tree of information from an XMPP server. We’ve already seen what happens when you query the main server with a disco#info stanza, so let’s see what happens when use a disco#items at a MUC component:

```xml
<iq type="get" to="muc.example.com" id="ab17a">
    <query xmlns="http://jabber.org/protocol/disco#items"/>
</iq>
```

The server responds with a list of available and active chatrooms.

```xml
<iq from="muc.example.com" type="result" to="user1@example.com" id="ab17a">
    <query xmlns="http://jabber.org/protocol/disco#items">
        <item name="" jid="chatroom@muc.example.com"/>
        <item name="" jid="adminsonly@muc.example.com"/>
        <item name="" jid="openchat@muc.example.com"/>
        <item name="" jid="chatroom2@muc.example.com"/>
    </query>
</iq>
```

Say we want to find information about openchat chatroom, we send a disco#info IQ stanza to that JID, openchat@muc.example.com:

```xml
<iq type="get" to="chatroom@muc.example.com" id="aaeda">
    <query xmlns="http://jabber.org/protocol/disco#info"/>
</iq>
```

The server then responds with settings for the room, the results here have been truncated:

```xml
<iq from="chatroom@muc.example.com" type="result" id="aaeda" to="user1@example.com">
     <query xmlns="http://jabber.org/protocol/disco#info">
         <identity category="conference" type="text" name=""/>
         <feature var="http://jabber.org/protocol/muc"/>
         <feature var="muc_semianonymous"/>
         <feature var="muc_open"/>
         <feature var="muc_public"/>
         <feature var="muc_unsecured"/>
         <x xmlns="jabber:x:data" type="result">
             <field type="FORM_TYPE" var="hidden">
                 <value>http://jabber.org/protocol/muc#roominfo</value>
             </field>
             <field type="muc#roominfo_occupants" label="Number of occupants">
                 <value>7</value>
             </field>
             <field type="muc#roominfo_subject" label="Current discussion topic">
                 <value>Open Discussion</value>
             </field>
             <field type="muc#roomconfig_allowinvites" label="Whether occupants allowed to invite others">
                 <value>1</value>
             </field>
             <field type="muc#roomconfig_changesubject" label="Whether occupants may change the subject">
                 <value>0</value>
             </field>
             <field type="muc#roomconfig_enablelogging" label="Whether logging is enabled">
                 <value>0</value>
             </field>
             <field type="muc#roomconfig_moderatedroom" label="Whether room is moderated">
                 <value>1</value>
             </field>
             <field type="muc#roomconfig_passwordprotectedroom" label="Whether a password is required to enter">
                 <value>0</value>
             </field>
             <field type="muc#roomconfig_presencebroadcast" label="Roles for which presence is broadcast">
                 <value>moderator</value>
                 <value>participant</value>
                 <value>visitor</value>
             </field>
             <field type="muc#roomconfig_publicroom" label="Whether room is publicly searchable">
                 <value>1</value>
             </field>
             <field type="muc#roomconfig_roomowners" label="Full list of room owners">
                 <value>admin@example.com</value>
             </field>
         </x>
     </query>
 </iq>
```

This list gives us a good amount of information about the room from disco#info. Now, when we send a disco#items stanza to the same muc address…

```xml
<iq type="get" to="chatroom@muc.example.com" id="ab1fa">
     <query xmlns="http://jabber.org/protocol/disco#items"/>
 </iq>
```

The result is a list of users:

```xml
<iq from="chatroom@muc.example.com" type="result" id="ab1fa" to="user1@example.com">
     <query xmlns="http://jabber.org/protocol/disco#items">
         <item name="Admin" jid="chatroom@muc.example.com/Admin"/>
         <item name="Joe" jid="chatroom@muc.example.com/Joe"/>
         <item name="Bob" jid="chatroom@muc.example.com/Bob"/>
         <item name="Wojciech" jid="chatroom@muc.example.com/Wojciech"/>
         <item name="Artur" jid="chatroom@muc.example.com/Artur"/>
         <item name="Andrzej" jid="chatroom@muc.example.com/Andrzej"/>
         <item name="Eric" jid="chatroom@muc.example.com/Eric"/>
     </query>
 </iq>
```

Notice however, that usernames are not shown as bare JIDs, but as full JIDs as a part of muc component, which is because when users join a chatroom, they are given an alias from the chatroom. Typically, you will not be able to retrieve a user’s JID from muc component. As we have seen, the disco#info and disco#item XML namespaces can be incredibly handy for navigating the tree of information in an XMPP server. Similar exchanges can be seen for discovering nodes and items in PubSub components, finding available commands and features for components, or potential tasks components can carry out.
