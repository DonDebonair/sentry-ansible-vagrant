# Sentry with Ansible

This playbook provides a complete, hassle-free way to setup [Sentry](https://github.com/getsentry/sentry) on a VPS/Dedicated Server. You can also optionally install it on a Virtual Machine using Vagrant so you can play around with it. This project is a continuation of [sentry-vagrant](https://github.com/DandyDev/sentry-vagrant), using Ansible, because I wanted to try that out as well. Turns out, I like it better than Puppet, so I will maintain this version, and probably not the Puppet version.

What gets installed:

*  PostgreSQL database
*  NginX webserver/reverse proxy
*  Python, Pip & VirtualEnv
*  The latest stable version of **Sentry** and all of its dependencies

## Let's get rolling!

If you want to install Sentry on a VM using Vagrant, you first need to install [Vagrant](http://www.vagrantup.com/) and a Virtual Machine provider of choice ([VirtualBox](https://www.virtualbox.org/) is free and works out of the box with Vagrant). You also need to [install Ansible](http://docs.ansible.com/intro_installation.html).

You can configure your install by modifying the variables in the _sentry.yml_ file before provisioning.

Then: 

```
$ git clone https://github.com/DandyDev/sentry-ansible-vagrant.git
$ cd /path/to/sentry-ansible-vagrant
$ vagrant up
```

In order to properly access Sentry by its configured hostname (`sentry.server` in the sentry.yml), you have to add this hostname to your hostsfile. On POSIX systems (Linx & OS X), you can add it by doing:

```
$ sudo echo "<servername> 192.168.33.10" >> /etc/hosts
```

## Different OSes

By default, the Vagrant box runs Ubuntu 12.04, but the playbook supports Debian 7 and CentOS 6.4 as well! To try those out, uncomment the appropriate lines in the Vagrantfile and comment out the Debian lines.

## Using the playbook standalone

You can of course also use the playbook without Vagrant. In that case you must provide your own inventory file specifying the host on which to install Sentry. The playbook has been tested on Ubuntu 12.04, Debian 7 and CentOS 6.4. Other flavors of Linux might work as well.

## Secret key

On production environments you will want to set the ``secret_key`` setting under the ``sentry`` namespace to a unique key that acts as a signing token. Generate a secret key for [here](http://www.miniwebtool.com/django-secret-key-generator/)

## Sending mail

There's currently no Mail Transfer Agent being installed, like `postfix`. To enable sending mails anyways, you can use an external mail service that supports sending mail through SMTP. One such service is [Mailgun](http://www.mailgun.com), which allows sending up to 10,000 mails per month for free. To enable mail for sentry, you can edit your Sentry config (`/var/sentry/sentry_conf.py`) and add these variables:

```
EMAIL_HOST = 'YOUR SMTP HOST'
EMAIL_HOST_USER = 'YOUR SMTP USER'
EMAIL_HOST_PASSWORD = 'YOUR SMTP PASSWORD'
EMAIL_PORT = 25 # or 587/465/etc.
EMAIL_USE_TLS = False
SERVER_EMAIL = 'EMAIL ADDRESS MAILS SHOULD ORIGINATE FROM' # eg. sentry@mysentryserver.com
```

## Known issues / TODO

* Sending mails is currently only possible through external services like Mailgun. Possibly install `postfix` for this purpose.
* Unlike the [Puppet version](https://github.com/DandyDev/sentry-vagrant), this version does not require any manual steps. The one major drawback is that it's not completely idempotent. If you run it twice, the creation of the superuser will throw an error because it already exists.
* This hasn't been tested on other Providers than VirtualBox yet
* On CentOS, the firewall is completely closed by default (at least on the box I tried it with). So you have to manually open the relevant port (80) using `iptables`, or if you're not concerned about security (because you're running it locally through Vagrant), you can always flush the firewall with `iptables -F`.

## Contribute

If you have any suggestions, feel free to create an issue here on Github and/or fork this repo, make changes and submit a pull request!
