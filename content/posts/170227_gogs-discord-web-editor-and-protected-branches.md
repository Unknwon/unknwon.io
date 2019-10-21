+++ 
draft = false
date = 2017-02-27T16:41:00-04:00
title = "Gogs: Discord, Web Editor and Protected Branches"
slug = "" 
tags = []
categories = ["Gogs"]
thumbnail = "<no value>"
description = ""
+++

> This post is published corresponding to the [Gogs](https://gogs.io) `0.10` release.

First things first, you may have noticed there are some notable changes to the branding of Gogs project:

1. New logo: great thanks to its designer [Egon Elbre](https://twitter.com/egonelbre). Please also take a look at [his brilliant art-work of Gophers](https://github.com/egonelbre/gophers), you will love them.
2. Simplified tagline: it is just **Gogs**, no _Go Git Service_ anymore (you can still use it if that makes sense to you), and **NOT** `GoGS` (this is 100% wrong!).
3. Twitter handle: it is now [@GogsHQ](https://twitter.com/gogshq), not _@gogitservice_.
4. Official pronunciation: `/gɔgs/`, [Google Translate](translate.google.com) can pronounce it very similarly (type `Gogs`).

Great! It has been another year since last release post, around 1.2k commits are added from 140 more contributors, so we now have 322 contributors in total. High-rank features like **web editor**, **protected branches**, **IPython Notebook rendering**, **lable templates** are also shipping within this release.

### Upgrade to 0.10

- Starting from `0.10 RC` release, Gogs no longer supports auto-migration for installations that are lower than `0.7.0`. Therefore, if you're using version before `0.7.0`, you have to run any of earlier version before `0.10 RC` (e.g. `0.9.141`) once, then upgrade to `0.10`.
- Upgrade steps are available in:
 - [Upgrade from binary](https://gogs.io/docs/upgrade/upgrade_from_binary)
 - [Upgrade from source](https://gogs.io/docs/upgrade/upgrade_from_source)

### Discord Webhooks

Native webhook support for Discord is added under request from [@Meowbeast](https://github.com/Meowbeast). Big thanks to him for active responses to all bug reports and feedback.

![](/img/170227/Snip20170226_34.png)

For now, you're able to customize its **name**, **icon** and **color**, same as the Slack webhook settings.

### Web Editor

The original feature was implemented by [@richmahn](https://github.com/richmahn) and you're now able to **create**, **edit**, **delete** or even **upload** files to repository in your browser!

![](/img/170227/Snip20170226_36.png)

### Protected Branches

It is now possible to protect branches from [force pushes](http://stackoverflow.com/questions/10510462/force-git-push-to-overwrite-remote-files), deletion and require pull request to merge changes. For organizational repositories, you can also whitelist users and teams for pushing to a specific branch.

![](/img/170227/Snip20170226_37.png)

### Others

- Gogs has redesigned Git hook mechanism which is the basis of implementing protected branches and completely solved the problem [Large consumption of RAM while pushing through HTTP](https://github.com/gogits/gogs/issues/636).

### Final Bits

Thank you for supporting Gogs and taking time to read this post, if you have any advice or feedback, please contact us on [forum](https://discuss.gogs.io/).

Enjoy!