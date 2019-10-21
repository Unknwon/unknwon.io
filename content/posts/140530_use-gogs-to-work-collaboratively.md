+++ 
draft = false
date = 2014-05-30T02:20:00-04:00
title = "Use Gogs to work collaboratively"
slug = "" 
tags = []
categories = ["Gogs"]
thumbnail = "<no value>"
description = ""
+++

This post is published corresponding to the [Gogs - Go Git Service](https://github.com/gogits/gogs) `v0.4.1` release.

In the very beginning of this post, I want to speak for the develop team say sorry to all Gogs users:

> No matter if you've ever noticed in Trello board, we're sorry to say that the organization feature didn't implement in this release. This is caused by limited of team members in this period, but we're sure about to provide this feature in 0.5.0.

For me, this is a terrible release, we've delayed or canceled some of features that we planned to implement. However, it does not this release make no sense. You'll found batch of bug fixes, improvements and features from [Change Log](https://github.com/gogits/gogs/releases/tag/v0.4.1). Meanwhile, the number of contributors has increased from 17 to 32.

## Upgrade to 0.4

- If you're using 0.3.*, you can upgrade Gogs smoothly to 0.4.1.
	- Download ZIP archive, extract and copy everything to old directory.
	- Fetch latest source code and rebuild, if you install from source.
- ~~If you're using early versions, please following [Release Announcement](http://gogs.io/docs/advanced/release_and_tips_blogs.md) for upgrading.~~

## Add collaborator

You're able to add another person as your project collaborator, and the person will have all permissions except **delete** and **transfer** repository.

### Add other people

Visit `/:username/:reponame/settings/collaboration` you'll see the collaborator setting panel:

![](/img/140530/Snip20140528_1.png)

You can any registered user:

![](/img/140530/Snip20140528_2.png)

### Be a collaborator

You can see the list of repository you're collaborator in Dashboard:

![](/img/140530/Snip20140528_3.png)

## Webhook

Webhook is one of the spotlights in this release, website has complete [documentation](http://gogs.io/docs/features/webhook). Currently, it only supports **PUSH** event and **JSON** format POST, but we've prepared for more events and format underlying.

## Better issue tracker

Another big update is the huge improvement of issue tracker, including labels, milestones, assignees and dashboard statistic. Since now, you should get almost same functional experience of issue tracker like GitHub. What's more, we'll integrate some open API services like Trello to issue tracker for even better issue management.

## Summary

Like I said before, I'm personally not satisfied of how much time I spent in this release. But no worries, we are keeping growing with more contributors and feedbacks!

Some features are implemented but may has some changes later, so we will public them with more tip blogs. If you somehow find out some of them, try and give us feedback, this is extremely important to us!

In the end, I'd like to give special thanks to [@fanningert](https://github.com/fanningert) for giving tons of feedbacks in this release!

Thank you for supporting Gogs and taking time to read this post, if you have any advice or feedback, please contact us on [GitHub](https://github.com/gogits/gogs/issues?state=open).