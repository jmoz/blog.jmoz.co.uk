---
layout: post
title: "Organically increase your followers with the Twitter API and a little Python"
date: 2013-08-02 12:00
comments: true
categories: [python, twitter api]
published: true
---
There are many reasons why you may want to *increase your Twitter followers*: for status, engagement and for more reach are a few that come to mind.  I'm certainly in the latter - more reach == more potential hits to my blog == web domination.

If you're in the status camp you'll most likely have gone down the route of paying for followers.  You know the drill - pay x amount of dollars for x amount of followers.  The majority, if not all of these followers are trash accounts; accounts solely created to follow others for no other reason than to increase their follower count.  They won't provide traffic to your blog or engage with you.

If you're after organic followers then it requires a little more effort than simply paying for them.  Real, organically acquired followers can be valuable as they may retweet your tweets, engage with you, or follow links back to your blog.  You can painstakingly follow or retweet users in the hope that they'll check you out and follow you.  You could @message them but that's a little spammy.  

Or you could favourite certain tweets and automate the process large scale.  I'd favour favouriting over retweeting as it doesn't pollute your timeline.  Less pollution and spam on your timeline means more room for relevant content the user may be interested in, potentially resulting in a new follower.

We're going to use a mix of simple **Python** and the **Twitter API**.

## The method

- Search for tweets by related users or content, e.g. tweets containing 'James Morris' or tweets with the hashtag '#php'.
- Favourite the tweets.
- User gets a notification that their tweet has been favourited.  Our timeline stays the same (unlike retweets, favourites don't get pushed to our timeline).
- User checks out profile of the favouriter and if they see something they like, hopefully we get a new organic follower!
- Rinse and repeat for more followers.

## The results

So when I first started using a script I think my follower count was somewhere in the 400s.  After a few weeks to a month of using it - when I remembered (there's a 24hr limit) - my follower count is up about 75% to the near 700s.

Interactions are up also - some good some pointless.  I get users retweeting and favouriting my tweets and some engaging and @messaging me.  A lot of users don't realise it's just a script blanket favouriting all mentions of certain terms; these users tend to @reply back with thanks or engage in conversation.

{% img http://i.imgur.com/qe9dsmw.png %}

I've noticed a lot of new related followers, e.g. developers and programmers, so all in all the script has been pretty useful for me.  I'm looking to build on it and create a simple web app others can sign up and use also.

You need to be careful with this, as if not used honestly but in a spammy manner, [there could be consequences](http://socialtimes.com/favoriting-tweets-bad-twitter-strategy_b131137); however, you could eliminate the build-up of favourites by writing another script that loops over your favourites list and simply calls the API's `favorites.destroy`.

## The code

So first off we need a way to search for tweets.  We set up the Twitter API with keys (which you'll need to get from their developer site first).

``` python
from twitter import Twitter, OAuth, TwitterHTTPError


OAUTH_TOKEN = 'foo'
OAUTH_SECRET = 'bar'
CONSUMER_KEY = 'baz'
CONSUMER_SECRET = 'bat'

t = Twitter(auth=OAuth(OAUTH_TOKEN, OAUTH_SECRET, CONSUMER_KEY, CONSUMER_SECRET))
```

And we create a function that hits the search endpoint.  Here's an example of some of the data you can get.  The tweets are all under the key 'statuses' and contain the important tweet 'id'.

``` python
[(t['text'], t['id']) for t in example.search_tweets('web development', 5)['statuses']]

[(u'No Title: ...the latest web development and internet marketing techniques adopted nowadays.articles play a ver... http://t.co/INHI5uPfWM',
  363044527784726529),
 (u'Please ReTweet: Contact us for the best prices for Print Design, Web Design &amp;amp;amp; Development.',
  363044464652075010),
 (u'Interested in becoming a Web Developer, Programmer or Software Developer? Check out our Development Professsional Programme...',
  363042887107227648),
 (u'MIT, India based company focusing on development and testing of commercial software products. Web application, web software,desktop apps etc',
  363042582982430720),
 (u"Come meet the man behind our graphics, web development, and film projects! Jarrod Bruner's Employee Spotlight: http://t.co/OohMVtRaEp",
  363041474885070849)]
```

Riveting tweets I'm sure you'll agree.

Next we create a function that favourites a tweet and plug that into a function that searches and the loops over the results and favourites each tweet.

``` python
def favorites_create(tweet):
    try:
        result = t.favorites.create(_id=tweet['id'])
        print "Favorited: %s, %s" % (result['text'], result['id'])
        return result
    except TwitterHTTPError as e:
        print "Error: ", e
        return None


def search_and_fav(q, count=100, max_id=None):
    result = search_tweets(q, count, max_id)
    first_id = result['statuses'][0]['id']
    last_id = result['statuses'][-1]['id']
    success = 0
    for t in result['statuses']:
        if favorites_create(t) is not None:
            success += 1

    print "Favorited total: %i of %i" % (success, len(result['statuses']))
    print "First id %s last id %s" % (first_id, last_id)
```

For `favorites_create()` we try to catch the exception that is thrown when you favourite an already favourited tweet (this happens a lot for retweets).  For `search_and_fav()` we store some useful tweet id's that can be used for the `max_id` param and also count how many successful favourites we create.

Here's a gist of the full code:

{% gist 6135716 %}

And you can run it like so:

``` python
import twitter_favouriter

twitter_favouriter.search_and_fav('#foo', 10)
```