---
layout: post
title: "A django-nonrel testapp blog with tagging"
date: 2012-11-20 12:00
comments: true
categories: [appengine, django, django-nonrel, django-testapp, python]
published: true
---
If you're working with **[django-nonrel](https://github.com/django-nonrel/django-nonrel)** and **appengine**, there's a **[django-testapp](https://github.com/django-nonrel/django-testapp)** barebones application you can work off as a starting point (there's no application code, just setup code and files for appengine).

I decided to have a play with it and come up with a basic blogging application.  I scoured the web for example simple django-nonrel apps but couldn't find too many; because of this I've decided to release this small example so as to point people in the right direction when starting out with appengine and nonrel.

I had a few troubles trying to figure out the best way to implement simple tagging for a blog post.  The solution I ended up with used a ListField from [django-toolbox](https://github.com/django-nonrel/djangotoolbox) (a django ManyToMany field is not supported by django-nonrel).

I also integrated [django-bootstrap-toolkit](https://github.com/dyve/django-bootstrap-toolkit) so it comes with a frontend out the box.

I forked the repo and uploaded my changes to [django-testapp](https://github.com/jmoz/django-testapp) so go there and clone the project to get a basic django-nonrel blogging application with tagging, and some bootstrap thrown in for free.