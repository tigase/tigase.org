---
title: Let's talk
tags:
  - anonucement
date: "2017-03-13T22:40:32.169Z"
description: We are making it easier to get in touch, get support and track our projects.
categories:
  - blog
header:
  overlay_image: /assets/posts/tiger_cleanup_darker.png
---

Previously, our main project, tigase-server used a couple of maven modules (separate for the server sources, documentation and one responsible for building distribution packages). Unfortunately for equally as long, the file structure did not follow the standard maven structure. This has been changed today! On subsequent instances of `$git pull` you can expect changes in the folder structure and files location from:

```Tigase Server
|- Master
|- Documentation
\- Distribution
```

To:

```
Tigase Server Master
|- Tigase server
|- Documentation
\- Distribution
```
