---
layout: post
title: "Nginx connect() failed while connecting to upstream"
date: 2012-07-11 11:00
comments: true
categories: [nginx]
published: false
---
I've just spent a good 30 minutes debugging my **PHP** code and **Nginx** config trying to figure out why I was getting a **HTTP/1.1 502 Bad Gateway error** in my browser and the following in my logs:

	2012/07/11 22:52:18 [error] 23021#0: *29 kevent() reported that connect() failed (61: Connection refused) while connecting to upstream, client: 127.0.0.1, server: dev.foo.co.uk, request: "GET /foo HTTP/1.1", upstream: "fastcgi://127.0.0.1:9000", host: "dev.foo.co.uk"
	
Turns out it's pretty simple and god damn OBVIOUS (after you've found out of course).

**php-fpm isn't running**.

This came up because I restarted my mac the other day and so to resume development I needed a quick `sudo nginx`.  I forgot about the `sudo php-fpm`.

My excuse is that my apps index page goes to a static html file and the rest to PHP so I was confused why only parts of it were working!