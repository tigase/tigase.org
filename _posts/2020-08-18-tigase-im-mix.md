---
title: xmpp.cloud just got even better and we are the only XMPP provider with future of XMPP group chat (MIX) 
tags:
  - server
  - xmpp.cloud
  - tigase.im
  - MIX
  - groupchat
date: "2020-08-18"
description: Tigase public XMPP services are the only one that offer MIX - state of the art approach to groupchat messages! Take advantage of it by using xmpp.cloud with our clients - BeagleIM and SiskinIM
categories:
  - blog
header:
  overlay_image: /assets/posts/mix-create-2.png
---
Tigase public XMPP services are the only one that offer MIX - state of the art approach to groupchat messages! Take 
advantage of it by using xmpp.cloud with our clients - BeagleIM and SiskinIM.

Our **free**, public XMPP installation: [`xmpp.cloud`](https://xmpp.cloud) is packed with features: domain hosting, file uploads, TURN/STUN server for your audio/video calls, high availability due to running as a cluster with automatic recovery and all of this while scoring 100% XMPP compliance and perfect A grade in a popular test tools. And now it got even **better**!  

# Group chat - the problem

For a very, very long time `MUC` (defined in [XEP-0045: Multi-User Chat](https://xmpp.org/extensions/xep-0045.html)) was the only option available in XMPP realm that allowed people to exchange messages in a group. At the beginning, when it was typical to have constant connection to the server, everything was working just fine, but over the course of time new challenges arose. People started to use more and more their mobile devices and connections to the server became anything but permanent (be that because of spotty internet connection or limitations imposed by mobile device manufacturer). In that environment `MUC` started to show it's architectural problems: if you are not connected you are not an occupant of the MUC room and therefore you don't receive any messages. This was a problem! 

# Previous attempts to improve the situation

There a couple of attempts to mitigate this situation: [XEP-0410: MUC Self-Ping (Schrödinger's Chat)](https://xmpp.org/extensions/xep-0410.html) for the situations where federation link would get broken and user would be unaware that it's not in a room anymore, [Multi-User Chat Light](https://xmpp.org/extensions/inbox/muc-light.html), which would allow to permanently register to the room without presence or vendor specific non XSF variations of "MUC subscription". There were also workarounds with with long-lived user sessions (terminating underlying network connection without closing XML stream) that were quite inefficient.

# What is XMPP MIX

`MIX` stands for `Mediated Information eXchange (MIX)` and it's basics are defined in [XEP-0369: Mediated Information eXchange (MIX)](https://xmpp.org/extensions/xep-0369.html):
> "an XMPP protocol extension for the exchange of information among multiple users through a mediating service. The protocol can be used to provide human group communication and communication between non-human entities using channels, although with greater flexibility and extensibility than existing groupchat technologies such as Multi-User Chat (MUC). MIX uses Publish-Subscribe to provide flexible access and publication, and uses Message Archive Management (MAM) to provide storage and archiving."

Specification outlines several [requirements](https://xmpp.org/extensions/xep-0369.html#reqs) of which those seems to be the most interesting:
* "A user's participation in a channel persists and is not modified by the user's client going online and offline."
* "Multiple devices associated with the same account can share the same nick in the channel, with well-defined rules making each client individually addressable."
* "A reconnecting client can quickly resync with respect to messages and presence."

`MIX` itself serves as an umbrella for set of MIX-related XMPP extensions that specify the exact protocol. Two of them are required for the implementation to be considered as MIX compliant:
* `MIX-CORE` defined in [XEP-0369: Mediated Information eXchange (MIX)](https://xmpp.org/extensions/xep-0369.html) - "sets out requirements addressed by MIX and general MIX concepts and framework. It defines joining channels and associated participant management. It defines discovery and sharing of MIX channels and information about them. It defines use of MIX to share messages with channel participants."
* `MIX-PAM` defined in [XEP-0405: Mediated Information eXchange (MIX): Participant Server Requirements](https://xmpp.org/extensions/xep-0405.html) - "defines how a server supporting MIX clients behaves, to support servers implementing MIX-CORE and MIX-PRESENCE."

In addition to the above extensions, there are several other that are optional:

* `MIX-PRESENCE` defined in [XEP-0403: Mediated Information eXchange (MIX): Presence Support](https://xmpp.org/extensions/xep-0403.html) - adds the ability for MIX online clients to share presence, so that this can be seen by other MIX clients. It also specifies relay of IQ stanzas through a channel.
* `MIX-ADMIN` defined in [XEP-0406: Mediated Information eXchange (MIX): MIX Administration](https://xmpp.org/extensions/xep-0406.html) - specifies MIX configuration and administration of MIX.
* `MIX-ANON` defined in [XEP-0404: Mediated Information eXchange (MIX): JID Hidden Channels](https://xmpp.org/extensions/xep-0404.html) - specifies a mechanism to hide real JIDs from MIX clients and related privacy controls. It also specifies private messages.
* `MIX-MISC` defined in [XEP-0407: Mediated Information eXchange (MIX): Miscellaneous Capabilities](https://xmpp.org/extensions/xep-0407.html) - specifies a number of small MIX capabilities which are useful but do not need to be a part of MIX-CORE: handling avatars, registration of nickname, retracting of a message, sharing information about channel and inviting people, converting simple chat to a channel.
* `MIX-MUC` defined in [XEP-0408: Mediated Information eXchange (MIX): Co-existence with MUC](https://xmpp.org/extensions/xep-0408.html) - defines how MIX and MUC can be used together.

## How does it work?

The most stark difference to MUC is that MIX requires support from both server that hosts the channel and user's server. This is done to facilitate the notion that the user (and not particular connection or client application) joined the group and allows for greater flexibility in terms of message delivery (which can be send to one or many connections, or even generates notification over PUSH). Another important difference is the flexibility to choose which notifications from the channel user wants to receive (that can be messages, presence, participators or node information).
In the most basic approach, when user decides to join a channel, it sends an IQ stanza to it's own local server indicating address of the desired channel and list of MIX nodes to which it wants to subscribe. User's server then forward's subscription request to the destination, MIX server. As a result user receives subscription confirmation and from this point onwards will receive notifications from the channel, independently of it's current network connection.
Another essential bit of MIX is the reliance on [XEP-0313: Message Archive Management](https://xmpp.org/extensions/xep-0313.html) to control message history and the complementary interaction between MIX server and user's server. Main channel history is handled by the MIX server, but user's that joined the channel will retrieve and synchronise message history querying their local server, which will maintain complete history of the channels that user has joined (based on the received notifications). This also means that even if the channel is removed, user is still able to access it's history through local MAM archive (limited to time when user was member of the channel).
As a result, chatter between client, client's server and mix server is also reduced and unnecessary traffic is eliminated.

## Benefits for mobile-first applications relying on push

All of this helps especially with clients that relay on constrained environment - be that unreliable network connection or operating system that limits time that application can be running. Because there is no dependency on the dynamic state of user presence/connection the issue with occupant leaving and (re)joining the room is eliminated - user gets the notification always. What's more, thanks to shared responsibilities between MIX and user's server, and the latter getting all notifications from MIX channel, it's possible to generate notifications without relaying on workarounds (that most of the time are not reliable or impact resource usage).

In case of Tigase XMPP server it gets better thanks to our experimental [filtering groupchat notifications](https://xeps.tigase.net/docs/push-notifications/filters/groupchat/) feature, which allows user controll when to receive PUSH notifications from group chats (always, only when mentioned or never)

## Is MUC obsolete?

We think that MIX is the way forward, but we also know that this won't happen overnight. Because of that MUC is still supported in all our applications and Tigase XMPP Server implements [XEP-0408: Mediated Information eXchange (MIX): Co-existence with MUC](https://xmpp.org/extensions/xep-0408.html) to allow all non-MIX client to participate in MIX channel discussions using MUC protocol.

# Tigase xmpp.cloud service with MIX support

Our [`xmpp.cloud`](https://xmpp.cloud) installation offers MIX today! It supports MIX-CORE, MIX-PAM (with MAM), MIX-ADMIN, MIX-MUC, MUX-MISC (message retraction)  

For now, neither MIX-PRESENCE (we only inform about channel participants without explicit publication their presence) nor MIX-ANON (there is only support for 'private messages') are available.

![open channel]({{ site.url }}{{ site.baseurl }}/assets/posts/xml.png)

# How to use it

First of all - you need an XMPP client that supports MIX, for now this is limited to [BeagleIM](https://beagle.im/) for macOS and [SiskinIM](https://siskin.im/) for iOS. Creating and joining channel is not different to joining MUC room:

1. select `open channel`:

![open channel]({{ site.url }}{{ site.baseurl }}/assets/posts/mix-create-1.png)

2. fill out the form:

![channel join form]({{ site.url }}{{ site.baseurl }}/assets/posts/mix-create-2.png)

3. start chatting!

![chat]({{ site.url }}{{ site.baseurl }}/assets/posts/mix-create-3.png)

# Other benefits of xmpp.cloud

As mentioned at the beginning of this article, in addition to MIX, [`xmpp.cloud`](https://xmpp.cloud) offers a lot:
- never worry about server downtime - it's a clustered installation, which means that at every point in time there will always be at least one server to connect to
- host your own domain for **free** - it's enough to point your domain's DNS SRV records to `tigase.me` and add it in `xmpp.cloud` system (as described [in the documentation](https://docs.tigase.net/tigase-server/master-snapshot/Administration_Guide/html/#_hosting_via_tigase_me))
- better PUSH for your mobile devices - more granular configuration and encrypted notifications
- anti-SPAM mechanism to squash unwanted messages
- **free** audio/video server (STUN/TURN) for you calling needs
- perfect A security grade:
    ![xmpp.net security score]({{ site.url }}{{ site.baseurl }}/assets/posts/xmpp-net.png) 
- 100% XMPP compliance
    ![100% XMPP compliance]({{ site.url }}{{ site.baseurl }}/assets/posts/xmpp.cloud-compliance-result.png)
