# Paperless Post Development Environment

* [Summary](#summary)
* [Prerequisites](#prerequisites)
* [Installation](#installation)
* [Problems](#problems)

## Summary
All the separate [Paperless](https://github.com/paperlesspost) applications have been moved into a virtualized Linux environment in an effort to speed up onboarding of new developers (or configuring your next Mac) and to ensure that development is as close to production as possible. This repository contains everything you need to set it up on your local machine.

## Prerequisites
* Virtualbox 4.1.10 or higher (automatic installation coming soon)
* AWS access keys for `paperless-interval.s3.amazonaws.com` (pulled through Pushparty or another application for brokering keys eventually)
* Mac OS X 10.6 or greater (Snow Leopard or Lion)
* Access to our Github repositories
* Ruby 1.9.3 (**[rbenv](https://github.com/sstephenson/rbenv)** and **[RVM](https://github.com/wayneeseguin/rvm)** can help with this)
* XCode 4.3
* Bundler

## Installation

### Github Access

I'm assuming if you're reading this then you already have access to our repositories but double-check anyway as we use SSH agent forwarding to pull the repositories in using your own Github account. The repositories you need access to are:

* [paperless-post](https://github.com/paperlesspost/paperless-post)
* [paperless-dispatcher](https://github.com/paperlesspost/paperless-dispatcher)
* [paperless-mobile](https://github.com/paperlesspost/paperless-mobile)
* [cardapi-as3](https://github.com/paperlesspost/cardapi-as3)
* [vagrant](https://github.com/paperlesspost/vagrant)

If you're missing access to any of these talk to your project manager or **[@mrb](https://github.com/mrb)** ([mrb@paperlesspost.com](mailto:mrb@paperlesspost.com))

You need an SSH keypair on your local machine with the corresponding public key on Github. `ssh-keygen` will generate the key and piping the public key (`~/.ssh/id_rsa.pub` normally) to `pbcopy` will put it on your clipboard.

    <~> 
    [(05:34 PM) dcondomitti@disrupt] $ ssh-keygen
    Generating public/private rsa key pair.
    Enter file in which to save the key (/Users/dcondomitti/.ssh/id_rsa):
    Enter passphrase (empty for no passphrase): 
    Enter same passphrase again: 
    Your identification has been saved in /Users/dcondomitti/.ssh/id_rsa.
    Your public key has been saved in /Users/dcondomitti/.ssh/id_rsa.
    The key fingerprint is:
    a6:6c:26:6d:27:a7:97:af:be:96:e7:1c:5f:a3:9b:4d dcondomitti@disrupt
    The key's randomart image is:
    +--[ RSA 2048]----+
    |                 |
    |                 |
    |                 |
    |                 |
    |        S        |
    |     o o         |
    |    . O oo.   E  |
    |     = ==..o * . |
    |      .++*+ =..  |
    +-----------------+
    <~> 
    [(05:34 PM) dcondomitti@disrupt] $ cat ~/.ssh/id_rsa.pub | pbcopy

Go to [Github's SSH settings](https://github.com/settings/ssh) and paste the public key:

![](https://paperless-vagrant.s3.amazonaws.com/sshkeys.jpg)

Test it by sshing to `git@github.com`:

    [(05:39 PM) dcondomitti@disrupt] $ ssh -T git@github.com
    Hi dcondomitti! You've successfully authenticated, but GitHub does not provide shell access.

### AWS Access Keys

Hopefully we'll develop an automated way to obtain these keys in the future but for right now you'll need to talk to someone on the **[ops](ops@paperlesspost.com)** team. You'll receive an AWS access key and secret access key that will be used for signing requests to AWS/S3 and downloading the Chef validator key and vagrant base image. Place them in your home directory (I use `~/.credentials/aws.keys`) and specify the path to it using `$AWS_CREDENTIAL_FILE` in `~/.bash_profile`: 

    <~/Sites/paperlesspost/vagrant> [master]
    [(04:32 PM) dcondomitti@disrupt] $ cat ~/.bash_profile 
    export AWS_CREDENTIAL_FILE=~/.credentials/aws.keys

    <~/Sites/paperlesspost/vagrant> [master]
    [(04:33 PM) dcondomitti@disrupt] $ cat ~/.credentials/aws.keys
    AWSAccessKeyId=A0A0A0A0A00A0A0A00A0
    AWSSecretKey=a0a0a0a0a0a00a0A000a0a00a0000A00A0A0A0A0

Once you've added them, reload your `.bash_profile` by typing `. ~/.bash_profile` or by relaunching your terminal. If you're going to be working directly with S3, EC2 or any other AWS service through the CLI tools you'll need a certificate and corresponding private key file. Again, talk to **[ops](ops@paperlesspost.com)** to obtain these or an SSH keypair for launching and bootstrapping instances.

### XCode 4.3

As of XCode 4.1 you need to download it from the Mac App Store. This means you need to have at least Snow Leopard (Mac OS X 10.6) installed on your mac. Download [XCode 4.3](http://itunes.apple.com/us/app/xcode/id497799835?mt=12) and install it; you don't even need an Apple Developer account anymore. Once you have it installed, start it up and install the command line tools by opening XCode preferences and clicking **Install** next to `Command Line Tools` on the `Downloads` tab.

![](https://paperless-vagrant.s3.amazonaws.com/xcodecli.jpg)

### Ruby 1.9.3

It really doesn't matter what you use to install Ruby it's only used to control Vagrant; all actual applications run within our self-packaged system rubies within the VMs. **[rbenv](https://github.com/sstephenson/rbenv)** and **[RVM](https://github.com/wayneeseguin/rvm)** can be used to install Ruby. If you run into any issues there are users of both in the office. View our installation documentation for [rbenv](https://github.com/dcondomitti/vagrant/tree/master/documentation/rbenv.md) or [RVM](https://github.com/dcondomitti/vagrant/tree/master/documentation/rvm.md) for more info.

### Bundler

This one's easy:

    <~/Sites/paperlesspost/vagrant> [master]
    [(05:23 PM) dcondomitti@disrupt] $ gem install bundler --no-rdoc --no-ri
    Successfully installed bundler-1.1.3
    1 gem installed

### Virtualbox

Download the Virtualbox [.dmg](http://download.virtualbox.org/virtualbox/4.1.14/VirtualBox-4.1.14-77440-OSX.dmg) from [virtualbox.org](https://www.virtualbox.org) and install it. You'll need to provide your administrator credentials during the installation process but other than that it needs no configuration.

### Gem Dependencies

This project has very few dependencies as they're all contained within the individual project Gemfiles.

    <~/Sites/paperlesspost/vagrant> [master]
    [(05:26 PM) dcondomitti@disrupt] $ bundle install --binstubs
    Using archive-tar-minitar (0.5.2) 
    Using builder (3.0.0) 
    Using ffi (1.0.11) 
    Using childprocess (0.3.2) 
    Using erubis (2.7.0) 
    Using excon (0.13.4) 
    Using formatador (0.2.1) 
    Using mime-types (1.18) 
    Using multi_json (1.3.4) 
    Using net-ssh (2.2.2) 
    Using net-scp (1.0.4) 
    Using nokogiri (1.5.2) 
    Using ruby-hmac (0.4.0) 
    Using fog (1.3.1) 
    Using multi_xml (0.4.4) 
    Using httparty (0.8.3) 
    Using i18n (0.6.0) 
    Using json (1.5.4) 
    Using log4r (1.1.10) 
    Using vagrant (1.0.3) 
    Using bundler (1.1.3) 
    Your bundle is complete! Use `bundle show [gemname]` to see where a bundled gem is installed.

### Fire this 'ish up

Simply type `bin/vagrant up`, provide your password and your development environment should be ready by the time you're done grinding a few cups of pourover coffee for your coworkers (you owe it to them, do it.) The Vagrant gem and chef recipes will handle downloading the base CentOS 6.2 image, importing it into Virtualbox, creating the necessary NFS mounts and port forwards and most importantly installing the application and its dependencies.

    <~/Sites/paperlesspost/vagrant> [master]
    [(05:27 PM) dcondomitti@disrupt] $ bin/vagrant up
    [default] Importing base box 'paperless-final'...
    [default] Matching MAC address for NAT networking...
    [default] Booting VM...
    ...


## Problems

This is all brand new to us so you're likely to run into problems. Ask any of the **ops** staff in campfire, open a [Github Issue](https://github.com/dcondomitti/vagrant/issues) on this project with any problems or email/gtalk **[@dcondomitti](https://github.com/dcondomitti)** ([dan@paperlesspost.com](mailto:dan@paperlesspost.com)).
