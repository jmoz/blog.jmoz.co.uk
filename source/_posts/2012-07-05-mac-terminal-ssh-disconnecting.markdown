---
layout: post
title: "Mac terminal ssh disconnecting"
date: 2012-07-05 12:00
comments: true
categories: [mac, terminal, ssh]
published: false
---
I switched to a mac at the start of the year; prior to that I always used Linux and regularly ssh'd into remote boxes with hardly any problems and rarely any disconnects.

Since I've started using the mac for development and ssh'ing into remote boxes using *iTerm*, *Terminal* or *TotalTerminal* etc, I've noticed I keep having problems with **ssh disconnecting**, sometimes with the error message **Write failed: Broken pipe**.

To fix it just edit your `~/.ssh/config` file and add the following options which will affect all hosts:

	Host *
	ServerAliveInterval 240
	ServerAliveCountMax 3

The `ServerAliveInterval` tells ssh to send a keep alive packet every 240 seconds or 4 minutes.  The `ServerAliveCountMax` tells ssh to do this a maximum of 3 times without getting a response, then to close the connection from the client side.  This will prevent the annoying lockup where the terminal freezes for a minute or two then disconnects.  Since I've made these changes my terminal has started to behave more like my good old Linux laptop.