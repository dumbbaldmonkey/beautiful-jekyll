---
layout: post
title: Setting Up Ruby on Rails on Ubuntu 17.10
image: /img/hello_world.jpeg
---

# Setting up Rbenv
Rbenv is my choice for a Ruby version manager.  It is easy to use and allows you to have multiple versions of Ruby on 
your computer without much fuss.

Step one is to install a bunch of dependencies for rbenv as well as for Ruby and other languages in general on Ubuntu
~~~ shell
$ sudo apt-get install autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm3 libgdbm-dev
~~~

If you want the full instructions on how to install rbenv go checkout the [rbenv github][rbenv].
If you just want to cut to the case run the following commands 
~~~ shell
$ git clone https://github.com/rbenv/rbenv.git ~/.rbenv
$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
~~~
Next, if you want to easily install different Ruby versions and not just install them, then we need to get
 [ruby-build].
~~~ shell
$ mkdir -p "$(rbenv root)"/plugins
$ git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
~~~

Now you have the secret sauce to install and run set any ruby you want!
~~~
$ rbenv install 2.5.0
$ rbenv global 2.5.0
~~~

At this point you have to re-login to your environment for it to pickup the changes to your path.  That is, the shims that rbenv put 
in place won't exist so you won't be able to use `ruby -v` in your terminal. 
So just log out then log back in and poof, magic!


Or if logging in and out it too tedious for you, just run 
~~~ shell 
. ~/.profile
~~~ 
This is just a way to tell bash to load from that file. `.` is an alias for the `source` method in bash if you're 
curious.

After reloading your session `ruby -v` should now output the rbenv version you just installed 
and `gem environment` should list the directory of rbenv that you just setup.
~~~ shell
$ ruby -v
$ ruby 2.5.0p0 (2017-12-25 revision 61468) [x86_64-linux]
~~~
While we're thinking of it let's install bundler because we're sure going to use it in the future.
~~~ shell
$ gem install bundler
~~~

# Installing PG
Right now if you try to install the [pg] gem then you're not going to have a very good time.
This means if you're going to be running with Postgres then you'll need to install postgres as well as its development 
dependencies. 
~~~ shell
$ sudo apt-get install postgresql libpq-devgem
~~~ 

You should now be able to run `gem install pg` just fine, but there are a few steps left before you can fully leverage
 Postgres locally.
 
At the very least you'll need to create a role for a user, I just use my local account name since that is what Rails 
defaults to if none is provided in the `database.yml`, but you don't have to listen to me.

Execute the following and type in your account name and make that user a superuser (assuming you want to 
do stuff with your freshly installed database)
~~~ shell
$ sudo -u postgres createuser --interactive
~~~


# Installing RubyMine (Optional) 
I personally like to use [Rubymine] for my Rails development.
Its pretty straight forward to install, but I'll show how I install it just to be fully comprehensive.

Go download [Rubymine] the latest and greatest version. I installed version 2017.3.3 so there may be some discrepancies.
Take the **Rubymine.x.y.z.tar.gz** and extract it to where you like.
I have mine installed to the `/opt/Rubymine` directory.
In your Rubymine install directory run to startup the application. 
~~~ shell
$ ./bin/rubymine.sh
~~~
When its running start choosing the defaults to your liking. 
Once you're done configuring your install the last thing that is nice to do is to create a desktop entry for this 
application so you don't have to manually call the script every time you want to code.  

To do this, use the dropdown in the bottom right of the app to choose "Create Desktop Entry".  
If you don't do this now you can always do it later by finding the option, if you chose the default keymap, by hitting 
`Ctrl+Shift+A` and searching for it there.

[rbenv]: https://github.com/rbenv/rbenv
[ruby-build]: https://github.com/rbenv/ruby-build
[Rubymine]: https://www.jetbrains.com/ruby/
[pg]: https://bitbucket.org/ged/ruby-pg/wiki/Home