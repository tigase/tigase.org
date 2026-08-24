---
title: XMPP - An Introduction Part III - Establishing a Presence
tags:
  - server
  - xmpp-intro
  - pro-tip
date: "2015-09-10T22:40:32.169Z"
description: Understanding XMPP from beginner perspective - how to let the world we want to talk?
categories:
  - blog
header:
  overlay_image: /assets/posts/xmpp-logo-460.png
---

Now that there’s an idea what the structure looks like for communication to and from servers and clients, we need to classify them. Once a stream is established, XMPP recognizes three standard types of messages; Presence, IQ, and Message. Let’s start with the Presence stanzas.

Presence stanzas are small stanzas that contain basic presence information. What this means, is that presence stanzas are sent between clients and servers designating the status of the user, room, or other entity to all interested persons. This may need further unpacking; when a user signs on that users client will send an online presence to anybody on his roster who would want to know he is online.

```xml
<presence from="userA@tigase.org" to="userB@tigase.org">
    <priority>5</priority>
</presence>
```

Note that that the online and available status is not explicit, but implied in this stanza. User B now knows that user A is online. In return, user B’s client will send its own presence back to user A sending his status along with it.

```xml
<presence from="userB@tigase.org/mobile" to="userA@tigase.org">
    <show>away</show>
    <status>I’m busy on my phone</status>
    <priority>5</priority>
</presence>
```

From what we can see here, is that user B is on his phone, is away, and has left a custom status message. User A’s client will show user B as away. There are a few established <show> types that Tigase, and XMPP use by default:

- `<chat>` - Setting to say user is available for chat.
- `<away>` - As seen earlier, user is temporarily away.
- `<xa>` - Extended away to say a user will not be back for a long time.
- `<dnd>` - The user does not wish to be disturbed.

Finally, if a user wishes to go offline, they send an <unavailable> status to the server.

```xml
<presence type="unavailable" />
```

When presences are sent to the server from a client, the server will then repeat those presence statuses to all JIDs listed on the users roster. As you can guess, the more people online and on a users’ roster, the more traffic that will generate when a user comes on. This behavior will also be duplicated for any multi-user chat rooms that the user might be in. Each user in the room will get another presence message from the room in addition to the one sent from the server. It will look similar, but have some key differences.

```xml
<presence from="chatroom@muc.tigase.org/userB"
          type="unavailable" to="userA@tigase.org">
    <x xmlns="http://jabber.org/protocol/muc#user">
        <item affiliation="none" nick="User B" role="participant"
              jid="userB@tigase.org/mobile/"/>
    </x>
</presence>
```

Mainly that the XML name space is different, and the users’ role, affiliation, and nickname in the chat room are included in the stanza. Like many portions of XMPP, presence stanzas can deliver information like GPS locations, current songs you are listening too, or really any relevant information from the client. Keep in mind however, that a presence packet is not meant to serve in place of other stanzas and, in best practices, should not carry a large amount of information to keep the bandwidth load low.
