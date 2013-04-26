---
layout: post
title: "Double Netbeans icon on Docky"
date: 2011-01-07 12:00
comments: true
categories: [netbeans, docky, ubuntu]
published: true
permalink: /post/2638218912
---
For the past couple of weeks i’ve been putting up with a bug where my Docky shows two instances of Netbeans.  The icon on the dock acts as a launcher which creates another Netbeans icon instead of using just the single icon and lighting up.  The problem is due to Netbeans being a Java app and is also down to the way Docky handles window matching.

I did a bit of googling and found the cure at [http://www.issathen.co.uk/?p=14](http://www.issathen.co.uk/?p=14).

To get Docky to work nicely with Netbeans 6.9.1, create a file, netbeans-6.9.1.desktop, and stick the following contents inside it:

``` bash
#!/usr/bin/env xdg-open

[Desktop Entry]
Encoding=UTF-8
Name=NetBeans IDE 6.9.1
Comment=The Smart Way to Code
Exec=/bin/sh "/home/foo/netbeans-6.9.1/bin/netbeans"
Icon=/home/foo/netbeans-6.9.1/nb/netbeans.png
Categories=Application;Development;Java;IDE
Version=1.0
Type=Application
Terminal=0
StartupWMClass=java-lang-Thread
```

Obviously adjust the paths so they match up with your Netbeans location.  Alternatively, copy the Netbeans icon from your Ubuntu Applications -> Programming menu bar to your desktop and edit it.

Either way, once you’ve got the file with the crucial **StartupWMClass=java-lang-Thread** appended to the bottom, drag it onto your Docky and it should work correctly!