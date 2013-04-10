---
layout: post
title: "A Symfony2 console Command and the Foursquare API venuehistory"
date: 2011-09-05 12:00
comments: true
categories: [php, symfony2, foursquare, api]
published: true
---
I've been playing with the **Foursquare API** recently, I'm attempting to get a new homepage built and want to display a map of where I hang out. I use Foursquare quite a bit so wanted to get the locations from their API then plot them on Google maps.

There's a **venuehistory** endpoint that gives you a massive list of all the venues you've checked into with details such as the venue name, location including lat and lng coordinates, and the count of times you've checked in.

Here's a screenshot of their json response.

{% img http://dl.dropbox.com/u/25378206/Symfony2_4sq/Selection_004.png %}

I want to pull this into my own database so I have a local copy of it and can play around with it. I'm using **Symfony2** so I've created a **console command** which I can run on a cron, or on demand. I decided to do it this way as mainly a learning exercise and also this is a bit better than a standard php script.

The command hits the endpoint with my Oauth access token (outside of the scope of this article, but see [the docs](https://developer.foursquare.com/docs/oauth.html) for details) and then simply loops over the response and creates a load of Checkin entities which the entity manager then persists.

{% gist https://gist.github.com/1191678 %}

The command takes an optional argument for the access_token or otherwise defaults to a parameter. There's also an optional flag to disable the truncation of the checkins table. You can see how to set these up along with the help text in the configure() method. Output is handled by the call to `$output->writeln()`` and you can see how highlighting works in the screenshot of the output.

{% img http://dl.dropbox.com/u/25378206/Symfony2_4sq/moz%40mz-904%20-%20byobu_003.png %}

I may write a future post showing how to push the Foursquare code into a service based implementation so look out.