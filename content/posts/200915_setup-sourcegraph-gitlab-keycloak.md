+++ 
draft = false
date = 2020-09-15T15:59:00+08:00
title = "Set up Sourcegraph with two GitLab and Keycloak using SAML 2.0 OmniAuth Provider"
slug = "" 
tags = ["Sourcegraph", "GitLab", "Keycloak", "SAML"]
categories = ["DevOps"]
thumbnail = "<no value>"
description = ""
+++

## Background

I need to set up two GitLab that uses same SSO (Keycloak) and wire up with Sourcegraph to test repository permissions. This is my ops log for doing it.

Because both [GitLab SAML OmniAuth Provider](https://docs.gitlab.com/ee/integration/saml.html) and [Keycloak](https://twitter.com/jc_unknwon/status/1305417765667315712?s=20) require HTTPS and I'm so used to just use [Caddy](https://caddyserver.com/) to set up such thing with [Let's Encrypt](https://letsencrypt.org/), so I create one GCP VM for each GitLab and Keycloak instances and assign them with some subdomains.

For [Sourcegraph](https://sourcegraph.com/), I'll use my local instance.

## Set up Keycloak

### Preparation

1. Create a VM using **Ubuntu 20.04**, machine type **e2-medium (2 vCPUs, 4 GB memory)**, and **10 GB SSD**.
2. Follow the offical guide of [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/).
3. Add a DNS A record to resolve a subdomain to this GCP VM, here I use `keycloak.example.com`.

### Install Keycloak

Run the following command to start a Keycloak Docker container:

```sh
$ sudo docker run -d \
    -e PROXY_ADDRESS_FORWARDING=true \
    -e KEYCLOAK_USER=admin \
    -e KEYCLOAK_PASSWORD=<REDACTED> \
    -p 8080:8080 \
    --name keycloak \
    jboss/keycloak
```

This tells the Keycloak that it is running behind a reverse proxy (Caddy) and set up the initial admin account with given credentials.

### Install and configure Caddy

_As of writing, 2.2.1 is the lastest version of Caddy._

```sh
$ wget https://github.com/caddyserver/caddy/releases/download/v2.1.1/caddy_2.1.1_linux_amd64.tar.gz
$ tar zxf caddy_2.1.1_linux_amd64.tar.gz
$ sudo mv caddy /usr/bin/caddy
$ sudo mkdir -p /etc/caddy
$ sudo vi /etc/caddy/Caddyfile
{
    http_port 80
    https_port 443
}

keycloak.example.com {
    reverse_proxy * localhost:8080
}

$ screen -S caddy
$ sudo caddy run --environ --config /etc/caddy/Caddyfile
```

### Configure Keycloak

Visit https://keycloak.example.com and login with the admin account, then create a test user "alice":

![](/img/200915/keycloak_alice.png)

TBD
