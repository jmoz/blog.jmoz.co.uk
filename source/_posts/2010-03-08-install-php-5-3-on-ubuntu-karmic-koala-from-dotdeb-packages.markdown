---
layout: post
title: "Install PHP 5.3 on Ubuntu Karmic Koala from dotdeb packages"
date: 2010-03-08 12:00
comments: true
categories: [ubuntu, php, dotdeb]
permalink: /post/435401471
published: true
---
## First up, add the dotdeb repository

Fire up vim and add the following 2 lines to /etc/apt/sources.list:


    deb http://php53.dotdeb.org stable all
    deb-src http://php53.dotdeb.org stable all

Now do a sudo apt-get update to update your packages.  The next step may fail
but give it a go anyway.

## Use Aptitude to install the PHP 5.3 packages.


    sudo aptitude install php5-cli php5-common php5-cgi php5-mysql php5-curl libapache2-mod-php5 php5-memcache php5-xdebug

If this goes through ok, well done!  In my case I had dependency issues that
could not be satisfied.  The package **libicu38** was needed but could not be
found in the repos in my sources.  I did a quick google and could see that
**libicu38 **had been taken out of Karmic but was available in Jaunty
security, so add that repo to the sources file:


    deb http://security.ubuntu.com/ubuntu jaunty-security main

Now do a apt-get update and then:


    sudo aptitude install libicu38

Now go back to the previous step and try to install the **PHP 5.3** packages
again, hopefully they should work now.

You can now try and see if php 5.3 is working, issue php -v at the command
line and see if it works correctly, once working you'll get:


    PHP 5.3.1-0.dotdeb.1 with Suhosin-Patch (cli) (built: Dec  5 2009
	20:08:29) Copyright (c) 1997-2009 The PHP GroupZend Engine v2.3.0, Copyright
	(c) 1998-2009 Zend Technologies with Xdebug v2.0.5, Copyright (c) 2002-2008,
	by Derick Rethans with Suhosin v0.9.29, Copyright (c) 2007, by SektionEins
	GmbH

In my case I had a couple of errors that needed fixing - PHP complained about
a date setting and xdebug also shouted a bit.  They were easily fixed.

## Edit xdebug.ini.

Fire up vim and edit /etc/php5/conf.d/xdebug.ini, you need to change the
extension line so it's a zend_extension and maybe also set the full path to
the xdebug.so file (depending on if you set your include path in php.ini):


    zend_extension=/usr/lib/php5/20090626+lfs/xdebug.so

## Edit php.ini.

vim /etc/php5/apache2/php.ini and find the line that starts with
;date.timezone and change it to:


    date.timezone = Europe/London

To get xdebug to work correctly you need to enable html errors:


    html_errors = On

Finally do a sudo apache2ctl restart and everything should be working!