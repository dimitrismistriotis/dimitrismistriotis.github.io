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
to read, decided to roll my own tutorial/post.

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

### Most influential post for writing this

Sophie De Benedetto's [Deploying Rails to DigitalOcean, the Hard Way](http://www.thegreatcodeadventure.com/deploying-rails-to-digitalocean-the-hard-way/)

There are many posts lying around but following this one I managed to complete
the installation. See References section for more.

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

**Application Server**: Why passenger.

## Deployment

### Steps

The steps are the following:

1. Create/Rent/Lease a server
2. Connect to server with SSH (depends on **1**)
3. Install Ruby with [rbenv](https://github.com/rbenv/rbenv) and
   [ruby-build](https://github.com/rbenv/ruby-build) (depends on **2**)
4. Install Rails and NodeJS (depends on **3**)
5. Install [NGINX](https://nginx.org/en/) (depends on **2**)
6. Copy the Rails application to the server (depends on **2**)
7. Install [Phusion Passenger](https://www.phusionpassenger.com/) and configure
   it with the current Ruby and your application (depends on **4, 5, 6**)
8. Install and configure [PostgreSQL](https://www.postgresql.org/) (depends on
   **1**)
9. Connect to application from browser (depends on **6, 7**)
10. See login page (depends on **8, 9**)

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

Before next step: `sudo apt-get update` and `sudo apt-get upgrade`.

### 3. Install Ruby with rbenv

There is a nice post here:
<http://kgrz.io/Programmers-guide-to-choosing-ruby-version-manager.html> on
choosing a version manager for Ruby. I decided on this combination based on
popularity plus I saw an easier-to understand integration with Passenger on the
tutorials that this post <del>has stolen from</del> is based on.

Generally reflecting on writing this, the decision was to have a as boring
server as humanly possible, hence easy to debug. So opting for the most
popular choices is at least desired. For not choosing RVM, changing how cd works
is something that this geek's heart cannot endure, provided with an alternative.

From: <https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rbenv-on-ubuntu-14-04>:

```
sudo apt-get install git-core curl zlib1g-dev build-essential \
  libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 \
  libxml2-dev libxslt1-dev libcurl4-openssl-dev \
  python-software-properties libffi-dev
```

All these are unfortunately needed for rbenv. Unfortunately because more
packages progressively bloat the system with probable security and maintenance
implications.

For rbenv:

```
cd ~
git clone git://github.com/sstephenson/rbenv.git .rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
source ~/.bash_profile
```

(From Digital Ocean's tutorial and rbenv's installation instructions with some
modifications)

Then:

```
cd ~
git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bash_profile
source ~/.bash_profile
```

And time to get our preferred version of Ruby:

```
rbenv install -v 2.3.1
rbenv global 2.3.1
```

A `ruby -v` should return something in the lines of:
"ruby 2.3.1p112 (2016-04-26 revision 54768) [x86_64-linux]"

### 4. Install Rails and NodeJS

<img src="/images/deploy_rails/rails-logo.svg" style="width: 30%"><br>

A nice idea that usually gets forgotten is to run at this point:
`echo "gem: --no-document" > ~/.gemrc`. Documentation is not that much needed
in a production server and removing it out will speed up the gem
installation/update process.

For Rails installation:

```
gem install bundler && rbenv rehash
gem install rails && rbenv rehash
```


The "rehash" command of brenv, "*Installs shims for all Ruby executables known
to rbenv (i.e., ~/.rbenv/versions//bin/). Run this command after you install a
new version of Ruby, or install a gem that provides commands ...*" according to
tool's documentation. In order to be sure that this happens all the time, the
command should be appended to a future deployment script.

![Rehash... all the things](/images/deploy_rails/rehash_all_the_things.jpg)

In any case once everything is over, check with `rails -v` to see that Rails has
been properly installed.

We can verify that everything is all right by creating a new Rails application
and running it:

```
cd ~
rails new testit
cd testit
rails s
```

Which brings up an error: "*There was an error while trying to load the gem
'uglifier'. (Bundler::GemRequireError) Gem Load Error is: Could not find a
JavaScript runtime. See https://github.com/rails/execjs for a list of available
runtimes.*" This can be fixed by installing the missing piece of this step,
NodeJS: `sudo apt-get install nodejs`. I am not 100% sure but even if Ubuntu or
Debian have a more legacy version of Node, it should be OK for Rails. There are
also different options for a JavaScript runtime but chose not to explore them.

Do not know if it would be better to be able to do this processing in a
different machine to the one we want to deploy, keeping the production machine
with minimal packages installed. In any case this is the way things currently
are...

### 5. Install NGINX

![NGINX logo](/images/deploy_rails/nginx.png)

Installing Nginx with defaults should be easy

```sudo apt-get install nginx```

then

```sudo service nginx start```

For some reason (probably something to do with how docker comprehends the
world), running daemons (in our case nginx and postgresql) did not persist
between runs of the machine or reboots. That's why I used this little script
named "`start_services`" after every `vagrant ssh`:

```
#!/bin/sh
sudo service postgresql start
sudo service nginx start
```

Available here: <https://gist.github.com/dimitrismistriotis/2aebe16bf713c40aaf98cee6fc8d4fa6#file-start_services>. Do not
forget to make it executable (`chmod +x start_services`).

### 6. Copy the Rails application to the server

In the case of using Vagrant the tgz should be first copied to the shared
directory of the host machine extracted from the the current user inside the
container: `tar -xvzf /vagrant_data/webapp.tgz`. Target application is in the
"web" directory, which from now on will be: "/home/vagrant/web".

**Note**: When on an actual machine it will be copied there before extraction
with a secure copy, while probably most will do a git clone which as discussed
before is out of this post's main body.

Then `bundle`. It will complain about the pg gem for Postgresql connectivity.
This is fixed by: `sudo apt-get install libpq-dev` and then `bundle` again. As
always followed by an `rvn rehash`

We can see that there is some life by running a console (`rails c`), or even a
production console(`RAILS_ENV=production rails c`). Just do not try to use the
database, because nothing is there yet or it has not been configured. Trigger
an error if curious by: `User.all`.

I also had not configured Devise's secret key which raised an error as well.
Remember to fix the application first if that is the case and then copy it
again (This is where using git would be handy).

## Extras

### Retrieve from a Git repository (Github/Gitlab)

Placeholder

## References

[Michele Anica](https://www.digitalocean.com/community/users/manicas)'s
[How To Install Ruby on Rails with rbenv on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rbenv-on-ubuntu-14-04)

