+++ 
draft = false
date = 2014-09-13T23:39:00-04:00
title = "Organize your teams with Gogs organization"
slug = "" 
tags = ["Gogs"]
categories = ["Gogs"]
thumbnail = "<no value>"
description = ""
+++

This post is published corresponding to the [Gogs - Go Git Service](https://github.com/gogits/gogs) `v0.5.0` release.

After a whole summer, we finally go from Alpha to Beta with a new release. In this release, we introduced some major features like Organization, Multiple Languages and new UI, and we're receving contributions from more developers which make contributors of Gogs to be 55.

Please [Change Log](https://github.com/gogits/gogs/releases/tag/v0.5.0) for full list of changes in this release.

Strictly speaking, this is a transition release from `v0.4.2` to `v0.6.0`, and here are the reasons:

- New UI doesn't apply to all pages, few pages still using old UI.
- Core functionality of repository Fork and Pull Request cannot be released with this version because of UI still in progress.
- Some features are not implemented detailedly, need to be improved in next release.

So, why don't we get started to see what's new in this release?

## Upgrade to 0.5

- If you're using `0.3.* - 0.4.*`, you can upgrade Gogs smoothly to `0.5`.
	- Download ZIP archive, extract and copy everything to old directory.
	- Fetch latest source code and rebuild, if you install from source.
- ~~If you're using early versions, please follow [Release Announcement](http://gogs.io/docs/advanced/release_and_tips_blogs.md) for upgrading.~~

## Organization

### Create new organization

In the page `/org/create`, you can type a organization name and e-mail address to create one:

![](/img/140913/Snip20140913_13.png)

Then, you can get into organization home page through your dashboard:

![](/img/140913/Snip20140913_14.png)

As the creator, you are also the owner of the organization.

### Create new repository

If you're the owner of organization, you can choose the owner of reposiory to be an organization:

![](/img/140913/Snip20140913_15.png)

Now, you can find it in the organization home page(`/org/:orgname`):

![](/img/140913/Snip20140913_16.png)

### Create new team

The best way to distinguish members' responsibilities is putting them into different teams with different permissions:

![](/img/140913/Snip20140913_17.png)

Now, you can add members to the team, note that before a memeber join any team, he/she does not have any level of permissions of all organization's repositories:

![](/img/140913/Snip20140913_18.png)

or add repositories to a team:

![](/img/140913/Snip20140913_19.png)

### Manage organization

Of course, you can manage all members in organization at once:

![](/img/140913/Snip20140913_20.png)

or all teams:

![](/img/140913/Snip20140913_21.png)

## Other major changes

- Localize Gogs: Gogs now has English, Simplified Chinese and Germany as built-in support of localization, please see [documentation](http://gogs.io/docs/features/i18n.html) for details.
- Slack Webhook: setting it at webhook page.
- Organization level webhook: setting it at organization settings page.
- Repository migration API: scriptable migration API, please see [documentation](https://github.com/gogits/go-gogs-client/wiki/Repositories#migrate) for details.
- Web framework has been changed from Martini to [Macaron](https://github.com/go-macaron/macaron) in order to reuse modules that are developed by Gogs project.

## Summary

I have several things to say before finishing this post:

- We accepted many remarkable Pull Requests during this release, we're grateful for all support from the community.
- Roadmap will be released at early development cycle of next release.
- If you're good at languages other than English, Simplified Chinese and Germany, we need your help to make more translations, please see [documentation](http://gogs.io/docs/features/i18n.html#contribute-translation) for details.
- ~~Now [Gopm](https://github.com/gpmgo/gopm) tool is officially recommended to build Gogs project in deployment environment.~~

Thank you for supporting Gogs and taking time to read this post, if you have any advice or feedback, please contact us on [GitHub](https://github.com/gogits/gogs/issues?state=open).