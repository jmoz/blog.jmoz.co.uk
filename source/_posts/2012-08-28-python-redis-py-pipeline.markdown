---
layout: post
title: "Improve Redis performance with pipelining"
date: 2012-08-28 12:00
comments: true
categories: [python, redis]
published: true
---
I have been playing with **Python** and **Redis** recently, specifically [redis-py](https://github.com/andymccurdy/redis-py) which is available on github and can be easily installed with `pip install redis`.

With **NoSQL** (Redis) a common usage pattern is to save items, such as users or tweet type things as hashes, indexed in a set or list, then retrieve them by key individually but in a large looped batch.  In SQL this is analogous to querying a table by id in a loop rather than one select statement with a `WHERE` clause.  If you did this in SQL other developers would probably talk about how much of a noob you were over Skype; in Redis it's ok because Redis is [web-scale](http://www.mongodb-is-web-scale.com/) and optimised for this kinda stuff.

I've been wondering about the performance of Redis and **redis-py**'s [pipelining feature](https://github.com/andymccurdy/redis-py#pipelines) which "can be used to dramatically increase the performance of groups of commands by reducing the number of back-and-forth TCP packets between the client and server".  

I created a script that simulates some typical Redis processing with and without pipelining and timed it using Python's [timeit](http://docs.python.org/library/timeit.html).

## Method

To test the performance of the **Redis pipeline** feature the following actions are executed non-pipelined and then pipelined.

- Create 10000 user keys in a list.
- Update 10000 user hashes with id, name and email fields.
- Retrieve 10000 user hashes into a Python list.
- Delete the user list and 10000 user hashes.

Each test is timed and executed one time but [repeated 7 times](http://stackoverflow.com/a/8220943/68534) then the minimum timing is reported.  For some of the tests there is some setup code that is executed by *timeit*.  I also throw in a timing on the `cleanup()` delete.

The `pipe()` function I have written decorates the test functions with the pipelining functionality by replacing the Redis r instance inside the test function with an instance of the pipeline then executing it.

## The script

{% gist 3498598 %}

## The results

	(venv)james on moz-air in ~/code/jmoz.co.uk-flask (develop)
	 $ py redis-pipe-test.py 
	* working with 10000 items
	* repeating timeit() 7 times with 1 iteration each time

	create_users()
	0.863823890686
	pipe(create_users)
	0.132288217545

	update_users()
	1.03583908081
	pipe(update_users)
	0.276007175446

	retrieve_users()
	0.896469116211
	pipe(retrieve_users)
	0.172962903976

	cleanup()
	0.829195022583

	pipe(cleanup)
	0.105372905731

You can see *clearly* that *pipelining improves the performance* of multiple calls to Redis - it's roughly about 5 times faster when using `pipeline()` so start using it!