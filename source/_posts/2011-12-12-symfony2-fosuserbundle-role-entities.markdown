---
layout: post
title: "Symfony2 FOSUserBundle Role entities"
date: 2011-12-05 12:00
comments: true
categories: [php, symfony2, fosuserbundle]
published: true
---
I've been working with **FOSUserBundle Roles** recently, adding database persistence using a simple Doctrine array mapping.  We needed a better implementation where the Roles could be managed from the database and dynamically added and removed to a User through an admin interface.  I found quite a few posts on stackoverflow and the Symfony2 google group asking how best to implement **Role entities** but very few answers or solutions, and no documentation on the FOSUserBundle github page.

Symfony2's security system is one of the most complex parts of the framework.  Couple this with FOSUserBundle's weird AOP implementation and you've basically got a big massive ball ache when it comes to figuring out what's going on.  After staring through a lot of code and not really getting anywhere, I figured I'd write some tests to cover the existing functionality then refactor the User and Role classes until everything went green.

## Unit test to cover existing functionality

For reference here is the unit test I was working with.

{% gist 1423722 %}

## Creating the Role entity

FOSUserBundle provides no Role entity.  Symfony2 provides a Role object but some class members are private so we just implement the Role interface, hoping it will ensure the new Role implementation works correctly with the security system.

{% gist 1423700 %}

It's a simple object, I'm implementing a `__toString()` method so we can loop in the template over `User::getRoles()` and echo the `$role`.

## Creating the User, Role relationship

This is the User class with the Role relationship mapped.  I tried to implement the same Role functionality as FOSUserBundle.  You are restricted to certain method parameters due to the type hinting in the parent class, e.g. `setRoles()` must take an array.  I found type hinting and return type expectations in some of the symfony2 security layer code, such as:

    UsernamePasswordToken::__construct($user, $credentials, $providerKey, array $roles = array())

Because of this, I mixed up an ArrayCollection and array implementation.  You can see I also provided the `(set|get)RolesCollection()` methods to make things easier when working with doctrine.

{% gist 1423762 %}

## Modify the schema

Check the changes to the database:

    $ php app/console --env=test doctrine:schema:update --em=user --dump-sql
    CREATE TABLE security_roles (id INT AUTO_INCREMENT NOT NULL, role VARCHAR(70) NOT NULL, UNIQUE INDEX UNIQ_5A82CD6D57698A6A (role), PRIMARY KEY(id)) ENGINE = InnoDB;
    CREATE TABLE security_users_roles (user_id INT NOT NULL, role_id INT NOT NULL, INDEX IDX_71E6DDEFA76ED395 (user_id), INDEX IDX_71E6DDEFD60322AC (role_id), PRIMARY KEY(user_id, role_id)) ENGINE = InnoDB;
    ALTER TABLE security_users_roles ADD CONSTRAINT FK_71E6DDEFA76ED395 FOREIGN KEY (user_id) REFERENCES security_users(id) ON DELETE CASCADE;
    ALTER TABLE security_users_roles ADD CONSTRAINT FK_71E6DDEFD60322AC FOREIGN KEY (role_id) REFERENCES security_roles(id) ON DELETE CASCADE;
    ALTER TABLE security_users DROP roles

Either run this using the `--force` parameter or create a migration.

## Summary

I managed to get all the tests to pass:

     $ phpunit --stop-on-error --stop-on-failure -c app src/JMOZ/Bundle/SecurityBundle/Tests/Functional/SecurityTest.php
    PHPUnit 3.5.5 by Sebastian Bergmann.

    ...........

    Time: 30 seconds, Memory: 82.25Mb

    OK (11 tests, 35 assertions)

I would rather have not mixed the array/ArrayCollection implementation but was forced to do so due to the existing functionality and interfaces.  I'd have liked to have seen some other implementations or solutions but could not seem to find any.  If anyone has seen anything better please let me know.  I hope this helps someone out there.