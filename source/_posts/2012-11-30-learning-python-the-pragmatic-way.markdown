---
layout: post
title: "Learning Python the pragmatic way"
date: 2012-11-30 12:00
comments: true
categories: [flask, git, gunicorn, python, redis, supervisor]
published: true
---
Around 5 months ago I was on a motocross bike making my way to the top of the Bokor mountain range in Cambodia.  I'd chosen to take time off as a contract PHP developer in London and travel around south east Asia - Thailand, Cambodia and Malaysia.  I spent the first half of the year there and discovered, partied, relaxed and forgot about work.  Whilst there I decided that I needed something new to learn when I arrived back in the UK; I'd recently just learnt and professionally used Symfony2 for PHP, so I decided to change things up a bit and learn either Python or iOS development.  I chose **Python**.

After arriving back in the UK and just managing to catch the summer, I was in no rush to go back to work so spent around 3 months building a Python app I would eventually use as my homepage, [jmoz.co.uk](http://jmoz.co.uk).  I tried to spend a few hours a day on it, but to be honest, I spent a lot of time enjoying my life and free time; I even went back to Thailand (I got bored of working in my kitchen and in coffee shops around Shoreditch, besides the flight was a deal) for a month where I properly relaxed and kept fit, training muay Thai.  Whilst there I'd do a few hours coding here and there in coffee shops, in bars and restaurants and even beaches! (It's amazing what you can do when you're really into something).

I'd say with a month of solid work on a project you're passionate about, and with previous programming experience, you could learn Python from scratch to an intermediate level and gain knowledge of the ecosystem and stack options.

This post is about what I've learnt, how I did things and how my app works (and also an explanation of what the hell I've been doing since December 11).

## Learning Python the pragmatic way

There's a well known online book, [Learn Python The Hard Way](http://learnpythonthehardway.org/).  I was advised by a fair few people to read this at the start of my quest to learn Python.  It has 52 exercises.  I think I managed to look at a few then decided against going through the entire book.  Personally, I don't have the time and the patience to sit through pages and pages of code and syntax and then expect to remember it, never mind put it all together and start developing.

For example, [exercise 37: symbol review](http://learnpythonthehardway.org/book/ex37.html).  There's nearly 100 different symbols to look at and the exercise suggests "Spend about a week on this, but if you finish faster that's great." - too much, too long.

For me, the easiest and *most pragmatic way to learn Python* is not by reading through pages of reference documentation and syntax, but to *just start coding*.

Pick a task, an app or project, or something you're passionate about and take the bold step of building it entirely in the new language.  You'll use your existing knowledge of programming paradigms and design patterns from your previous language to get through it.  

For the first week or so of transitioning from PHP to Python my history in Google would look something like, "how to do x from php in python":

- foreach loop in Python
- Python concatenate string
- Python array

Each core concept you need to learn will rapidly branch out to other language constructs and features, e.g. discovering the equivalent of PHP's array opens you up to Python's world of dicts, lists, sets, tuples.

I will admit, at first I found the learning curve rather steep.  Reflecting on it now, it was due to the masses of reference and API documentation and long articles or books such as "Learn Python the hard way".  There was just too much information to take in at once.  I couldn't find any posts which gave a really good lo-down of the Python ecosystem (I [found this](http://mirnazim.org/writings/python-ecosystem-introduction/) once I'd finished my project).  Once I decided to just start coding, I found the pace I learned at increased rapidly.  And there was a noticeable point where everything 'just clicked' into place; for me this was once I'd figured out data structures, loops and the object/class implementation.

## Use Stackoverflow!

