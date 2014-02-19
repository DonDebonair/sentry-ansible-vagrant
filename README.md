# Sentry with Ansible

This playbook provides a complete, hassle-free way to setup [Sentry](https://github.com/getsentry/sentry) on a VPS/Dedicated Server. You can also optionally install it on a Virtual Machine using Vagrant so you can play around with it. This project is a continuation of [sentry-vagrant](https://github.com/DandyDev/sentry-vagrant), using Ansible, because I wanted to try that out as well. Turns out, I like it better than Puppet, so I will maintain this version, and probably not the Puppet version.

What gets installed:

*  PostgreSQL database
*  NginX webserver/reverse proxy
*  Python, Pip & VirtualEnv
*  The latest stable version of **Sentry** and all of its dependencies

## Let's get rolling!

If you want to install Sentry on a VM using Vagrant, you first need to install [Vagrant](http://www.vagrantup.com/) and a Virtual Machine provider of choice ([VirtualBox](https://www.virtualbox.org/) is free and works out of the box with Vagrant).

You can configure your install by modifying the variables in the _sentry.yml_ file before provisioning.

Then: 

```
$ git clone https://github.com/DandyDev/sentry-ansible-vagrant.git
$ cd /path/to/sentry-ansible-vagrant
$ vagrant up
```

## Different OSes

By default, the Vagrant box runs Debian 7, but the playbook supports Ubuntu 12.04 and CentOS 6.4 as well! To try those out, uncomment the appropriate lines in the Vagrantfile and comment out the Debian lines.

## Using the playbook standalone

You can of course also use the playbook without Vagrant. In that case you must provide your own inventory file specifying the host on which to install Sentry.

## Known issues / TODO

* Mailer doesn't work yet
* Unlike the [Puppet version](https://github.com/DandyDev/sentry-vagrant), this version does not require any manual steps. The one major drawback is that it's not completely idempotent. If you run it twice, the creation of the superuser will throw an error because it already exists.
* This hasn't been tested on other Providers than VirtualBox yet
* On CentOS, the firewall is completely closed by default (at least on the box I tried it with). So you have to manually open the relevant port (80) using `iptables`, or if you're not concerned about security (because you're running it locally through Vagrant), you can always flush the firewall with `iptables -F`.