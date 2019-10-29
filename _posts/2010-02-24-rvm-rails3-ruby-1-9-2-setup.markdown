---
title: Rails 3.0 Setup using rvm & Ruby 1.9.2
date: 2010-02-24 00:00:00 -0800
---
##Intro
People want to start working on apps in  Rails 3.0. Rails 3 is supporting ruby 1.8.7 and ruby 1.9.2. These instructions will assist you with getting Rails 3 and ruby 1.9.2 installed. I'm tossing this up here to have something to point people to when they have questions.


I'm making a few assumptions:

1. You're on OS X Snow Leopard (x86_64) (Most of these instructions will work on any \*nix box. Leave a comment if you have trouble.)
2. You have macports install (or readline installed somewhere else on your box)

##Install Ruby Version Manager

<div class="panel">
  Note: Just run this command and follow the instructions.
</div>

{% highlight console %}
$ bash < <( curl http://rvm.beginrescueend.com/releases/rvm-install-head )
{% endhighlight %}

##Install Ruby 1.9.2
<div class="panel">
Your readline directory may be in a different spot.
</div>

{% highlight console %}
$ rvm install 1.9.2 -C --with-readline-dir=/opt/local,--build=x86_64-apple-darwin10
Installing Ruby from source to: /Users/mturner/.rvm/rubies/ruby-1.9.2-p0

Running autoconf

Configuring ruby-1.9.2-p0, this may take a while depending on your cpu(s)...

Compiling ruby-1.9.2-p0, this may take a while, depending on your cpu(s)...

Installing ruby-1.9.2-p0

Installation of ruby-1.9.2-p0 is complete.

Updating rubygems for ruby-1.9.2-p0

Installing gems for ruby-1.9.2-p0.

Installing rake

Installation of gems for ruby-1.9.2-p0 is complete.

$ rvm 1.9.2

$ ruby -v
ruby 1.9.2p0
{% endhighlight %}


##Create a Rails 3.0 Gem set and switch to it
This processes allows us to isolate the Rails 3.0 environment gems.
{% highlight console %}
$ rvm use --create 1.9.2@rails3
{% endhighlight %}


##Install the Rails 3.0 Gems and dependencies
{% highlight console %}
$ gem install sqlite3-ruby
$ env ARCHFLAGS="-arch x86_64" gem install mysql -- --with-mysql-config=/usr/local/mysql/bin/mysql_config
$ gem install rails
{% endhighlight %}


##Done
Hopefully everything worked:
{% highlight console %}
$ ruby -v
  ruby 1.9.2p0 (2010-08-18 revision 29036) [x86_64-darwin10.4.0]
$ rails --version
  Rails 3.0.0
$ gem list
  *** LOCAL GEMS ***

  abstract (1.0.0)
  actionmailer (3.0.0)
  actionpack (3.0.0)
  activemodel (3.0.0)
  activerecord (3.0.0)
  activeresource (3.0.0)
  activesupport (3.0.0)
  arel (1.0.1)
  builder (2.1.2)
  bundler (1.0.0.rc.5)
  erubis (2.6.6)
  i18n (0.4.1)
  mail (2.2.5)
  memcache-client (1.8.5)
  mime-types (1.16)
  mysql (2.8.1)
  rack (1.2.1)
  rack-mount (0.6.12)
  rack-test (0.5.4)
  rails (3.0.0)
  railties (3.0.0)
  rake (0.8.7)
  sqlite3-ruby (1.3.1)
  text-format (1.0.0)
  text-hyphen (1.0.0)
  thor (0.14.0)
  tzinfo (0.3.22)
{% endhighlight %}

##Switching back to your system Ruby
`rvm system`

##Back to your Rails3.0 environment
`rvm 1.9.2@rails3`

##Use your RVM environment as your Default
`rvm 1.9.2@rails3 --default`

Read more about RVM over at [http://rvm.beginrescueend.com/](http://rvm.beginrescueend.com/)

###Update 04/16/2010
Updated to reflect the recent changes in RVM.

###Update 08/18/2010
Updated to use the production release of 1.9.2

###Update 08/30/2010
Updated to reflect the Rails 3.0 release.
