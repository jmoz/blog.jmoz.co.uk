---
layout: post
title: "Virgin Media - a long tale of Super Hub trouble"
date: 2011-07-10 12:00
comments: true
categories: [virgin media, superhub]
published: true
---
DISCLAIMER: I now (March 2013) work for Virgin Media as a contract PHP Developer.  The views below are on Virgin's Superhub service and do not reflect my views on the project I am working on (which is a great project) or any of Virgin's development.  These views still stand and I will not be censored.

## Update 9

Wellsy rang again in the morning as it's been intermittent again.  Apparently if we log a complaint 7 days in a row then the contract is viable to be cancelled.

Found out that their big internal expansion project in this area that should allegedly fix the issues is on the 6th of March 2013.

## Update 8 (Tuesday 20th November, 16:15)

[Intermittent again](https://twitter.com/jwmoz/status/270924930806329344) so using the iPhone's 3g.  Just found out our neighbour had Virgin also but sacked them off for Sky because of the problems.

{% tweet http://twitter.com/jwmoz/status/270924930806329344 %}

## Update 7 (Thursday 15th November, 14:20)

Gone down again, been down since 2pm.  Both Wellsy and I are working from home today.  He's on the phone to them again with the usual:

"Just look at our account for how much it goes down.  This is unacceptable."

After a lengthy phone call of at least 25 minutes where he had to elevate to speak to someone more useful, here's what we've got:

* Techy guy fobbed us off to customer services saying the issue would not be resolved till mid Nov.
* CS had no access to tech data so did not understand why tech guy forwarded to them.
* CS forwarded to tech guys at Swansea who were "furious" because the initial tech guy was referring to an issue we logged 3 months ago and was still not resolved.
* This guy said we have 2 options; get rid of Virgin, or reduce 60Mb to 30Mb to pay less (!!??! this was purely to reduce the cost, dropping down does nothing for the service).  Contractually we may be able to  get out of it, but in the T&Cs area outages are not included, it needs to be bad service to the household - in our case it's the area that's the problem.
* They have an internal planning project to increase the service in the area to meet the demand (so it's a known problem).
* Over-demand in the area which they are doing nothing about till next year.
* Said they could try to credit our account per month, but in reality we know we will get just a few quid for this, which isn't worth it - our issue is the shitty service.
* They have an SLA but they also have Ts and Cs which cover the SLA.  Area outages or force of nature are not included.  Force of nature obviously makes sense but an area outage, for example what we are experiencing, is down to them and their over subscribing of the area!?  What is covered is outages to a single household over periods of time i.e. more that 3 outages in a month.

Ok so the chap on the phone has confirmed that the problem is **over-demand in our area** and not specifically our household.  This means that *Virgin are knowingly over-subscribing a sub standard service*.

As far as I can see, we'll get bugger all from them, so we now need to decide whether to just leave the service in the hope it will improve at the start of the year, or move to another provider.  Personally I want to escalate this to someone further up and get their opinion on our service and what they can do.  It's just so unacceptable it's unbelievable.

The service has been down intermittently now for 2 1/2 hours, ongoing.  Wellsy who works for SAP has had to go to our mates office for the internet, whereas I've taken a break from some Django hacking and am now resorting to my trusty iPhone's 3g.

{% tweet http://twitter.com/jwmoz/status/269103305018454016 %}

## Update 6 (Friday 26th October, 13:00)

The internet has been fine for a while (although I have been away for a month) but we spotted a Virgin van outside the other day.  Since they came, it's been dropping out continually throughout the day.  I've noticed it whilst I've been developing a script that makes HTTP requests and which constantly keeps throwing HTTP connection exceptions.  Occasionally you'll notice it in the browser where I'll get the bland grey Google Chrome error page, but then on the next request everything will be working.

Looks like another phone call is in order.

"You know you sent the guys round the other day to fix the internet?"

"Yes, sir."

"Well, they broke it."

## Update 5 (Monday 20th August, 12:35)

Internet just dropped out again.  No DNS, can't reach anything outside of the router.

## Update 4 (Wednesday 1st August, 14:35)

So since the last update it's gone down a few times more.  An engineer just turned up and fitted us some cabling in and around the box at the front of the house.

{% oembed http://www.flickr.com/photos/jwmoz/7691136070/ %}

So after talking to him about the situation he tells me there's a network problem in the area they think, around N1, apparently NW is even worse.  Sounds like they're trying to replace cables and fix things locally to sort it all out.

Ironically Wellsy has just come downstairs moaning that now he's replaced the cables it's all gone a bit slow.

## Update 3 (Wednesday 11th July, 22:17)

At around 20:00 tonight **Virgin media superhub went down** AGAIN.  My housemate rang the usual number which he said took about 6 minutes to speak to someone, and then it trolled him through a load of menus and kept cutting out during the password questions:

> "Please enter the CRRRRFFPPPFFCCRRRR letter of your password.."

He got to some sort of badly pronounced automated notice saying that there was errors in the "E8" or "A8" area.

It's just after 10pm now and the modem has started working again.

## Update 2 (Tuesday 10th July, 16:55)

The engineer is here now fitting the new Tivo so at least they came through with that, he's a decent guy, I've been talking to him about the service and he says he's heard of problems in the Dalston area and that they should be able to find out what is wrong and fix it rather than sending us around in circles.

Here he is installing the Tivo.

{% oembed http://www.flickr.com/photos/jwmoz/7543712294/in/set-72157630514632344/ %}

Oh god it's crashed already...

{% oembed http://www.flickr.com/photos/jwmoz/7543713602/in/set-72157630514632344/ %}

Just before he left, the engineer suggested we remove the filter on the superhub modem which was fitted some time ago to try and fix the power issues.  We're gonna try this and see how it goes (shouldn't the engineer be doing this rather than us dismantling the modem?)

## Update 1 (Tuesday 10th July, 13:08)

So I tweeted that virgin were down again in the morning and I've just received a [reply from @virginmedia](https://twitter.com/virginmedia/status/222656337744625666) which is more than likely a bot that scrapes twitter for 'virgin media down'.

{% img http://imgur.com/vFQ8ol.png %}

The 'smartphone friendly link' 302s to some pointless page telling me they're "making some changes".  Great.  What changes?  How long for?  How often are these changes?

{% img http://imgur.com/Gzi6zl.png %}

Although the service seems to be back up and running I have no idea what happened or really why it happened and I think [some twitter guy at Virgin just tried to confuse me](https://twitter.com/virginmedia/status/222663911651020801) by responding with some non contextually specific question.  And how am I meant to know if "making some essential changes" which is written on YOUR website is about the website or area.  So basically their response on twitter hasn't helped and instead served to confuse me.

{% img http://imgur.com/L9i6el.png %}

I pointed them to both of my posts about the service hoping they'll read it.  If they don't maybe I'll have to create a script that tweets them "virgin media is down" and stick it on a 1 min crontab.

# Virgin aren't great

![Y U NO WORK](http://imgur.com/TJLZy.jpg)
    
You can read my original post at [VIRGIN MEDIA SUPER HUB A.K.A. HOW VIRGIN MEDIA EFFED MY LIFE](http://blog.jmoz.co.uk/virgin-media-super-hub-aka-how-virgin-media-e) to get some context on this.

**Virgin Media superhub is down again**, Tuesday 10th July 2012 at around 10:00am.  I think we'll sack it off today and go to Haggerston Espresso Room for a coffee and somewhere where the goddamn internet works, or last resort tether my iPhone and use the 3g connection which no doubt will be better than Virgin.

I've been meaning to update my original post about [Virgin media superhub being rubbish](http://blog.jmoz.co.uk/virgin-media-super-hub-aka-how-virgin-media-e) for a long time now, since then the service has REMAINED BAD and we have had constant problems.  Engineers have been sent out, they all feed us different stories and presume we know jack all about the internet.  The only decent one that came (of about 10) fed us some bullshit about the power band being too wide and then adjusted it but it still didn't work reliably.  Actually he widened it where as the one before him unwidened? squeezed? it.

It went down last week and Wellsy ended up on the phone to them AGAIN; there must be at least 20 calls logged on our account, it's clear we have a problem yet it's never resolved and we get sweet fa compensation for it.

They're back up now as I'm writing this at 10:33am.

So after it going down last week we also found out that the deal we had is now over and the monthly cost has gone up.  Obviously we'd had enough of this bullshit and Ryan gave them a ring on the phone and shouted at them for about half an hour with the sole intention of leaving and going to Sky.  I can probably get a cheap deal there as I used to work for BSkyB and have a few contacts still at the place - they're constantly doing deals for family and friends so there's a very strong case for us to move there. (They're back down again now at 10:43am) Anyway they ended up offering us some upgrade package - we get Tivo and a larger capacity box, the speeds get upgraded to some alleged monumental amount which we will never achieve and one of the rooms is getting an extra connection and a Tivo box.  Oh and I think the price stays roughly the same.  So not only do they provide us with a shit service, they abuse and take advantage of our kind hearted nature and blag us to stay with upgrades.

Ironically they're meant to be installing it today some time and the service is all over the place.

Wellsy has just reminded me to moan about their FUCKING REMOTE! Useless piece of crap busted and started eating the batteries like they were chocolate biscuits; we ended up BUYING A NEW ONE OFF EBAY.  You can stick that on top of the shitty service compensation you already owe us that has started from today.

Last point I want to note is that all of this time when we have rang them they've tried to push it under the rug telling us there's no problem and treating us like internet users on the same level as my Dad, e.g. "have you tried turning it on and off again".  Only once did one of the telephone guys admit there might be a **problem in the area**.  So the next day we happen to find a Virgin engineer round the corner with the box open, we spoke to him briefly and he told us that there's a problem in the area and I'm sure either he or the telephone guy mentioned **OVER SUBSCRIPTION** to the service, not our goddamn shitty superhub modem wireless router thing, **THE PROBLEM IS IN THE SERVICE**.

So now I've written this I will try and keep it updated with every time they mess up in the hope that some day they will actually provide a reliable service.  Right, now I'm off to upload this post USING MY IPHONE'S 3G CONNECTION.