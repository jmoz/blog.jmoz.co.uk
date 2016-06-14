---
layout: post
title: "We Work Contract - a job board for contractors"
date: 2016-06-14 12:00
comments: true
categories: [python, we work contract]
published: true
---
I have recently launched a side project I've been working on for the past couple of months when not busy - [We Work Contract - a job board for contract web jobs](http://weworkcontract.com).  

There's existing job boards but my time contracting has shown me a lot of developers end up using just email and LinkedIn as the existing job boards are not focused enough on contracts.  A lot of permanent developers do not even know where to start looking for contracts or how to get into the sector.  My aim with this site is for it to be a clean, simple destination for job seekers and a source of the niche contract developer market for hiring companies.

Right now I am indexing good sources of contract jobs.  I am filtering out bad jobs so no job adverts with missing day rates as no contractor wants to be sent around in circles.  I am working on the backend mainly and will be improving the frontend end soon.  I am working to allow the posting of job adverts by hiring companies once traffic starts coming through.

There is a twitter account you can follow to keep up to date with the jobs as they come in [@weworkcontract](http://twitter.com/weworkcontract).

## Developers

For the curious, here's how it's made.

First is the scraping system which is built using Scrapy.  Spiders are fired up to fetch jobs from job source websites.  The jobs are then passed through a pipeline which does filtering to drop any bad data or inappropriate records.

The second component is a Celery/Redis task system.  I use this to asynchronously process the jobs - fetching further details and tweeting the job.

The main web site is then built using Django and Postgres.  Nothing to see here move along.

I have used Vagrant and Ansible for the development environment.  The entire stack can be created and data scraped and imported with a simple **vagrant up**.  I use an Ansible playbook with an extra role (just git stuff and symlinking) to deploy on AWS.
