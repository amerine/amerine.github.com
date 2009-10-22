---
layout: post
title: Rails Stack on OpenSolaris 2009.06
---
I enjoy using [OpenSolaris](http://opensolaris.org) when I can, and I've been using it for a couple small Ruby on Rails deployments. I've received a few questions recently about deploying Ruby on Rails app on [OpenSolaris](http://opensolaris.org), so to kill a few 'birds' with one stone. I'm throwing this post together so that I can just point them here and let them run with it. 

{% include ad-block.markdown %}

Sun has an easy to install meta-package called `amp` that includes Apache 2.2, Apache 2.2 DTrace probes, MySQL Server 5.1 & PHP. If you've never used Solaris/OpenSolaris before the `pfexec` command will seem foreign to you. Don't let it worry you its part of the Solaris RBAC (Role Based Access Control) by default the non-root user you created during the install has the `root` user role and can execute commands as root if you use `pfexec`.

I'm assuming you're starting from scratch on an OpenSolaris 2009.06 server with nothing extra installed. I also assume you've had some experience with a Unix/Linux variant.

## Basics ##
We want to install some of the GNU stack including `gmake`, `gcc` and git + subversion. We're going to install git from source to be sure we're on the latest release.

### gmake, gcc, subversion and curl ###
Commands:
{% highlight bash %}
pfexec pkg install SUNWgmake
pfexec pkg install SUNWgcc
pfexec pkg install SUNWsvn
pfexec pkg install SUNWcurl
{% endhighlight %}
### git ###
`curl` is required to build `git` don't skip installing `SUNWcurl` from above.

Note: There seems to be an issue with building versions of git newer than 1.6.0.6 on OpenSolaris 2009.06. I'll figure out why but in the meantime I've provided instructions for installing git 1.6.0.6. Please verify that `/usr/local/bin` is in your `$PATH`.

Commands:
{% highlight bash %}
pfexec mkdir -p /opt/src
pfexec chmod 777 /opt/src
cd /opt/src
wget http://kernel.org/pub/software/scm/git/git-1.6.0.6.tar.gz
tar xzf git-1.6.0.6.tar.gz
git-1.6.0.6
./configure
gmake
pfexec make install 
git --version
{% endhighlight %}
## Apache Stack (AMP) ##
Like I noted above, OpenSolaris has a meta-package we can install that will take care of most of this. It will install the following packages for us:

* MySQL Server 5.1
* Apache 2.2.30
* PHP 5.2.9

Commands:
{% highlight bash %}
pfexec pkg install amp
{% endhighlight %}
### Enable Apache & MySQL Services ###
By default the Apache and MySQL services are not enabled. To turn them on run the following commands:

Apache 2:
{% highlight bash %}
pfexec svcadm enable http:apache22
{% endhighlight %}
MySQL
{% highlight bash %}
pfexec svcadm enable mysql
{% endhighlight %}

## Ruby & Ruby on Rails ##
First we are going to use the Sun provided Ruby 1.8.7 package that also installs rubygems. Then we will update rubygems, install the gemcutter gem, add http://gems.github.com as a gem source (for legacy purposes), install the rails gems and finally modify our path to include the gem binaries. 
{% highlight bash %}
pfexec pkg install SUNWruby18
pfexec gem update --system
pfexec gem install gemcutter
gem tumble
gem sources -a http://gems.github.com
pfexec gem install rails
echo 'export PATH=/usr/ruby/1.8/bin:$PATH' >> ~/.profile
echo 'export PATH=/var/ruby/1.8/gem_home/bin:$PATH' >> ~/.profile
{% endhighlight %}
(Note: Modifying .profile requires you to re-login to the machine, as it's only read during that point. If you like you can add that same line to your .bashrc file)

### Install Database Gems ###
Mysql:
{% highlight bash %}
pfexec gem install mysql -- --with-mysql-dir=/usr/mysql/5.1
{% endhighlight %}	
sqlite3
{% highlight bash %}
pfexec gem install sqlite3-ruby
{% endhighlight %}	
	
## Passenger Installation ###
We need to install fastthread and passenger, then configure apache to use it. We're going to install passenger from source as it fixes a bug with the use of PTHREAD_STACK_MIN (See [here](http://code.google.com/p/phusion-passenger/issues/detail?id=369)) This is fixed in passenger 2.2.6 and I will update this when that is released. 
{% highlight bash %}
pfexec gem install fastthread
cd /opt/src
git clone git://github.com/FooBarWidget/passenger.git
cd passenger
pfexec ./bin/passenger-install-apache2-module
{% endhighlight %}	
### Apache Configuration ###
We're going to keep the passenger configuration in it's own file to keep the httpd.conf clean.

Create the file:
{% highlight bash %}
touch /etc/apache2/2.2/conf.d/passenger.conf
{% endhighlight %}	
Paste the lines from the passenger-install-apache2-module command in that file. My file contains the following:
{% highlight bash %}
LoadModule passenger_module /opt/src/passenger/ext/apache2/mod_passenger.so
PassengerRoot /opt/src/passenger
PassengerRuby /usr/ruby/1.8/bin/ruby
{% endhighlight %}
## Let's Test ##
Lets create a little rails app to verify that rails and passenger 
{% highlight bash %}
pfexec mkdir -p /data/apps
pfexec chown -R mturner:staff /data/apps
cd /data/apps
rails test
cd test
./script/generate controller helloworld index
pfexec chown -R webservd:webservd test
{% endhighlight %}	
Lets add a vhost config for apache. 
{% highlight bash %}
pfexec vim /etc/apache2/2.2/conf.d/test-site.conf
{% endhighlight %}	
Here is the contents of my file:
{% highlight bash %}
<VirtualHost *:80>
     ServerName test-site.com
     DocumentRoot /data/apps/test/public

     <Directory /data/apps/test>
      Options +FollowSymLinks -SymLinksIfOwnerMatch +MultiViews -Indexes -ExecCGI
      AllowOverride ALL
      Order allow,deny
      Allow from all
     </Directory>

</VirtualHost>
{% endhighlight %}
Add the `127.0.0.1 test-site.com` to `/etc/hosts`
	
Restart Apache
{% highlight bash %}
pfexec svcadm restart http:apache22
{% endhighlight %}	
Test the site in apache and you see the following:
{% highlight ruby %}
Helloworld#index

Find me in app/views/helloworld/index.html.erb
{% endhighlight %}
It Works!!