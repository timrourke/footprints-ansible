# Provision scripts for footprints

This repository contains the provisioning scripts used for footprints, along
with a Vagrantfile to aid in local development.

## Getting Started

### Vagrant for local development

If you don't already have it, [download Vagrant](https://www.vagrantup.com/downloads.html)
for your preferred platform. Comes with a nice installer, easy to get started.

### VirtualBox for local development

If you don't already have it, [download VirtualBox](https://www.virtualbox.org/wiki/Downloads)
for your preferred platform. Also easy to install.

### RVM

Installing RVM happens via [rvm1-ansible](https://github.com/rvm/rvm1-ansible).

To install this open source playbook, run this command from your host OS:
```bash
ansible-galaxy install rvm_io.ruby
```

### Configure your local OS's hosts file

For mac users, you'll want to add a line to your `/etc/hosts` file pointing the
IP address `192.168.50.4` to the hostname `www.footprints.localdev`.

Not set in stone, so if this creates conflicts for anyone we can change it.

**Windows users:** the relevant file for you is _probably_ `C:\Windows\System32\Drivers\etc\hosts`.

### Vagrant up

Change directories into the folder you cloned this repository into. From there,
run this command:

```bash
vagrant up
```

You should see the Vagrant box download, set itself up, and then run the Ansible
provisioning steps.

If all that shit works, browse to [http://www.footprints.localdev](http://www.footprints.localdev)
and you'll see the app in all of its slightly buggy glory.

## TODO

- **HIGH PRIORITY** I haven't set up a synchronized filesystem yet for local development, but Vagrant supports this very nicely. The idea is that you can mount part of your host filesystem inside of the virtual machine, but at least in the development setup, we should get this set up. Currently, Ansible runs a git command to clone master of our repo, which is obviously going to start sucking when we're actively developing.
- **MEDIUM PRIORITY** We need to figure out how we're going to set environment variables to configure the EC2 instance with things like DB credentials, etc. File-based systems for this are terrible. Vault + Consul is pretty standard for this kind of thing, but that might be overkill. Pretty sure we could use Ansible to automate SSHing in, setting env vars from a file that is _only_ on our local disks (eg. not in GitHub, etc.) and do it that way, but for real CI/CD this stops working fast. I'm open to suggestions.
- Related to the above, I'm not nuts about using the Ansible concept of `group_vars` as a place to store configuration details about the provisioning process. We should be using env vars for this kind of thing to keep them out of source control and off of disk as much as possible. So those should go away.
- RVM is installed for the root user. This is probably not good or necessary. The [RVM site](https://rvm.io) has lots of disclaimers about how this is bad and insecure, but I'm a dummy and couldn't get Ruby installed without installing RVM and Ruby for all users.
- NTP needs port 123 open for 2-way UDP traffic so it can keep time synchronized. Probably will need to open this in the AWS Security Groups configuration.


