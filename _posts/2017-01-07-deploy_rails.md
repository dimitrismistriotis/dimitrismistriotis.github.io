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
would suggest to have a budget: Say to yourself that after an arbitrary
15$/month threshold, you will roll your own, choosing one of the VMs from
Amazon or Digital Ocean or Google CLoud or Azure, etc.

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

Alternatively I have seen that some authors create a rails application on the
spot which should be all right as long as there is some database connectivity.
One of those tutorials is this from digital ocean: [https://www.digitalocean.com/community/tutorials/how-to-deploy-a-rails-app-with-puma-and-nginx-on-ubuntu-14-04], which was a
little bit confusing since it was difficult to understand which step was related
to deployment and which to application development.

## Assumptions

**Shell**: I usually work with [fish](https://fishshell.com/) but the plain
vanilla is bash, so will slide with bash on the production server for
everything.

## Deployment

### Steps

The steps are the following:

1. Create/Rent/Lease a server
2. Connect to server with SSH (depends on **1**)
3. Install Ruby with [rbenv](https://github.com/rbenv/rbenv) (depends on **2**)
4. Install [NGINX](https://nginx.org/en/) (depends on **2**)
5. Copy the Rails application to the server (depends on **2**)
6. Install [Phusion Passenger](https://www.phusionpassenger.com/) and configure
   it with the current Ruby (depends on **3, 4**)
7. Install and configure [PostgreSQL](https://www.postgresql.org/) (depends on
   **1**)
8. Connect to application from browser (depends on **5, 6**)
9. See login page (depends on **7, 8**)

### 1. Create/Rent/Lease a server

There are many options here that start with leasing a machine somewhere or
starting with a virtual machine at the development PC. The easiest way is if
there is a spare computer anywhere to install Ubuntu or distribution of choice
and experiment on that. I would feel more comfortable to do a deployment once or
twice in a controlled local environment and then to the final production server.
The local environment can be used then for experimentation or decommissioned.
I really enjoyed using vagrant so deployed an docker instance of an Ubuntu VM
which is available here: <https://gist.github.com/dimitrismistriotis/2aebe16bf713c40aaf98cee6fc8d4fa6#file-vagrantfile>.
VirtualBox could also be used but I already develop using docker plus I do not
want any Oracle software running on my computers.

So copy the file to a new directory along with the `webapp.tgz` produced in the
prerequisites, and then a `vagrant up` starts the machine. Remember after each
session to `vagrant halt` to stop it. The current box exposes guest's port 80
(the HTTP port) to host's 4567, so in order to access the application later on
we will navigate to <http://localhost:4567/>.

[Vangrant](https://www.vagrantup.com/) from
[HashiCorp](https://www.hashicorp.com/): *Create and configure lightweight,
reproducible, and portable development environments.* With Vagrant you can:
*Create a single file for your project to describe the type of machine you
want, the software that needs to be installed, and the way you want to access
the machine. Store this file with your project code.*, then: *Run a single
command — "vagrant up" — and sit back as Vagrant puts together your complete
development environment. Say goodbye to the "works on my machine" excuse as
Vagrant creates identical development environments for everyone on your team.*
I installed Vagrant from it's download page:
<https://www.vagrantup.com/downloads.html>

### 2. Connect to server with SSH

For Vagrant it is easy, after going to the directory where vagrant was run, a
`vagrant ssh` is enough.

For remote servers, everybody agrees on creating a pair of SSH keys and then use
these to connect disabling password login for that user and root login
altogether.

While most posts suggest the same commands for generating the private/public
pair for ssh login, after reading  Gert van Dijk's [Upgrade your SSH keys!](https://blog.g3rt.nl/upgrade-your-ssh-keys.html#generate-your-new-sexy-ed25519-key), I would suggest:

`ssh-keygen -o -a 100 -t ed25519`

or follow as much of the advice of
<https://stribika.github.io/2015/01/04/secure-secure-shell.html> as possible
moreover because having a new box without any need to support legacy produced
keys.

**Note**: As the deployment has not been run in a remote server as of writing
this post, I will come back when it's done again with the missing steps. These
are from the top of my head: a. Disable root login on remote host, b. copy
public key to remote host, c. test connection, d. block password login on remote
host, e. create config file on local host.

Congratulations! By "unlocking challenge 2" you can connect to the production
server. The next steps can be executed sequentially, in parallel or in a
different order as described in the "Steps" section above.

## Extras

### Retrieve from a Git repository (Github/Gitlab)

Placeholder