[Stackoverflow](http://stackoverflow.com) is undoubtedly my go-to resource for help on Python.  Countless times I've scoured the official Python docs for detailed info and hammered Google, but then turn to SO and find someone with the exact same problem and more often than not, a solution to go with it.

## Use your friends and colleagues

I'm fortunate enough to have worked with some clever people that know more things than I.  Often it's far easier for me to throw a quick tweet out asking for advice, or Skype someone.  I'll favour advice from friends over posts or articles I find.  So thanks to [@rtt](https://twitter.com/rtt), [@glenswinfield](https://twitter.com/glenswinfield) and anyone else that gave me tips.

A prime example is one of the first Python packages I used - [requests](http://docs.python-requests.org/en/latest/).  I tweeted something along the lines of "Something analogous to PHP's HTTPRequest, in Python?".  I got a one word reply of "requests" which at first I thought was a piss-take purely because of who it came from.  Turns out it's really simple to make HTTP requests using a nice API.

Here you can see how some first hand advice saved me time and introduced me to a nice api; first the PHP which I've used before, followed by the Python:

{% gist 4154192 %}

## Get Pythonic

Coming from PHP to Python is kinda weird.  PHP started off really messy and bad but then started making movements towards more structured code, inheritance, interfaces, composition, vast nested directory structures and namespaces.  The language, library and framework design started getting more complex and over-engineered (*cough* Symfony2 *cough*); it started to look and behave like Java.

Python throws that out the window.  Once you start looking at other peoples work you realise coders use Python very dynamically and less structured as the modern PHP community does.  And it kinda makes sense.  Are all those interfaces *really* necessary?

I found myself trying to do things the Python way, and not how I would do it in PHP.  I duck-typed.  I liked list comprehensions and generators.  I started googling for '[Pythonic](https://www.google.co.uk/search?q=what+is+pythonic)' examples.  import this:

{% gist 5330317 %}

## My project

For a long time I have owned my domain but no homepage or app was present.  I am an avid user of social media services and apis such as Foursquare, Posterous, Instagram and Twitter.  I decided that I would use these services rather than create my own, and aggregate them into one single service which would be a representation of me.  People come to my homepage to find out about me and so I'll give them the public things I share (this could be a bad idea).

So I had project I was passionate about.  Learning Python is also something I'd wanted to do for some time.  I'd also been hearing about Redis and NoSQL so decided to give that a spin.  

## First steps

One of the first things I knew I would need for my app was to hit Foursquare's api and pull my checkins.  I've done this before in PHP so it was just a matter of finding a decent client library for Foursquare.  I went with the library recommended by Foursquare, conveniently named [foursquare](https://github.com/mLewisLogic/foursquare) and easily installed with `pip install foursquare`.  Pulling my checkins was as easy as:

{% gist 4158057 %}

This taste of Python and Foursquare's API pushed me on to writing some simple parsing of the response and transforming the core parts of the checkin data (id, location name) into 'items' for my backend to save then display later in a timeline.

## Using Redis as a MySQL replacement

I've grown tired of setting up databases, using yaml schemas to create tables, performing complex joins and SQL to get data out the db.  This is also partly due to the fact the last couple of contracts I've done have been web service based and made extensive usage of REST apis.

I looked and played with [Redis](http://redis.io/) and found I could replace an item table or schema with an item [hash](http://redis.io/commands#hash).  And that hash could be dynamic and easily updated or changed.

Oh and also you could just start adding hashes into the db without any setup code.  Easy.

I knew that a core part of my app would be a timeline on the index page, of items in descending date created order.  Initially I used a [list](http://redis.io/commands#list) and did some sorting then rpush'd item ids onto the list.  This worked well but seemed rigid.  Later on, I found out about, and refactored to a [sorted set](http://redis.io/commands#sorted_set) implementation where the score was the date created and the member was the item id.  Now I could pull out the items by date created in descending order with [ZREVRANGE](http://redis.io/commands/zrevrange).

{% gist 4175557 %}

The library I used, redis-py also allowed [pipelining](https://github.com/andymccurdy/redis-py#pipelines) which greatly [sped up Redis](http://blog.jmoz.co.uk/python-redis-py-pipeline).

## Choosing a framework

When I first started with Python, Django was the de facto and only framework I'd heard of.  I started to play with it and got as far as a 'hello world' from a controller (view).

The further I delved into Django, I started to realise that my app would be mainly domain logic and would use little features from Django - all I really needed was some simple page controlling and a template layer.  I had read that Django was pretty heavy and was starting to find this out.  It's about this time that I was made aware of a micro framework named Flask.

[Flask](http://flask.pocoo.org/) looked perfect for what I needed, and I kept hearing it mentioned so decided to give it a blast.  With literally a few lines of code I'd managed the same as what I'd got in Django, in far less time and with a lot more ease.

It also looked incredibly similar to [Silex](http://silex.sensiolabs.org/) (or the other way round) - a PHP micro framework I had just been using to create an API.

If you're looking to get into Python and need a starting framework, I'd highly recommend you try Flask, below is the minimal code that powers the index of [jmoz.co.uk](http://jmoz.co.uk).

{% gist 4155241 %}

Flask also supports extension, such as [Flask-OAuth](http://packages.python.org/Flask-OAuth/), which I'm using to provide login via Twitter.  Below you will find an example of my callback for Twitter's OAuth process.

{% gist 4155334 %}

## My stack lo-down

So after getting some example code working and playing with Redis, I'd delved far enough into Python to start implementing my stack.  Through the advice of friends and colleagues I ended up with the following stack:

- [Python](http://www.python.org/download/releases/2.7.3/)

For everything.

- [Virtualenv](http://www.virtualenv.org/en/latest/)

Allows each project to use it's own environment with a specific Python version and all packages bundled into the venv.  Makes it very easy to deploy the same environment on your live box.

- [Git](http://git-scm.com/)

For version control and deployment.

- [Flask](http://flask.pocoo.org/)

The micro framework that puts everything together.

- [Redis](http://redis.io/)

I use Redis as the NoSQL database store.  I store items as hashes and use sorted sets for a timeline.

- [Nginx](http://nginx.org/)

Nginx proxies http requests to Gunicorn.

- [Gunicorn](http://gunicorn.org/)

Gunicorn is the WSGI server that runs the app.

- [Supervisor](http://supervisord.org/)

Controls the Gunicorn process and allows easy restarting and updating of the app.

## Nginx proxying to Gunicorn

Nginx is very easy to use in combination with Gunicorn - just proxy all requests to the Gunicorn server running on port 8000.  In the example configuration below you can also see how I redirect www. requests to the canonical domain jmoz.co.uk.

{% gist 4149806 %}

## Supervisor config for Gunicorn

Supervisor can be used to monitor the Gunicorn process; starting it at boot and making it easy to restart with `supervisorctl restart gunicorn-jmoz`.  The configuration below specifies the path to the gunicorn bin file in my venv.  Without the path to the venv the Python environment and dependant packages will not load correctly.

{% gist 4149833 %}

I store this file inside my project then create a symlink like so:

    web@sagat:~$ ll /etc/supervisor.d/
    total 8.0K
    4.0K drwxr-xr-x  2 root root 4.0K Nov 22 18:46 .
    4.0K drwxr-xr-x 88 root root 4.0K Nov 26 09:50 ..
       0 lrwxrwxrwx  1 root root   49 Nov 22 18:46 gunicorn-jmoz.conf -> /home/web/sites/jmoz.co.uk/gunicorn-jmoz.conf

For this directory.d/ style of configs to work, make the end of `/etc/supervisord.conf` look like:

    [include]
    files = supervisor.d/*.conf

## Mac (dev) environment vs Debian (live) environment

I develop on my Mac using brew, pip and virtualenv.  This all works great and is a pretty standard setup.  The problem I have is that my [Linode VPS](http://www.linode.com/?r=c66cb6051be162bdbda98e9c4f588768374f502c) is running Debian stable (Squeeze).  Debian packages are always out of date due to their strict testing and acceptance process so my Mac's Python 2.7.1 does not match up with Squeeze's 2.6.6.

This may not be a problem for you, but for me it broke my unit tests.  When I deployed my code, I ran my tests on the live VPS to confirm everything was working but I got errors in the `unittest` package due to new features introduced in Python 2.7.

I found a solution using [pythonbrew](https://github.com/utahta/pythonbrew) which lets you install different Python versions (environments) in your home directory, and toggle between versions on the fly.  On my VPS I managed to get 2.7.3 running (I still need to update locally) which fixed all my tests.

On my Mac I use virtualenv directly, with my venv inside my project directory (it could be moved to home) which all works fine.  To activate my Mac venv I do:

    james on moz-air in ~/code/jmoz.co.uk-flask (master)
     $ . venv/bin/activate
    (venv)james on moz-air in ~/code/jmoz.co.uk-flask (master)
     $

But on Debian I have to go through pythonbrew and activate the venv (stored in home) like so:

    web@sagat:~$ pythonbrew venv use jmoz
    # Using `jmoz` environment (found in /home/web/.pythonbrew/venvs/Python-2.7.3)
    # To leave an environment, simply run `deactivate`
    (jmoz)web@sagat:~$

These are the 2 main differences between environments.  Once the venv is activated, everything behaves the same (so far).

## Deploying using Git, installing, then restarting Gunicorn

I do a `git push live master` then a [git post-receive hook](http://blog.jmoz.co.uk/deploy-git-push-php-silex) takes care of checking out my master branch and running an install script contained in the project.  

Here's the post-receive hook which deploys the master branch only:

{% gist 4149929 %}

The install script first has to set up pythonbrew (Debian specific, I don't use pythonbrew on my mac) so that the `pythonbrew` command will work - this is because the web users .bashrc is not executed during the `git push` and post-receive hook.  If you were to look at my web users .bashrc, you'd see:

    [[ -s "$HOME/.pythonbrew/etc/bashrc" ]] && source "$HOME/.pythonbrew/etc/bashrc"

Which you are told to add as part of the pythonbrew installation process.

The install script then looks like this:

{% gist 4149934 %}

This will activate the virtualenv so we get the correct version of Python, then we install all dependencies into said venv using pip and a requirements file (which is generated during dev with `pip freeze > requirements.txt` after installing any new packages).

Once pip has finished installing any new packages or updating them we restart the Gunicorn process using supervisorctl so that any changes to Python files are picked up.

Once the restart has finished we should have a successful deployment of our Python app all from a simple `git push live master`!

## Further reading

There were a handful of posts and articles that I found really useful, not just one line code examples.

- [The Python tutorial, official docs](http://docs.python.org/2/tutorial/classes.html#a-first-look-at-classes)

Use the tutorial first - find a section you need more info on, e.g. classes.

- [The Python library, official docs](http://docs.python.org/2/library/stdtypes.html#mapping-types-dict)

Use the library (or API) for detailed functions and methods, e.g. find out what operations a dict supports.

- [Dive Into Python Must Die](http://oppugn.us/posts/1272050135.html)

You can learn Python without reading a book.  A lot of them are dated; Google and Stackoverflow are better options.  The post above is a great example, and also worth reading purely for the use of the phrase, "neckbeard ass".

- [Learn Python The Hard Way](http://learnpythonthehardway.org/)

This is a great resource if you want to learn from scratch.  Personally I'd suggest to just start coding then consult certain exercises when needed.

- [Building websites in Python with Flask](http://maximebf.com/blog/2012/10/building-websites-in-python-with-flask/)

Check out Flask, a nice micro framework.

- [Using Pythonbrew and Virtualenv(with pip) for creating sandboxed Python development environments](http://suvashthapaliya.com/blog/2012/01/sandboxed-python-virtual-environments/)
- [Tools of the Modern Python Hacker: Virtualenv, Fabric and Pip](http://www.clemesha.org/blog/modern-python-hacker-tools-virtualenv-fabric-pip/)
- **[Python Ecosystem - An Introduction](http://mirnazim.org/writings/python-ecosystem-introduction/)**

Learn about virtualenv, pip and Python's ecosystem.  That last link seems like an absolute godsend, and I'd recommend you read it first; unfortunately I only found it after I'd spent countless hours researching and learning things and after I'd finished my app!

## What I've learnt

The easiest and most practical way to learn a language is to build an app, that you're passionate about.  That's how I learned PHP about 5 years ago - I built some crappy name-in-a-chuck-norris-fact image machine thingy which ended up on a dedicated server with over 1000 visitors a day.  It's crazy to think I started there and ended up somehow at BSkyB getting paid to write PHP.

I'm happy with how I progressed with Python.  I know enough now not to be scared at the sight of unknown Python code.  I'm actively discovering and learning new things every day and I love it.  I *might* prefer it to PHP.

I intend to develop all personal projects in Python and improve my knowledge to a level where I can be professionally employed as a Python developer. Fancy testing me?  [Get in touch](http://www.linkedin.com/in/jwmoz).

Oh, and visit my app, **[James Morris](http://jmoz.co.uk)**.

**Update:** My core intention with this was to show a basic stack and examples for newcomers. The surplus references to 'just start coding' and LPTHW etc is an attempt to reference the recognisable title (which I hoped would generate interest ;)  More on [hacker news](http://news.ycombinator.com/item?id=4853019).  [Follow me](https://twitter.com/jwmoz) for sort of tech related tweets.