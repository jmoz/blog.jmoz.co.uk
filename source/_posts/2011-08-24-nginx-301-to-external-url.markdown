---
layout: post
title: Nginx 301 to external url
date: 2011-08-24 13:00
comments: true
categories: [nginx]
published: true
---
I'm currently migrating my posterous blog domain name
[vietnameseinshoreditch.co.uk](vietnameseinshoreditch.co.uk) to something which I think is a little
better and more SEO friendly, **[shoreditchvietnamese.co.uk](shoreditchvietnamese.co.uk)**.  To
redirect the existing urls on the old domain to the new domain I need to set
up an **Nginx 301 redirect**.

Currently my DNS A records for vietnameseinshoreditch.co.uk point to
posterous' IP.  I want to point them to my own server which is running
**Nginx** and get Nginx to **301** any requests to my new domain (which will
point to posterous).  This way any previous links lying around will still work
and I'll maintain all of Google's link juice and eventually
shoreditchvietnamese.co.uk will be seen as the **canonical url**.

Here's the Nginx config:

{% gist https://gist.github.com/1166673 %}

Simple.  This 301s all requests to the new domain name and maintains the full
request uri.

Give it a go, the link below should get 301'd from
vietnameseinshoreditch.co.uk to shoreditchvietnamese.co.uk.

[http://vietnameseinshoreditch.co.uk/welcome-to-vietnamese-in-shoreditch](http://vietnameseinshoreditch.co.uk/welcome-to-vietnamese-in-shoreditch)
