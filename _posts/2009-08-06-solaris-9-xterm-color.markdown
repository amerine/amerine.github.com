---
layout: post
title: Solaris 9 and xterm-color
---
If you connect to Solaris 9 from a OS X or any recent Linux box via SSH, chances are your terminal is set to xterm-color. If you try to use vi or anything you'll see an error like below:

{% highlight bash %}
xterm-color: Unknown terminal type
{% endhighlight %}

Its easy enough to change the terminal declaration in the preference dialog... but why not teach Solaris 9 to handle the xterm-color terminal type? 

This assumes you have the sun freeware tools installed in /usr/sfw/bin.


Download [this file](http://amerine.net/files/xterm-color) and place it in /usr/share/lib/terminfo/x/ Example: 

{% highlight bash %}
cd /usr/share/lib/terminfo/x/
/usr/sfw/bin/wget http://amerine.net/files/xterm-color
{% endhighlight %}

Now that annoying error won't stop vi from working. Enjoy.