---
title: Tigase services continue to improve working with Amazon Web Services!
tags:
  - server
  - AWS
  - pro-tip
date: "2018-06-12T22:40:32.169Z"
description: Tigase XMPP Server works great with AWS's ELB, or any other setup with load balancer and internal IPs.
categories:
  - blog
header:
  overlay_image: /assets/posts/tiger_cleanup_darker.png
---

Recently, we began utilizing our _DualIP_ variant of our Load Balancing solution (based on see-other-host XMPP semantics) on our servers hosted on AWS and have found out that our load balancing implementation behaves very well with Amazon's ELB environment. The two services complement each other well to provide an extremely reliable and high tolerance XMPP platform.

Amazon's EC2 instances, by default, have both internal and external addressing (IP addresses and relevant hostnames), but only the internal addressing is readily available. What's more - Tigase uses the machine's addressing for various purposes: internal packet routing, inter-cluster communication and Tigase's Load Balancer ([Tigase Load Balancing](http://docs.tigase.org/tigase-server/7.1.2/Administration_Guide/html_chunk/loadBalancing.html)).
In case of the latter - Tigase Load Balancer - this duality of addressing in AWS cloud would pose a problem because users would receive a redirection to an internal address to which they would not be able to connect.

Moreover a lot of organizations, to improve high availability of the service, tend to use Amazon's Elastic Load Balancer (ELB) but unfortunately it doesn't understand XMPP. Therefore it's load balancing, while spreading the connections between cluster nodes, would leave room to improvement. Here is where Tigase's own load balancing solution with DualIP variant of our Load Balancing comes in. When Tigase server clusters are activated they are able to interconnect to all other active Tigase clusters over internal interfaces, but they also retrieve details about external addressing for all nodes within the cluster. This allows Tigase to maintain efficient inter-cluster communication but also to pass the proper external IP to the client upon connection so traffic knows exactly where to go. Tigase can further optimize traffic between it's nodes, by selecting a cluster that is best optimized or appropriate for the traffic. For example, all resource connections of particular JID would be switched to a single server to reduce network load between the clusters. Now the load balancers are no longer exclusive. For the server administrator, they see a much more reliable and optimized traffic flow between multiple cluster nodes. The end result is a dynamic, elastic, and optimized real time communication hosting solution capable of many millions of users combined with unparalleled scalability.

The solution is not exclusive for Amazon's cloud and can be adjusted to
other providers as well.

These benefits are now deployed and available on Tigase hosted servers backed by Amazon Web Services. If you want to take advantage of these benefits for your own realtime communication solution, contact <office@tigase.net> to find out how.

![Tigase on AWS behind ELB]({{ site.url }}{{ site.baseurl }}/assets/posts/tigase_acs_aws.2_1.gif)
