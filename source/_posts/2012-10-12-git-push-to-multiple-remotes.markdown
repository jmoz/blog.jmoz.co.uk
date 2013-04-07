---
layout: post
title: "Git push to multiple remotes"
date: 2012-10-12 12:00
comments: true
categories: [git]
published: false
---
I've been in Thailand for the past month but whilst there I was working on an app and regularly committing to my local git.  The fear of getting my Mac stolen or lost prompted me to push my work to my remote vps for a backup.

It's always better to have a couple of separated backups so I could potentially use Github, but my app is private and I don't have a Github pro account with private repos.  Fortunately Bitbucket can provide unlimited private repos so I decided to give that a try.

## Git push to multiple remotes

If you add 2 remotes in git, when you do a `git push` the code will only go up to one remote.  The trick is to set up a pseudo origin 'merged-remote' so that when you `git push origin master` the code will go up to both remotes.

First make sure you've added both your remotes in the regular fashion, then edit the config so there are 2 `url` items for your new merged-remote origin:

	 $ cat .git/config 
	[remote "my_vps"]
		url = ssh://my_vps/home/web/jmoz.co.uk.git
		fetch = +refs/heads/*:refs/remotes/my_vps/*
	[remote "bb"]
		url = https://jmoz@bitbucket.org/jmoz/jmoz.co.uk.git
		fetch = +refs/heads/*:refs/remotes/bb/*
	[branch "develop"]
		remote = bb
		merge = refs/heads/develop
	[remote "origin"]
		url = ssh://my_vps/home/web/jmoz.co.uk.git
		url = https://jmoz@bitbucket.org/jmoz/jmoz.co.uk.git

Now when I do a `git push origin develop` my branch goes up to both of them:

	(venv)james on moz-air in ~/code/jmoz.co.uk-flask (develop)
	 $ git push origin develop 
	Counting objects: 42, done.
	Delta compression using up to 4 threads.
	Compressing objects: 100% (33/33), done.
	Writing objects: 100% (33/33), 4.92 KiB, done.
	Total 33 (delta 16), reused 0 (delta 0)
	To ssh://my_vps/home/web/jmoz.co.uk.git
	   a567eac..97f5758  develop -> develop
	Counting objects: 42, done.
	Delta compression using up to 4 threads.
	Compressing objects: 100% (33/33), done.
	Writing objects: 100% (33/33), 4.92 KiB, done.
	Total 33 (delta 16), reused 0 (delta 0)
	remote: bb/acl: jmoz is allowed. accepted payload.
	To https://jmoz@bitbucket.org/jmoz/jmoz.co.uk.git
	   a567eac..97f5758  develop -> develop

This is perfect for my current workflow where I want a backup of my work in multiple locations.  Later on in development I could look at using the bitbucket remote as a backup and my vps box as my deployment box making use of [git push and hooks to deploy the code](/deploy-git-push-php-silex).