+++ 
draft = false
date = 2015-12-12T22:22:00-04:00
title = "Gogs: Pull Request, Wiki and New Design"
slug = "" 
tags = []
categories = ["Gogs"]
thumbnail = "<no value>"
description = ""
+++

> This post is published corresponding to the [Gogs - Go Git Service](https://gogs.io) `v0.8.0` release.

It has been over a year since last release post, mainly because there are too much work undergoing and plans are completely outdated. Fortunately, most of things finally get done in this big release. 

From last post of release (`v0.5.0`), almost 1.8k commits with tons of improvements, bug fixes, new features and others are added to Gogs. Especially top-wanted features like **pull request**, **repository wiki** and **builtin SSH server**. Besides, all pages are now under new Semantic UI theme with **completely redesigned issue tracker**, and many people have given us positive feedback. The last thing I never forget to mention is Gogs now has 182 contributors from the community.

### Upgrade to 0.8

- Gogs has auto-migration since `0.5.x`, so all upgrades should happen automatically and you do not need to take care of it.
- ... However, starting from `0.8` release, Gogs no longer supports auto-migration for installations that are lower than `0.6.0`. Therefore, if you're using version before `0.6.0`, you have to run any of earlier version before `0.8.0` once, then upgrade to `0.8.0`.
- Upgrade steps are available in:
 - [Upgrade from binary](https://gogs.io/docs/upgrade/upgrade_from_binary)
 - [Upgrade from source](https://gogs.io/docs/upgrade/upgrade_from_source)

### Pull Request

The No.1 wanted feature has finally arrived in Gogs, and it has been tested with several releases since `0.6.9`. It is good to use now, but not perfect yet, such as you can't make a pull request inside same repository, and no review comments support.

Many underlying improvements can be applied as well, dirty works needed to be cleared out.

![](/img/151212/Snip20151211_7.png)

### Wiki

You can now edit your wiki pages via online Markdown editor or edit locally and push to Gogs.

![](/img/151212/Snip20151211_8.png)

### Builtin SSH Server

Few people ask why Gogs even need this. Let me answer it once forever:

Even though Gogs has gained many advantages by developing based on Go, such as low resource usage and fast response, but primary goal of Gogs is still very simple, which provide painless Git hosting solution. Embed a SSH server means an external SSH server is no longer a hard requirement and Gogs will not mess up with `authorized_keys` file any more. Last but not the least, it works on Windows!

### Issue Tracker

Dozens of bug fixes by redesigned issue tracker, you are also able to use emoji and edit comments now.

![](/img/151212/Snip20151211_9.png)

![](/img/151212/Snip20151211_10.png)

### Others

- You can push [linux](https://github.com/torvalds/linux) to Gogs now.
- Limited APIs are available at [wiki](https://github.com/gogits/go-gogs-client/wiki). 
- HTTPS secured [downloads](https://dl.gogs.io).
- Discussion [forum](http://forum.gogs.io/).
- Official support for [Docker](https://github.com/gogits/gogs/tree/master/docker) with only 40MB image size.
- More and more [cloud platforms](https://github.com/gogits/gogs/tree/master#deploy-to-cloud), [products](https://github.com/gogits/gogs/tree/master#product-support), [services and software](https://github.com/gogits/gogs/tree/master#software-and-service-support) support.

### Final Bits

Can't thank enough to the all the people who follow a long way to this point. You are awesome!

Thank you for supporting Gogs and taking time to read this post, if you have any advice or feedback, please contact us on [GitHub](https://github.com/gogits/gogs/issues?q=is%3Aopen).