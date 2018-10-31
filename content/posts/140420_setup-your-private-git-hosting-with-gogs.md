+++ 
draft = false
date = 2014-04-20T15:53:00-04:00
title = "Setup your private Git hosting with Gogs"
slug = "" 
tags = ["Gogs"]
categories = ["Gogs"]
thumbnail = "<no value>"
description = ""
+++

This post is published corresponding to the [Gogs - Go Git Service](https://github.com/gogs/gogs) `v0.3.0` release.

After hard working of more than half of month, Gogs finally gets into a very important release: v0.3.0. There are couple of reasons that why I say this release is important:

- We start receiving advice and feedback from users after first public release, which makes Gogs make more improvements and less bugs.
- This release adds a lot of new features, including private repository, migrate or mirror repository, ship with Docker, and social account log in.
- The [contributors of Gogs](https://github.com/gogs/gogs/graphs/contributors) during this release has increased to 17 people, which means Gogs is getting more eyes from other developers.

If you're interested in how many changes we've made compare to last release, please see the [Change Log](https://github.com/gogs/gogs/releases/tag/v0.3.0).

Unlike last release post with only overview, this post will teach you how to use some new features to help you get started.

## Upgrade from v0.2.0 to v0.3.0

~~If you're using release v0.2.0, the upgrade should be smooth. The only thing you need to do is setting up the mailer, and reset users' password by sending reset password e-mails. This is because we changed the way to encrypt users' password and became more secure ([note](https://github.com/gogs/gogs/wiki/Troubleshooting#upgrade-from-v020)).~~

## Setup private Git hosting

This release of Gogs supports two types of private hosting: private repository and whole site private.

### Private repository

It's just a matter of check box in create repository page:

![](/img/140420/QQ20140418-1.png)

Gogs hasn't supported collaborator yet, so only the creator of repository has rights to view and access the data.

### Whole site private

This feature was added in v0.2.0, but due to incomplete functionality of HTTP(S) PUSH/PULL, we decide to announce in this release.

To enable this feature, just modify following configuration options:

```ini
[service]
; Does not allow register and admin create account only
DISABLE_REGISTRATION = true
; User must sign in to view anything.
REQUIRE_SIGNIN_VIEW = true
```

What they're doing are: disallow registration(admin create only) and force log in to view anything.

Once you enable sign in view option, the public repository will require user authorization for HTTP(S) access.

## More types of repository

Responding to feature requests from some users, we add more types of repository in this release, such as migrate repository from other Git hosting sites, mirror repository, and some other support.

### Migrate repository

In the migrate repository page, type your HTTP(S) address and you're all set. Be aware that this address can from any Git hosting site, not only GitHub.

![](/img/140420/QQ20140418-2.png)

Type user name and password if it needs authorization.

### Mirror repository

In the same page of migrate repository, check box of "This repository is a mirror", it leads to automatically check updates from source(24 hours interval as default). Notice that the only operation you can do to mirror repository is `git clone`.

### Other support

~~In repository setting page, we added default branch and `go get` enable option:~~

~~Nothing need to say about default branch. Once you enable the `go get` option, you can use `go get` to pull your repository that are hosted by Gogs.~~

Now supported by default.

## Summary

Private support is a spotlight of v0.3.0, but not the only spotlight. We also use Smart HTTP(support since Git 1.6.6) to replace Webdav to make huge improvements for stability and availability, add [Docker deploy](https://github.com/gogs/gogs/tree/master/docker) support. Furthermore, we successfully reduced memory and CPU usage when access the Git repository.

Thank you for supporting Gogs and taking time to read this post, if you have any advice or feedback, please contact us on [GitHub](https://github.com/gogs/gogs/issues?state=open).