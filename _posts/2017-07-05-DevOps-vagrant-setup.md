---
layout: post
title:  "DevOps: Vagrant setup"
date:   2017-07-05 22:24:07 +0100
categories: devops,development,vagrant
---

# Install MacOS
Vagrant uses [Virtualbox](https://www.virtualbox.org/) to manage the virtual dependencies. You can [directly download virtualbox](https://www.virtualbox.org/wiki/Downloads) and install or use homebrew for it.
```shell
$ brew cask install virtualbox
```

Now install Vagrant either from the [website](http://www.vagrantup.com/downloads.html) or use homebrew for installing it.
```shell
$ brew cask install vagrant
```

[Vagrant-Manager](http://vagrantmanager.com/) helps you manage all your virtual machines in one place directly from the menubar.
```shell
$ brew cask install vagrant-manager
```

# Usage
Add the Vagrant box you want to use. We'll use Ubuntu 12.04 for the following example.
$ vagrant box add precise64 http://files.vagrantup.com/precise64.box
You can find more boxes at [Vagrant Cloud](https://vagrantcloud.com/)

Now create a test directory and cd into the test directory. Then we'll initialize the vagrant machine.
```shell
$ vagrant init precise64
```

Now lets start the machine using the following command.
```shell
$ vagrant up
```

You can ssh into the machine now.
```shell
$ vagrant ssh
```

Other useful commands are ```halt```, ```suspend```, ```destroy``` etc.

# VagrantFile

## Network
```ruby
# map guest 8000 to host 12003
  config.vm.network "forwarded_port", guest: 8000, host: 12003, protocol: "tcp"
  config.vm.network "private_network", ip: "192.168.50.4"
```

## Proxy

### install

```shell
vagrant plugin install vagrant-proxyconf
```

### setup
```ruby
# proxy settings
  config.proxy.http     = "http://webproxy.merck.com:8080"
  config.proxy.https    = "http://webproxy.merck.com:8080"
  config.proxy.no_proxy = "localhost,127.0.0.1"
```

## SSH

### Do not generate a key
```config.ssh.insert_key = false```

### setup keys in the environment
```ruby
config.ssh.private_key_path = ["/Users/ryzhkovm/.ssh/vagrant", "~/.vagrant.d/insecure_private_key"]
config.vm.provision "file", source: "/Users/ryzhkovm/.ssh/vagrant.pub", destination: "~/.ssh/authorized_keys"
```
### add to root user and restart SSH
```ruby
config.vm.provision "shell", inline: <<-EOC
  sudo cp /home/ubuntu/.ssh/authorized_keys /root/.ssh/authorized_keys
  sudo sed -i -e "\\#PasswordAuthentication yes# s#PasswordAuthentication yes#PasswordAuthentication no#g" /etc/ssh/sshd_config
  sudo service ssh restart
EOC
```
## Add-ons

### python if doesn’t exist
```ruby
config.vm.provision "shell", inline: "test -e /usr/bin/python || (sudo apt -y update && sudo apt install -y python-minimal)"
```
