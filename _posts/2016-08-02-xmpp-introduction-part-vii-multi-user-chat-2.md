---
title: XMPP - An Introduction Part VII - Multi-User Chat (2)
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

An XMPP Server Administrator can create rooms, moderate them, close them and really manipulate any aspects of the room. However a single Administrator cannot maintain every chatroom on a server by themselves, and may need the help of other users to moderate individual chatrooms or larger sections. For this purpose, each chatroom has a number of affiliations and roles that be set for room jids. MUC divides privileges based on which role and/or affiliation is assigned rather than having to assign those privileges individually. Keep in mind that Affiliations are long term and persistent, whereas Roles are temporary and do not last longer than a user"s session.

A room can be created by sending a presence to the room you want to create:

```xml
<presence to="chatroom@muc.example.com/Admin" />
```

The room Chatroom will be created, and in this chatroom you are self-assigning the nickname Admin. As seen in the last blog, the room jid will now be `chatroom@muc.example.com/Admin`. The server then responds with the room information in a presence statement:

```xml
<presence from="chatroom@muc.example.com/Admin" to="demouser@example.com">
    <item affiliation="owner" nick="Admin" role="moderator" jid="demouser@example.com"/>
    <status code="110"/>
</presence>
```

This statement introduces us to affiliations and roles. Nick, is just the nickname given to the JID to create the room jid. A user can have one of a set list of affiliations; Owner, Admin, Member, Outcast, or None. Since `demouser@example.com` created the room, he is assigned the owner affiliation. Owners are given the greatest amount of control for a MUC room, and there can be more than one owner for a room, however an Owner cannot not have a second affiliation for the same room. The table here shows the various privileges for each affiliation as per the XEP, note that these are default layouts, and different MUC variations can customize these privileges.

| **Privilege**                      | **Outcast** | **None** | **Member** | **Admin** | **Owner** |
| ---------------------------------- | ----------- | -------- | ---------- | --------- | --------- |
| Enter Open Room                    |             | X        | X          | X         | X         |
| Register with Open Room            |             | X        |            |           |           |
| Retrieve Member List               |             |          | X          | X         | X         |
| Enter Member-only Room             |             |          | X          | X         | X         |
| Ban Members and Unaffiliated Users |             |          |            | X         | X         |
| Edit Member List                   |             |          |            | X         | X         |
| Assign and Remove Moderators       |             |          |            | X         | X         |
| Edit Admin List                    |             |          |            |           | X         |
| Edit Owner List                    |             |          |            |           | X         |
| Change Room Config                 |             |          |            |           | X         |
| Destroy Room                       |             |          |            |           | X         |

Outcast affiliations are essentially bans from a room where a particular user cannot interact with the room they are banned from at all. Again, these affiliations are preserved during the life of the room, so users disconnecting from the room will reconnect and maintain the same affiliation. Roles however are more ephemeral and need to be assigned or reassigned upon each connection. Although they are short term, the privileges are still important to review as they can severely affect a rooms operation. There are four roles that a user can take in a room; None, Visitor, Participant, and Moderator.

| **Privilege**             | **None** | **Visitor** | **Participant** | **Moderator** |
| ------------------------- | -------- | ----------- | --------------- | ------------- |
| Present in Room           |          | X           | X               | X             |
| Receive Messages          |          | X           | X               | X             |
| Receive Occupant Presence |          | X           | X               | X             |
| Broadcast Presence        |          | X           | X               | X             |
| Change Availability       |          | X           | X               | X             |
| Change Room Nickname      |          | X           | X               | X             |
| Send Private Messages     |          |             | X               | X             |
| Invite Other Users        |          |             | X               | X             |
| Send Messages             |          |             | X               | X             |
| Modify Subject            |          |             |                 | X             |
| Kick Users                |          |             |                 | X             |
| Grant Voice               |          |             |                 | X             |
| Revoke Voice              |          |             |                 | X             |

Although it may seem unnecessary to list no affiliation, or unaffiliated status for rooms, it is still a specific role used by the MUC protocol. If a user is leaving a room or is kicked they get the unaffiliated role which may contribute to other triggers or server behaviors. This is also part of how a user leaves a room as in this example when the user Jeff leaves the chatroom created above:

```xml
<presence type="unavailable" to="[chatroom@muc.example.com](mailto:chatroom@muc.example.com)/Jeff"/>

<presence from="chatroom@muc.example.com/Jeff" type="unavailable" to="Jeff@example.com">
    <item affiliation="none" nick="Jeff" role="none"/>
    <status code="110"/>
</presence>
```

Note that both affiliation AND roles are set to none on exit, this will be the same for users of any role or affiliation.

Forcing a change in roles to unruly users can be done using an iq stanza sent to the chatroom. Let"s say Jeff is being a nuisance and is dragging the conversation off topic, the admin can force him as a visitor to effectively silence him for a period of time.

```xml
<iq type="set" to="chatroom@muc.example.com" id="aacaa">
    <query xmlns="http://jabber.org/protocol/muc#admin"/>
        <item nick="Jeff" role="visitor"/>
    </query>
</iq>
```

Like any IQ Stanza, a blank result IQ stanza is sent back to confirm this change.

```xml
<iq from="chatroom@muc.example.com" type="result" id="aacaa" to="admin@example.com"/>
```

The server will send presence stanzas to update all users of the new role in the chatroom. As a visitor to the chatroom, Jeff can only see messages being sent back and forth, but is effectively silent. If things get particularly heated, Jeff might be banned from the chatroom, which involves changing the affiliation using a similar IQ stanza however the ban must be based on the bare JID of the user, not the room jid.

```xml
<iq type="set" to="chatroom@muc.example.com" id="ban1">
    <query xmlns="http://jabber.org/protocol/muc#admin"/>
        <item affiliation="outcast" jid="Jeff@example.com"/>
    </query>
</iq>
```

The server will send back a result stanza

```xml
<iq from="chatroom@muc.example.com" type="result" id="ban1" to="admin@example.com"/>
```

If Jeff is in the room, he will be kicked and will be unable to rejoin and will be given a not-authorized stanza. If admin decides to let Jeff rejoin, the affiliation can be changed in a similar manner by restoring their affiliation to none or above.

MUC has a far greater number of uses rather than just group chatting, but our scope is not wide enough to encompass all the uses MUC can serve. For more information on the MUC components refer to the XEP protocol for MUC.
