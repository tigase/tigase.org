---
title: XMPP - An Introduction
tags:
  - server
  - xmpp-intro
  - pro-tip
date: "2015-08-11T22:40:32.169Z"
description: Understanding XMPP from beginner perspective - let's start.
categories:
  - blog
header:
  overlay_image: /assets/posts/xmpp-logo-460.png
---

My first experience with XMPP was back in the early 2000s when a friend of mine had sworn off all instant messaging clients of the time, most of them being unable to connect to one another, having proprietary software loaded with advertisements, and no real way to have a unified chat log between them. He pointed me to an up and coming communication standard then known as Jabber. What I didn’t know at the time was this small open source chat interface was the beginnings of an open platform right up there with the likes of HTTP, POP, IMAP, and FTP. The Jabber protocol was eventually renamed to XMPP, standing for eXtensible Message and Presence Protocol. Although the name changed happened in 2007, there are still many references to the same protocol using both names. Of the most notable changes, and arguably the most important, to the name is the inclusion and initial position of ‘extensible’. This means that it can do many things, that the platform is designed to break out of its chat message origins and be…well more. XMPP can be extended to do some amazing things; be a platform for emergency information delivery, a system to create cloud storage, a GPS tracking and reporting system, and home security for starters. Because XMPP functions mostly in the background it was hard to notice its presence, but it is most certainly there. Some of the largest tech companies use XMPP; Google, Verizon, HP, Cisco, and Facebook to name a few.

For such a protocol to have such wide use, and so many applications, it can be daunting to know where to start or how to implement XMPP. This is what the XMPP Standards foundation helps to solve. XMPP is an open standard, meaning that the core protocols and extensions have a tested, proven, and expected way of working. Although you can write code however you want, the standards provided in the XMPP Extension Protocols (XEP) make it easy to implement a particular feature. For example, say you want your XMPP server to determine where a user’s physical location is. The article XEP-0080: User Location describes from start to finish how to obtain, interpret, and use that data taking the guesswork out of implementing that feature. Adhering to these standards makes your code easy to understand with those familiar to XMPP, and if you decide to be open source, can make collaboration much simpler.

Perhaps the biggest shock to my expectations of XMPP is that it is a thriving community. Developers, coders, and overall enthusiasts frequent many forums, coding projects, and chat rooms. There have been occasions where I have run into walls setting up an XMPP server and have been lifted over a particular hurdle by a friendly helping hand. Beyond that, developers have offered fixes and improvements to others’ projects without want for credit or reward. This altruistic spirit that resonates in open source communities, XMPP included, is an unexpected turn from the expectation of somebody walking into the high tech world. There is a good chance that if you are using anything running XMPP, it has been touched by hundreds if not thousands of hands.
