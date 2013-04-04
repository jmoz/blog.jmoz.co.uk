---
layout: post
title: "Infinite scroll featuring JQuery, Flask, Twitter Bootstrap and a cameo from Redis"
date: 2013-01-18 12:00
comments: true
categories: [python, jquery, infinite scroll, flask, twitter, bootstrap, redis]
published: false
---
The idea behind [jQuery infinite scroll](https://github.com/paulirish/infinite-scroll) is actually quite a clever one - it employs the principal of 'graceful degradation' - that is to add JavaScript and jQuery functionality to already working static HTML, so that if a user doesn't have JS enabled (which is rare nowadays), the JS enhanced functionality can still be achieved but in a less impressive, less dynamic, old school way.

Think about **pagination** for a moment.  Pagination ultimately exists to allow you to iterate over, or view, an entire recordset.  Infinite scroll achieves the end result, but in a different manner.

jQuery infinite scroll is built on top of regular, static HTML pagination.  It looks at the pagination links on a page and dynamically loads a page in the background, then pulls out specified items in the HTML and appends them onto the current page.  So if you have pagination on your page already, or easily provided by your framework, you can get up and running with infinite scroll in about 10 minutes.

## A Python pagination class

I'm using a Python micro framework, **Flask**, which doesn't come with built in pagination.  There's probably some library available on pip that can provide it but I wanted to write it myself.  I like Test Driven Development, in fact, *I love TDD*.  This, coupled with the fact that I'm learning Python atm made me decide to build the class from scratch for fun.  I wrote my tests along with it so I'm certain it works correctly for my case.

Here's the Pagination class I came up with.

{% gist 4552212 %}

You can see the usage in the doc.  The unit test I used is below.

{% gist 4565776 %}

## Putting it together with Flask

The only thing you need to change in Flask is to set up a route that takes a current page parameter and to pass the Pagination object to the view so it can display a list of links.  

I use Redis in my app so I've added some client code with a bonus bit of Redis' `ZREVRANGE`; you can see how I've used the Pagination object and offsets to get my result set back.

{% gist 4566295 %}

## Twitter bootstrap pagination

Here's the HTML to display a nice bootstrap pagination widget.

{% gist 4566424 %}

You can see what the end result looks like on the [bootstrap pagination page](http://twitter.github.com/bootstrap/components.html#pagination).

This all works great so it's time for the jQuery.

## jQuery infinite scroll

So after all that, the easiest part is to include the infinite scroll JavaScript file, then a small bit of JS sets it all up.

{% gist 4566491 %}

`.container` is my main div with my `div.item`s inside which hold each piece of amazing content from Redis.  The only thing I've tweaked is the `bufferPx` value as I found whilst scrolling on my MacBook Air, I'd hit the bottom of the page quite easily and needed a larger pixel amount to buffer at.

That's it.  The most complicated part of this was writing the Pagination class and unit tests - but that's my favourite part (I'm a backend dev).  Hopefully your framework will have pagination built in, and if so, implementing infinite scroll should be a breeze.