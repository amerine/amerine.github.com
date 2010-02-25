--- 
title: Rails 3.0 Setup using rvm & Ruby 1.9.2
layout: post
---
###Intro
Some people want to start doing some Rails 3.0 Beta work on Ruby 1.9.2 on their development machines. I'm tossing this up here to have something to point people to when they have questions. 

I'm making a few assumptions:  

1. You're on OS X Snow Leopard (x86_64)
2. You have macports install (or readline installed somewhere else on your box) 

###Install Ruby Version Manager  

{% highlight bash %}
$ sudo gem install rvm
$ rvm-install
{% endhighlight %}

###Install Ruby 1.9.2 HEAD (i.e. latest development code)
> Note: Your readline directory may be in a different spot.  

{% highlight bash %}
$ rvm install 1.9.2-head -C --enable-shared,--with-readline-dir=/opt/local,--build=x86_64-apple-darwin10
Installing Ruby from source to: /Users/mturner/.rvm/rubies/ruby-1.9.2-head

Running autoconf

Configuring ruby-1.9.2-head, this may take a while depending on your cpu(s)...

Compiling ruby-1.9.2-head, this may take a while, depending on your cpu(s)...

Installing ruby-1.9.2-head

Installation of ruby-1.9.2-head is complete.

Updating rubygems for ruby-1.9.2-head

Installing gems for ruby-1.9.2-head.

Installing rake

Installation of gems for ruby-1.9.2-head is complete.

$ ruby -v
ruby 1.9.2dev (2010-02-25 trunk 26759) [x86_64-darwin10.2.0]
{% endhighlight %}


###Create a Rails 3.0 Gem set and switch to it
This processes allows us to isolate the Rails 3.0 environment gems. 
{% highlight bash %}
$ rvm gems create rails3beta
$ rvm 1.9.2-head%rails3beta
{% endhighlight %}

###Install the Rails 3.0 Gems and dependencies 
{% highlight bash %}
$ gem install sqlite3-ruby
$ env ARCHFLAGS="-arch x86_64" gem install mysql -- --with-mysql-config=/usr/local/mysql/bin/mysql_config
$ gem install tzinfo builder memcache-client rack rack-test rack-mount erubis mail text-format thor bundler i18n
$ gem install rails --pre
{% endhighlight %}

###Done
Hopefully everything worked:
{% highlight bash %}
$ ruby -v
  ruby 1.9.2dev (2010-02-25 trunk 26759) [x86_64-darwin10.2.0]
$ rails --version
  Rails 3.0.0.beta
$ gem list
  *** LOCAL GEMS ***

	abstract (1.0.0)
	actionmailer (3.0.0.beta)
	actionpack (3.0.0.beta)
	activemodel (3.0.0.beta)
	activerecord (3.0.0.beta)
	activeresource (3.0.0.beta)
	activesupport (3.0.0.beta, 2.3.5)
	arel (0.2.1)
	builder (2.1.2)
	bundler (0.9.7)
	erubis (2.6.5)
	i18n (0.3.3)
	mail (2.1.3)
	memcache-client (1.7.8)
	mime-types (1.16)
	mysql (2.8.1)
	rack (1.1.0)
	rack-mount (0.6.0, 0.4.7)
	rack-test (0.5.3)
	rails (3.0.0.beta)
	railties (3.0.0.beta)
	rake (0.8.7)
	sqlite3-ruby (1.2.5)
	text-format (1.0.0)
	text-hyphen (1.0.0)
	thor (0.13.3)
	tzinfo (0.3.16)
{% endhighlight %}

###Switching back to you're system Ruby
`rvm system`

###Back to you're Rails3.0 environment
`rvm 1.9.2-head%rails3beta`