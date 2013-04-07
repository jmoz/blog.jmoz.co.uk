---
layout: post
title: "Python: Convert datetime to timestamp and back again"
date: 2012-08-29 12:00
comments: true
categories: [python, datetime, timestamp]
published: false
---
It's pretty easy to get a **datetime** object from a **timestamp** in Python.  However the same can not be said the other way around.  Just when I start to think "oh Python's pretty cool with a nice API" the further you look into the language and library you start to notice some cracks.

## Convert a timestamp to a datetime object

``` python
from datetime import datetime

print datetime.fromtimestamp(1346236702)

#2012-08-29 11:38:22
```

## Convert a datetime object to a timestamp

Why oh why did they not just implement a `datetime.totimestamp()` method on datetime?

``` python
from datetime import datetime
import time

dt = datetime.fromtimestamp(1346236702)

print time.mktime(dt.timetuple())

#1346236702.0
```