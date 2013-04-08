---
layout: post
title: "How to find out your Ubuntu version"
date: 2011-12-05 12:00
comments: true
categories: [ubuntu]
published: false
---
To **find out your Ubuntu version** (or codename) use the `lsb_release` command which will “print distribution-specific information”:

    $ lsb_release -a
    No LSB modules are available.
    Distributor ID: Ubuntu
    Description:    Ubuntu 11.04
    Release:        11.04
    Codename:       natty

Or if you just want the string of the codename:

    $ lsb_release -s -c
    natty