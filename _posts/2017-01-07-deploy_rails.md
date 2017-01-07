---
layout: default
title: Deploy Rails (production) on your own server
permalink: /deploy_rails
---

# Deploy Rails (production) on your own server

## Intro

I wanted to deploy a small rails application in my own server and I found the
material available a little bit puzzling. After spending lots of time with
different tutorials and being an advocate of writing things that I would like
to read, decided to roll my own tutorial.

The tutorial ends once we can see the rails application deployed on a plain
vanilla Ubuntu server, assuming that the process is similar to Debian boxes.
Also this document does not include ongoing update of the application, so will
not fiddle with how to retrieve and restart the application from github or any
deployment framework such as Dokku or Capistrano. When this bridge is crossed,
it will be part of an "extra material" section.

### A note on other tutorials

Most tutorials I have seen follow a linear path, while some of the steps can be
executed in parallel. Also many say what to do but do not explain that much
why you do it. This makes sense as someone could fall into the rabbit hole of
explaining how a webserver works for hours and hours - pages and pages in our
case. I will try to provide a short paragraph or at least one link in each of
these cases.

## The stack

Rails version 5.0 with PostgreSQL.

### Why deploy in the first place

Or why deploy in a world where Heroku exists? Did some digging and self
reflection before deciding to do a roll-your-own deployment. The answer is the
usual in IT circles "it depends" mixed with "what you want". I think Heroku is
a great option not only because you can deploy in minutes, but also because it
eliminates the need for a systems administrator in many cases. There will be the
case where Heroku is down and nobody can do anything which is as probable as
your provider of choice going down *without* including errors on your behalf.

Heroku has two issues: costs scale exponentially ("their" fault), and there is
no viable industry alternative (not "their" fault). For small/hobby projects I
would suggest to have a budget: after say 15$/month, I'd roll my own which in
case of Amazon or Digital Ocean or Google CLoud or Azure, etc.

For my project I wanted to run some custom services (not covered here), so
sooner or later there would be the need to have a Linux box somewhere. I would
also consolidate some other stuff in that machine. Also I would like that extra
control.

## Prerequisites

A running application in a development environment. Because the connection to
the database needs to be checked, make sure that there are some data there.
Maybe create a model and some seed data, or integrate Devise. This for the case
that there is not an existing application already.

Because there is no git deployment, create a copy with the following command,
assuming that the application resides in a folder named `web`:

`tar --exclude=".git" --exclude="tmp" -cvzf webapp.tgz  web/*`

## Deployment