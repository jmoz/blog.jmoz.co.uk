---
layout: post
title: "Symfony2 Warning: session_start(): The session id is too long or contains illegal characters..."
date: 2011-11-30 12:00
comments: true
categories: [php, symfony2]
published: true
---
Whilst trying to set up a new test environment I've run into another lovely Symfony2 bug/error.  I have a functional test with a body as such:

    public function testFoo()
    {
        $client = self::createClient();
        $client->request('GET', '/foo/');
        $this->assertEquals( 403, $client->getResponse()->getStatusCode() );
    }

FYI I'm trying to test access roles are working correctly so I should get a 403 forbidden.

And my relevant config_test.yml:

    imports:
        - { resource: config_dev.yml }
        - { resource: parameters_test.ini }

    framework:
        router:   { resource: "%kernel.root_dir%/config/routing_test.yml" }
        test:     ~

Upon running this test I get the lovely error:

    Warning: session_start(): The session id is too long or contains illegal characters, valid characters are a-z, A-Z, 0-9 and &#039;-,&#039; in /home/james/code/bonsai/admin/vendor/symfony/src/Symfony/Component/HttpFoundation/SessionStorage/NativeSessionStorage.php line 87 (500 Internal Server Error)

After messing with the config and looking around, turns out this is a known issue.  For whatever reason, my config was missing some required session parameters for testing.  Have a look at [this issue](https://github.com/symfony/symfony/issues/1759) for more info.  To fix it, I had to change the session storage method params in config_test.yml from native (inherited) to filesystem:

    framework:
        router:   { resource: "%kernel.root_dir%/config/routing_test.yml" }
        test:     ~
        session:
          storage_id: session.storage.filesystem

Now, the exception is gone and the test runs:

     $ phpunit --stop-on-error --stop-on-failure -c app src/Foo/Bundle/SecurityBundle/
    PHPUnit 3.5.5 by Sebastian Bergmann.

    ..F

    Time: 1 second, Memory: 29.75Mb

    There was 1 failure:

    1) Foo\Bundle\FooBundle\Tests\Functional\SecurityTest::testFoo
    Failed asserting that <integer:302> matches expected <integer:403>.

    /home/james/code/Foo/Bundle/SecurityBundle/Tests/Functional/SecurityTest.php:59

    FAILURES!
    Tests: 3, Assertions: 4, Failures: 1.

Now on to the next problem...