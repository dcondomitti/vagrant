# rbenv Installation

## Main Script

rbenv doesn't have a simple installer like RVM but it's still really easy to install it on your machine.

### Check out rbenv into your home directory

    <~> 
    [(06:31 PM) dcondomitti@disrupt] $ git clone git://github.com/sstephenson/rbenv.git .rbenv
    Cloning into '.rbenv'...
    remote: Counting objects: 1040, done.
    remote: Compressing objects: 100% (416/416), done.
    remote: Total 1040 (delta 650), reused 962 (delta 596)
    Receiving objects: 100% (1040/1040), 138.19 KiB, done.
    Resolving deltas: 100% (650/650), done.

### Add it to your path

    <~> 
    [(06:31 PM) dcondomitti@disrupt] $ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile

### Add rbenv init to your shell to enable shims and autocompletion

    <~> 
    [(06:31 PM) dcondomitti@disrupt] $ echo 'eval "$(rbenv init -)"' >> ~/.bash_profile

### Reload your ~/.bash_profile so the path changes take effect

    <~> 
    [(06:32 PM) dcondomitti@disrupt] $ . ~/.bash_profile 

## Ruby Builder

### Install ruby-build

    <~> 
    [(06:32 PM) dcondomitti@disrupt] $ mkdir -p ~/.rbenv/plugins
    <~> 
    [(06:32 PM) dcondomitti@disrupt] $ cd ~/.rbenv/plugins
    <~/.rbenv/plugins> [master]
    [(06:32 PM) dcondomitti@disrupt] $ git clone git://github.com/sstephenson/ruby-build.git
    Cloning into 'ruby-build'...
    remote: Counting objects: 856, done.
    remote: Compressing objects: 100% (384/384), done.
    remote: Total 856 (delta 423), reused 792 (delta 367)
    Receiving objects: 100% (856/856), 89.22 KiB, done.
    Resolving deltas: 100% (423/423), done.

### Rehash to reload rbenv

    <~/.rbenv/plugins> [master]
    [(06:32 PM) dcondomitti@disrupt] $ rbenv rehash

### Install Ruby 1.9.3-p194

    <~/.rbenv/plugins> [master]
    [(06:32 PM) dcondomitti@disrupt] $ rbenv install 1.9.3-p194
    Downloading http://pyyaml.org/download/libyaml/yaml-0.1.4.tar.gz...
    Installing yaml-0.1.4...
    Installed yaml-0.1.4 to /Users/dcondomitti/.rbenv/versions/1.9.3-p194
    Downloading http://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.3-p194.tar.gz...
    Installing ruby-1.9.3-p194...
    Installed ruby-1.9.3-p194 to /Users/dcondomitti/.rbenv/versions/1.9.3-p194
    <~/.rbenv/plugins> [master]
    [(06:37 PM) dcondomitti@disrupt] $ rbenv global 1.9.3-p194 --default
    <~/.rbenv/plugins> [master]

## Bundler

rbenv needs to be reloaded to detect any new binaries once a gem is installed.

    [(06:37 PM) dcondomitti@disrupt] $ gem install bundler --no-rdoc --no-ri
    Fetching: bundler-1.1.3.gem (100%)
    Successfully installed bundler-1.1.3
    1 gem installed
    <~/.rbenv/plugins> [master]
    [(06:37 PM) dcondomitti@disrupt] $ rbenv rehash
