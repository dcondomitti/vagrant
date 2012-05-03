# RVM Installation

## Main Script

RVM has an installer script that can be curl'ed into bash:

    $ curl -L get.rvm.io | bash -s stable

Just run that script and it will automatically install RVM.

    <~> [master]
    [(06:10 PM) dcondomitti@disrupt] $ curl -L get.rvm.io | bash -s stable
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100  8844  100  8844    0     0   8297      0  0:00:01  0:00:01 --:--:-- 78265
    Downloading RVM from wayneeseguin branch stable
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100  994k  100  994k    0     0   334k      0  0:00:02  0:00:02 --:--:--  491k

    Installing RVM to /Users/dcondomitti/.rvm/
        RVM PATH line found in /Users/dcondomitti/.bashrc /Users/dcondomitti/.bash_profile /Users/dcondomitti/.zshrc.
        RVM sourcing line found in /Users/dcondomitti/.bash_profile /Users/dcondomitti/.bash_login /Users/dcondomitti/.zlogin.

    # RVM:  Shell scripts enabling management of multiple ruby environments.
    # RTFM: https://rvm.io/
    # HELP: http://webchat.freenode.net/?channels=rvm (#rvm on irc.freenode.net)
    # Cheatsheet: http://cheat.errtheblog.com/s/rvm/
    # Screencast: http://screencasts.org/episodes/how-to-use-rvm

    # In case of any issues read output of 'rvm requirements' and/or 'rvm notes'

    Installation of RVM in /Users/dcondomitti/.rvm/ is almost complete:

      * To start using RVM you need to run `source /Users/dcondomitti/.rvm/scripts/rvm`
        in all your open shell windows, in rare cases you need to reopen all shell windows.

    # Daniel Condomitti,
    #
    #   Thank you for using RVM!
    #   I sincerely hope that RVM helps to make your life easier and more enjoyable!!!
    #
    # ~Wayne


    rvm 1.13.0 (stable) by Wayne E. Seguin <wayneeseguin@gmail.com>, Michal Papis <mpapis@gmail.com> [https://rvm.io/]

If you have any terminal windows open you'll need to run `source /Users/dcondomitti/.rvm/scripts/rvm` or just reopen them to use RVM.

## Dependency Packages

Now that RVM is installed, you can install the `zlib`, `openssl` and `readline` packages and build Ruby 1.9.3:

    [(06:12 PM) dcondomitti@disrupt] $ rvm pkg install zlib
    Fetching zlib-1.2.6.tar.gz to /Users/dcondomitti/.rvm/archives
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100  544k  100  544k    0     0   232k      0  0:00:02  0:00:02 --:--:--  308k
    Extracting zlib-1.2.6.tar.gz to /Users/dcondomitti/.rvm/src
    Configuring zlib in /Users/dcondomitti/.rvm/src/zlib-1.2.6.
    Compiling zlib in /Users/dcondomitti/.rvm/src/zlib-1.2.6.
    Installing zlib to /Users/dcondomitti/.rvm/usr

    <~/Sites/paperlesspost/vagrant> [master]
    [(06:12 PM) dcondomitti@disrupt] $ rvm pkg install openssl
    Fetching openssl-0.9.8t.tar.gz to /Users/dcondomitti/.rvm/archives
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100 3690k  100 3690k    0     0   133k      0  0:00:27  0:00:27 --:--:--  169k
    Extracting openssl-0.9.8t.tar.gz to /Users/dcondomitti/.rvm/src
    Configuring openssl in /Users/dcondomitti/.rvm/src/openssl-0.9.8t.
    Compiling openssl in /Users/dcondomitti/.rvm/src/openssl-0.9.8t.
    Installing openssl to /Users/dcondomitti/.rvm/usr
    <~/Sites/paperlesspost/vagrant> [master]

    [(06:14 PM) dcondomitti@disrupt] $ rvm pkg install readline
    Fetching readline-5.2.tar.gz to /Users/dcondomitti/.rvm/archives
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100 1989k  100 1989k    0     0   448k      0  0:00:04  0:00:04 --:--:--  471k
    Extracting readline-5.2.tar.gz to /Users/dcondomitti/.rvm/src
    Applying patch '/Users/dcondomitti/.rvm/patches/readline-5.2/shobj-conf.patch'...
    Configuring readline in /Users/dcondomitti/.rvm/src/readline-5.2.
    Compiling readline in /Users/dcondomitti/.rvm/src/readline-5.2.
    Installing readline to /Users/dcondomitti/.rvm/usr
    Fetching readline-6.2.tar.gz to /Users/dcondomitti/.rvm/archives
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100 2224k  100 2224k    0     0   452k      0  0:00:04  0:00:04 --:--:--  515k
    Extracting readline-6.2.tar.gz to /Users/dcondomitti/.rvm/src
    Applying patch '/Users/dcondomitti/.rvm/patches/readline-6.2/patch-shobj-conf.diff'...
    Configuring readline in /Users/dcondomitti/.rvm/src/readline-6.2.
    Compiling readline in /Users/dcondomitti/.rvm/src/readline-6.2.
    Installing readline to /Users/dcondomitti/.rvm/usr

## Ruby

    <~/Sites/paperlesspost/vagrant> [master]
    [(06:15 PM) dcondomitti@disrupt] $ rvm install 1.9.3
    Fetching yaml-0.1.4.tar.gz to /Users/dcondomitti/.rvm/archives
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100  460k  100  460k    0     0   332k      0  0:00:01  0:00:01 --:--:--  497k
    Extracting yaml-0.1.4.tar.gz to /Users/dcondomitti/.rvm/src
    Configuring yaml in /Users/dcondomitti/.rvm/src/yaml-0.1.4.
    Compiling yaml in /Users/dcondomitti/.rvm/src/yaml-0.1.4.
    Installing yaml to /Users/dcondomitti/.rvm/usr
    Installing Ruby from source to: /Users/dcondomitti/.rvm/rubies/ruby-1.9.3-p194, this may take a while depending on your cpu(s)...

    ruby-1.9.3-p194 - #fetching 
    ruby-1.9.3-p194 - #downloading ruby-1.9.3-p194, this may take a while depending on your connection...
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100 9610k  100 9610k    0     0   336k      0  0:00:28  0:00:28 --:--:--  369k
    ruby-1.9.3-p194 - #extracting ruby-1.9.3-p194 to /Users/dcondomitti/.rvm/src/ruby-1.9.3-p194
    ruby-1.9.3-p194 - #extracted to /Users/dcondomitti/.rvm/src/ruby-1.9.3-p194
    ruby-1.9.3-p194 - #configuring 
    ruby-1.9.3-p194 - #compiling 
    ruby-1.9.3-p194 - #installing 
    Retrieving rubygems-1.8.24
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100  371k  100  371k    0     0   688k      0 --:--:-- --:--:-- --:--:-- 1075k
    Extracting rubygems-1.8.24 ...
    Removing old Rubygems files...
    Installing rubygems-1.8.24 for ruby-1.9.3-p194 ...
    Installation of rubygems completed successfully.
    ruby-1.9.3-p194 - adjusting #shebangs for (gem irb erb ri rdoc testrb rake).
    ruby-1.9.3-p194 - #importing default gemsets (/Users/dcondomitti/.rvm/gemsets/)
    Install of ruby-1.9.3-p194 - #complete 

## Ruby Selection

    <~> [master]
    [(06:25 PM) dcondomitti@disrupt] $ rvm use 1.9.3-p194 --default
    Using /Users/dcondomitti/.rvm/gems/ruby-1.9.3-p194
       