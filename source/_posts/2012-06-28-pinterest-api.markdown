---
layout: post
title: "Pinterest API"
date: 2012-06-28 12:00
comments: true
categories: [pinterest api]
published: true
---
I've started working on my homepage [jmoz.co.uk](http://jmoz.co.uk) which I'm building in Python/Django, mainly for a learning exercise.  One of the things I wanted was to get some of my content from Pinterest, unfortunately their API isn't working or hasn't been built (as of June 2012).

I built a small scraper which pulls me my pins and spits it out in json.  I built it using [Silex](http://silex.sensiolabs.org/) which is a small lightweight framework built off of the Symfony2 components.

I've deployed the scraper as [Pinterest API](http://pinterestapi.co.uk), give it a go, see if it can provide you some of your Pinterest content to use on your app, make sure you cache it!