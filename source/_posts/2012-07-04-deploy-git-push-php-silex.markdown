---
layout: post
title: "Deploy a Silex app using Git push"
date: 2012-07-04 12:00
comments: true
categories: [git, silex]
published: true
---
Up until a few days ago I used to use a small bash deployment script to deploy a few simple sites to my live box.  The process was a git archive and extract, then an rsync to the live site.  Only inspecting it recently I realised that rsync no longer sent just the changes but all of the files, I'd never noticed before as the sites were so small the deploy was over very quickly.  The rsync used to work fine before as I would deploy my current working code where the timestamps on files would match the server.  Since I started using git at home for dev, the git archive method timestamps the files with the latest commit's timestamp.  This messes up rsync.

## My use case

I've been working on a very small **Silex** project which needs to be deployed to my live box so I decided to set up a new deployment method still using git but this time simplified into a **git push** à la heroku.  I trawled through a lot of pages and posts about git deployment looking for the simplest and most effective method that would fit my use case which is as follows:

- Store Silex app in git
- Commit on develop branch and use master as the deployment branch
- Push the code to my live box
- Run an install script to setup Silex

## The process

So for my use case of *deploying a Silex application using git*, here's the technical breakdown:

- Set up git to store the Silex app and *install Silex using composer*.  Commit the composer.json and composer.lock files as these are used for the install.  Use .gitignore to ignore the vendor/ directory and composer.phar.
- Set up *password-less ssh* access to live server.
- On the live server create a *bare git repository* where your changes will be pushed.
- On the live server create a *project directory* for your code to be checked out to (this is the git working tree of your master branch checked out from the git bare repository from above).
- Create a *post-receive hook* in the bare git repository which will checkout the tree to the project directory on a successful push then execute the *Silex install script*.
- The install script will do the composer installation, installing Silex and dependencies to vendor/ if it's not there or updating it if it is already.
- Add the live server as a *git remote*.
- **git push** to the live server to deploy the code and install the app.
- Win.

## .gitignore

This will prevent git from pushing the comparatively large Silex vendor/ directory to the live box, instead we rely on the composer.json and composer.lock files which will install Silex from the live box and make sure the versions are all correct.

    vendor/
    composer.phar

## SSH password-less access

This simplifies ssh'ing to your live box and is generally a good thing to use.  This is my `~/.ssh/config`:

    Host sagat
    HostName sagat.foo.co.uk
    User myuser
    Port 22
    IdentityFile ~/.ssh/my_key

## Live server git setup

Here we create the bare git repository and then a directory for the tree to be checked out into:

    $ mkdir /home/myuser/mysilexapp.git && cd !$
    $ git init --bare
    $ mkdir /home/myuser/mysilexapp

For a good explanation of git bare repositories go [here](http://www.bitflop.com/document/111).

## Git post-receive hook

The *post-receive hook* is the most important part of this deployment method.  The hook executes on a successful `git push` from a client machine and checks out the tree to our project directory then runs the install script to update Silex.  Also of importance is the `unset GIT_DIR`.  Without this **Silex will not install** correctly from your dev machines deploy as when the live box tries to update Silex using composer, composer and some of the dependencies rely on git and so get confused and break.

Make sure the post-receive hook is executable:

    $ chmod u+x /home/myuser/mysilexapp.git/hooks/post-receive

{% gist 3048279 %}

## The Silex install script

I like to store my installation commands for a project in a separate executable bash file:

{% gist 3048301 %}

The script needs to `cd` to it's current location as when you execute the script programatically it will think it's working from `/home/myuser/`.

## Git remote for live server

    $ git remote add live ssh://sagat/home/myuser/mysilexapp.git

I like to call my remotes by names I recognise like 'live' or 'github'.  Git traditionally uses 'origin' as the default remote name so if you named it origin a `git push` would automatically push to the origin server.  If you name it otherwise you will have to do a `git push live`.

## Win

So with everything setup, here is an example of my deployment process:

    james on moz-air in ~/code/mysilexapp (master)
     $ echo foo > foo 
    james on moz-air in ~/code/mysilexapp (master)
     $ git add !$
    james on moz-air in ~/code/mysilexapp (master)
     $ git commit -m "foo"
    [master df6c85c] foo
     1 file changed, 1 insertion(+), 6 deletions(-)
    james on moz-air in ~/code/mysilexapp (master)
     $ git push live
    Counting objects: 5, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (2/2), done.
    Writing objects: 100% (3/3), 257 bytes, done.
    Total 3 (delta 1), reused 0 (delta 0)
    total 572
    drwxr-xr-x 5 web web   4096 2012-07-04 18:01 .
    drwxr-xr-x 6 web web   4096 2012-07-03 18:57 ..
    -rw-r--r-- 1 web web     57 2012-07-03 19:03 composer.json
    -rw-r--r-- 1 web web   2657 2012-07-03 19:03 composer.lock
    -rwxr-xr-x 1 web web 532630 2012-07-04 17:19 composer.phar
    -rw-r--r-- 1 web web      4 2012-07-04 18:01 foo
    -rw-r--r-- 1 web web     22 2012-07-04 17:19 .gitignore
    -rwxr-xr-x 1 web web    101 2012-07-03 19:03 install.sh
    drwxr-xr-x 3 web web   4096 2012-07-03 19:03 lib
    drwxr-xr-x 6 web web   4096 2012-07-03 23:12 vendor
    drwxr-xr-x 2 web web   4096 2012-07-03 19:03 web
    #!/usr/bin/env php
    All settings correct for using Composer
    Downloading...

    Composer successfully installed to: /home/myuser/mysilexapp/composer.phar
    Use it: php composer.phar
    Installing dependencies from lock file
    Nothing to install or update
    Generating autoload files
    To ssh://sagat/home/myuser/mysilexapp.git
       ad5e1c1..df6c85c  master -> master