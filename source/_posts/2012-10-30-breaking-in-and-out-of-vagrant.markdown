---
layout: post
title: "Breaking in and out of Vagrant"
date: 2012-10-30 00:32
comments: true
categories: 
author: [Max Veytsman](https://twitter.com/mveytsman)
meta: true
---

[Vagrant](http://vagrantup.com/) is a great tool that allows you to
easily spawn and configure lightweight VMs to use as development
environments. Vagrant provides base installs of several flavours of
Linux, and takes care of setting up networking and shared folders for
you.  

Vagrant is really useful for managing your development environment,
and I highly recommend it. If you're doing a lot of development,
you might need to be running all kinds of application and database
servers on your machine, and it's probably a much better idea to run
them in a VM rather than on your host machine.  

Unfortunately, if you expose your Vagrant VMs (or any development VMs
really) to the outside world, what seemed like a secure best practice
can end up being very insecure. This isn't a novel concept, but the
disposable nature of virtual machines makes it easy to forget to think
about their security. A common attitude is "What's the worst that can
happen if someone gets on my development VM? I blow away and
re-generate this VM every half hour anyway, and aren't VM escape bugs
rare and esoteric anyway?"  

I'm going to show a really simple way to break out of Vagrant VMs, but first,

How Vagrant is Used 
------------------- 

Vagrant VMs are designed to be lightweight and built per-project. The
VMs around a concept of "base boxes," which are base installs of
various flavours of linux. You initialize a Vagrant VM from a base
box, and then can install your desired environment.

By default, Vagrant shares your project folder 
with the VM at `/vagrant/`. So, a usual workflow would be:

``` bash
host: $ cd /my/project/here/
host: $ #we're going to base our VM on an Ubuntu Lucid 32bit base box 
host: $ vagrant box add lucid32 http://files.vagrantup.com/lucid32.box
host: $ vagrant init lucid32 
host: $ vagrant up
host: $ vagrant ssh
  vm: $ #now we are on the VM
  vm: $ cd /vagrant/
  vm: $ run_my_server
```
You can then edit the code on your host machine, and use the VM to run an applciation and database server.

Bridging the Network
--------------------

Just like in any virtual machine, you can configure your Vagrant VMs'
networking to run in host-only mode or in bridged mode. In host-only
mode, a VM uses a virtual network adapter and is basically only
accessible from the host. In bridged mode, the VM bridges through the
network adapter of the host machine, and actually connects to the same
network as the host. So, for example, if you bridge a VM to a wifi
device, the VM will have a different IP address on the same wifi
network as the host. And of course, you'll be able to access the VM
from other machines on the wifi network. In host-only mode, the VM is
not accessible from outside the host. As they say, NAT is the poor
man's firewall.

The Vagrant
[documentation](http://vagrantup.com/v1/docs/bridged_networking.html)
doesn't mention any security risks of running development VMs in
bridged mode. There's a promising alert box, but the contents are:

> #### Not All Networks Work!
>Some networks will not work properly with bridged networking. Specifically, I've found that hotel networks, airport networks, and generally public-shared networks have configurations in place such that bridging does not work.
>You can tell if the bridged networking worked successfully by seeing if the virtual machine was able to get an IP address on the bridged adapter.

The author doesn't seem to see any intrinsic problem with running Vagrant VMs in hotels, airports, and coffee-shops outside of it sometimes not working.

Breaking In
-----------

So after scanning the airport wifi, you found someone running a
Vagrant VM in bridged mode. How do you get in? First of all, they are
most like likely doing some kind of development on it, so the VM is
probably running an app that's not very secured. There's also a good
chance that this VM is running a database and maybe CouchDB or Redis.
One of these is probably using guessable credentials (this is the
development environment after all), or the app itself might have an
exploitable bug.

All of the above sounds too hard. Remember the `vagrant ssh` command
above? Well, it has to get in somehow.  

On all of the VMs built from official base boxes, like `lucid32` the
credentials are `vagrant : vagrant`. The instructions for developers
building new base boxes actually say not to use password-based SSH
logins. Instead, anyone building a base box for public consumption is
using these [SSH keys](https://github.com/mitchellh/vagrant/tree/master/keys/) for the
`vagrant` user.

Either way, any Vagrant VM built from a public base box (read: almost
all of them) is going to be accessible with either `vagrant : vagrant`
or the SSH keys above.

Breaking Out 
------------ 

Breaking into a development VM isn't a big deal on it's own,
especially since the vagrant workflow encourages blowing-away and
rebuilding the VM often. What you really want is code execution on the
host, and it's going to be surprisingly easy to do that.

The Vagrant workflow encourages you to edit your code outside the VM.
That's why Vagrant helpfully shares the project directory as
`/vagrant/` in the VM. It's a safe assumption that anyone using
Vagrant for development is using some kind of version control, and if
that's the case, they are probably going to be interacting with it on
the host machine (where they edit the code). This is how we'll get
execution on the host.

For simplicity, I'm going to focus on Git, but this trick should work
for any other commonly used VCS. Hooks are little shell scripts that
run after you commit a certain action. For instance, a `post-commit`
hook is a shell script that git will execute every time a commit is
completed. Hooks are places in `.git/hooks/HOOK-NAME`.

So if I get on a Vagrant VM and want to escape into the host, all I
have to do is create a post-commit hook. I simply put evil things in
`/vagrant/.git/hooks/post-commit` and wait for the user to commit some
code. Since the `/vagrant/` directory is mounted from the host, my
hook will persist even if the user destroys the VM.

If you liked this, you should follow me on [twitter](https://twitter.com/mveytsman).