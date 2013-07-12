---
layout: post
title: "PHPUnit mocking and method chaining"
date: 2013-07-05 14:42
comments: true
categories: [php, phpunit, symfony2]
published: true
---

I've been given the task of **unit testing Symfony2's security layer**, which at first seems daunting, but in reality with a clever bit of PHPUnit mocking, it's actually quite simple.

Symfony2 makes heavy use of method chaining.  In the security listener it's common to see such code as:

``` php
<?php
$this->securityContext->getToken()->isAuthenticated()
```

To mock this you might think to do something like so:

``` php
<?php
$token = $this->getMockBuilder('App\FooBundle\Security\Authentication\Token\UserToken')
    ->disableOriginalConstructor()
    ->getMock();
$token->expects($this->any())
    ->method('isAuthenticated')
    ->will($this->returnValue(true));

$securityContext = $this->getMockBuilder('Symfony\Component\Security\Core\SecurityContext')
    ->disableOriginalConstructor()
    ->getMock();
$securityContext->expects($this->any())
    ->method('getToken')
    ->will($this->returnValue($token));

$this->assertTrue($securityContext->getToken()->isAuthenticated());
```

This is valid and should pass.

But there's a better way to do this without having to create the stub `UserToken` object.  We can add a method to the mocked `SecurityContext` - `isAuthenticated()` which will return `true`.  We then tell the call to `getToken` to return an instance of it's `self` - the mocked `SecurityContext` which has our stub method `isAuthenticated()`:

``` php
<?php
$securityContext = $this->getMockBuilder('Symfony\Component\Security\Core\SecurityContext')
    ->disableOriginalConstructor()
    ->setMethods(array('getToken', 'isAuthenticated'))
    ->getMock();
$securityContext->expects($this->any())
    ->method('getToken')
    ->will($this->returnSelf());
$securityContext->expects($this->any())
    ->method('isAuthenticated')
    ->will($this->returnValue(true));

$this->assertTrue($securityContext->getToken()->isAuthenticated());
```

## Using the at() matcher

In the process of digging into PHPUnit's internals I found a couple of discrepancies with the docs which I've documented below.

The matcher `PHPUnit_Framework_MockObject_Matcher_InvokedAtIndex at(int $index)` documentation states:

> Returns a matcher that matches when the method it is evaluated for is invoked at the given $index.

Which to me implies that the following code would behave as expected:

``` php
<?php
$mock = $this->getMockBuilder('Foo')
    ->disableOriginalConstructor()
    ->getMock();
$mock->expects($this->at(1))  // on index 1, 2nd call to foo()
    ->method('foo')
    ->will($this->returnValue('bar'));

$this->assertEquals('bar', $mock->foo()->bar()->baz()->foo();
```

When in reality, the correct way to use `at()` is not on a per-method basis, but on all methods:

``` php
<?php
$mock = $this->getMockBuilder('Foo')
    ->disableOriginalConstructor()
    ->getMock();
$mock->expects($this->at(3))  // on index 3, 4th method call
    ->method('foo')
    ->will($this->returnValue('bar'));

$this->assertEquals('bar', $mock->foo()->bar()->baz()->foo();
```

## Unit test demonstrating chaining

For reference, here is the unit test I was working with when testing the behaviour of PHPUnit.

{% gist 5928773 %}
