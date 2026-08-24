---
permalink: /about/
title: "About"
excerpt_separator: "<!--more-->"
header:
  overlay_image: /assets/images/Extra_3.png
---

Started in 2004 as a hobby project to create a complete XMPP/Jabber server which is super efficient on resources.

<!--more-->

The goal was to have a fully functional and complete server running with just 10MB of RAM and binaries taking less 
than 1MB of disk space for a minimal configuration. On the other hand, the same software had to be able to handle 
millions of online users and process millions of messages per second for large deployments.

The goal was reached. For a small number of users, the Tigase XMPP Server could be run on 10MB RAM and binaries 
took no more than 1MB of disk space. 

However, the minimal configuration with support for bare XMPP specification is no longer considered as a functional 
XMPP server. Nowadays, an XMPP server is supposed to support several extensions typically used by user apps. But 
still the main Tigase XMPP Server binary file size is less than 3MB and fully functional system can be run with just 
50MB of RAM.

Tigase XMPP Server also supports symmetric clustering to connect millions of users at the same time and can handle 
traffic of millions of messages per second. In fact there are production systems maintaining 40mln concurrent user 
connections and production systems in gaming sector handling over 3mln messages per second.

While the Tigase XMPP Server is a great piece of software and works very well, we did not stop there. We developed 
XMPP libraries to allow creating great end-user apps by anybody. And then, we used the libraries ourselves and 
created the best native XMPP/Jabber apps for iOS, MacOS, Android and we are still working to bring apps for all 
popular platforms.

And all our software is open source and available for free under AGPLv3 license.

Enjoy!
