---
layout: post
title: "SplObserver, SplSubject - Removing dependencies with the Observer pattern"
date: 2011-05-17 12:00
comments: true
categories: [php, SplObserver, SplSubject, observer, patterns]
published: true
---
I work with **symfony** every day - I&rsquo;m a backend developer who mainly enjoys designing and writing domain objects, core services and APIs. &nbsp;I like to create loosely coupled objects where each object has a clear role and is composed of other objects. &nbsp;Working on a symfony app, you usually have a mix of domain objects that are used by symfony actions, interspersed with symfony specific code such as logging and sfContext type stuff. &nbsp;A common bad practice I see is symfony specific code peppered inside of domain objects that could be used elsewhere (such as inside of a Zend app or a script from the cli) but now can't as they're coupled to the symfony code.

## The Observer pattern ##

One way you can remove an unwanted dependency is to use the **Observer pattern** - the dependency is pushed from inside the subject object to the client code that initialises the subject. The subject object exposes events in the code (such as before or after certain method calls) by calling a method notify(), this then notifies all observers that were set up at run time. The subject passes an instance of itself when each observer is notified so that whatever the implementation of the observer, it has full access to the context of when notify() was called. In (somewhat) simple terms, you pull out the line of code that is a dependancy, replace it with a call to notify(), then push the dependant code into an object that implements update() and attach this observer at runtime, we will use the interfaces provided by **SPL** - the Standard PHP Library, **SplSubject** and **SplObserver**.

## The Scenario ##

Right, so I&rsquo;ve tried to articulate this but as always a code example is best. &nbsp;The scenario is as follows and is something I&rsquo;ve seen numerous times: we have a symfony action that hits a web service and for this example just spits out the response. &nbsp;There are Service classes (which I won't show an implementation for as they're simple) which basically contain some business logic, parameters, and endpoint details such as url and HTTP method. &nbsp;The Client object takes a Service object and transport object (HTTP in this case) puts them both together and sends a HTTP request to the web service endpoint and returns the response. &nbsp;Inside all of this is the need for logging so there is symfony specific code coupled to the cohesive and loosely coupled Client object.
I'm going to show some example code and then refactor it till eventually the dependency is removed and the Observer pattern is implemented. &nbsp;Obviously there are many ways to do this and many reasons for and against it. &nbsp;Do not take this as dogma - it's merely here to demonstrate an option you can take. &nbsp;And none of this code is tested, just free styled.

## The Action ##

The action simply chucks a Service and Http object into the client for it to use and calls it.

{% gist 974567 %}

## The Client ##

The&nbsp;ServiceClient takes an instance of a Service and a Http object which are injected at construction. &nbsp;There's an accessor and a mutator so the Service can be switched at runtime. &nbsp;The main code is inside call() which sets up the Http request and returns the response. &nbsp;You can also see we are logging the Service call details by getting the logger from sfContext -&nbsp;not nice.

{% gist 974596 %}

## Refactoring out sfContext ##

Nobody likes sfContext calls inside domain objects. &nbsp;Fact. &nbsp;So let's make use of dependency injection and push it to the client code. &nbsp;Here's the new action and Client.

{% gist 974611 %}

{% gist 974602 %}

That's better than before but will still have the symfony dependency in the constructor and a call to a method named info().  This could be solved by using an adapter, but who said we wanted any logging inside this class in the first place?  Services, Service Clients and Http objects are all fairly cohesive, logging is not.

## Implementing the Observer pattern ##

[Shit just got real](http://somethingjustgotreal.com).  We'll now modify the Client so it **implements SplSubject**, the interface so kindly provided by PHP's SPL.  We'll create a new Observer object that **implements SplObserver** and push the logging to it, then all that's left is to modify the action code.  So first up is the Client.

{% gist 974654 %}

You can see we are implementing the SplSubject interface where we have to declare 3 methods, **attach()**, **detach()** and **notify()**.  As we are a subject we need to maintain an array of Observers, we do this by using a hash of the object as a key to the array which allows us to easily detach() it.  notify() is our event where we loop over each Observer calling **update()** and passing them the current instance so they have full access to the context of when notify() was called.

Next up the Observer.

{% gist 974666 %}

Here you can see the object has one clear role &ndash; to use symfony's logger and the Subject that we passed in (our Client) to log details about the Service that was called.  We could easily create another Observer that uses Zend's logger or create something that has a function other than logging.  The point is we pushed the symfony dependency from inside the Client object into a separate, decoupled object.

And finally the action that brings it all together.

{% gist 974659 %}

You can see we construct our Client with 2 cohesive objects then attach an Observer directly after.  We know that when notify() is called our logger will log details of the Service that was called.  We've pushed the dependency out of the domain object and up into client code so that the domain object is no longer coupled to symfony, we could use it with Zend if we really wanted to (*but probably don't*).  As mentioned before, we could attach multiple Observers each with slightly different functionality.  All we know is that each Observer wants to do something at the point that `notify()` was called.  *Nuff said*.
