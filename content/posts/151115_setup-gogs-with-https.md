+++ 
draft = false
date = 2015-11-15T20:15:00-04:00
title = "Setup Gogs with HTTPS"
slug = "" 
tags = ["Gogs"]
categories = ["Gogs"]
thumbnail = "<no value>"
description = ""
+++

There are two ways to use HTTPS for Gogs based on the way you choose to deploy:

1. Reverse proxy, e.g. NGINX, Caddy
2. Expose Gogs on the internet directly

And there are two types of HTTPS certificates: real and pretend to be real.

So, let's talk about how to get your HTTPS certificates.

### Obtain HTTPS Certificates

1. Buy business version (OH PLEASE!!! Donate a million or two to Gogs project 😊)
2. Apply free version from [Let's Encrypt](https://letsencrypt.org/) or [StartSSL](https://www.startssl.com/) 
3. Self-signed


You can Google (I just happened to be lazy) about how to get the first two versions of certificates. There are also some tools can generate self-signed certificates, but in this article, I'm going to talk about how to use Gogs to generate them. 

First of all, you have to build Gogs with build tags `cert` (official binaries are included this tag by default).

After that, execute the following command is all you need:

```
$ ./gogs cert -ca=true -duration=8760h0m0s -host=myhost.example.com
```

Don't forget to replace with your own domain (Wait, did you just say that you DO NOT OWN ONE? [Go buy it](https://www.godaddy.com/)!)

OK, now you should have two new files in the current directory: `cert.pem` and `key.pem`.

### Config HTTPS Certificates

I've mentioned that there are two ways to config HTTPS for Gogs.

If you're using reverse proxy, all you need to do on Gogs's configuration is making sure your `ROOT_URL` is using a HTTPS URL, such as `https://try.gogs.io`, and 

NOTHING ELSE!
NOTHING ELSE!
NOTHING ELSE!

For NGINX, see example below:

```
server {
    listen 443 ssl;
    server_name try.gogs.io;
    ssl_certificate path/to/cert.pem;
    ssl_certificate_key path/to/key.pem;

    location / {
        proxy_set_header  X-Real-IP  $remote_addr;
        proxy_pass http://localhost:3000$request_uri;
    }
}

# Redirect HTTP requests to HTTPS
server {
    listen 80;
    server_name try.gogs.io;
    return 301 https://$host$request_uri;
}
```

Look at it! Gogs still starts as normal HTTP! Let NGINX do all the dirty works!!!

Maybe I'm not so clear, but I believe you get the point...

Great, see how to do the same thing with Caddy:

```
https://try.gogs.io {
    proxy / localhost:3001
    tls path/to/cert.pem path/to/key.pem
}

# Redirect HTTP requests to HTTPS
http://try.gogs.io {
    redir https://try.gogs.io{uri} 301
}
```

It's so damn obvious in Caddy!

Of course, I'm not saying expose Gogs directly on the internet is bad, you have your ways of doing things. Then, change the following configuration in your custom `app.ini` file:

```ini
[server]
PROTOCOL = https
ROOT_URL = https://try.gogs.io/
CERT_FILE = path/to/cert.pem
KEY_FILE = path/to/key.pem
```

(I have to say Gogs itself has the least lines of configuration 😂)

Well done!

If you're using real certificates, a green nice locker will show near the URL bar in Chrome. Otherwise, an ugly red sad face is taking the place.