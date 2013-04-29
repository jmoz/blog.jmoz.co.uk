---
layout: post
title: "Make Symfony respond to a HTTP PUT or HTTP DELETE request"
date: 2010-04-24 12:00
comments: true
categories: [php, symfony2]
published: true
permalink: /post/545673353
---
Whilst developing a RESTful API at work, I ran into a small 'feature' of Symfony 1.4 which required digging into the sourcecode.  I was trying to use the pecl_http HTTP class to make GET, POST, PUT and DELETE requests - the GET and POST worked fine with Symfony populating the request with the correct variables, but PUT and DELETE didn’t want to play ball leaving the request parameters empty.

Turns out Symfony requires you to specifically set a **Content-Type** Header and have it’s value be set to **application/x-www-form-urlencoded**.

So a route specified as such:

``` yaml
foo_put:
  url: /foo
  class: sfRequestRoute
  param: { module: foo, action: put } 
  requirements: sf_method: [put]
```

With an action that wants to access the content parameter:

``` php
public function executePut(sfRequest $request) {
    echo $request->getParameter('content');
}
```

Will work correctly with the following pecl_http HTTP client code:

``` php
$request = new HttpRequest('http://symfony/foo', HTTP_METH_PUT);
$request->setHeaders(array('Content-Type' => 'application/x-www-form-urlencoded'));
$request->setPutData('content=bar');
$response = $request->send();
```

The same also goes for a HTTP DELETE request (the client code will obviously be different but the Header needs to be set the same way).  GET and POST requests don’t need anything special.