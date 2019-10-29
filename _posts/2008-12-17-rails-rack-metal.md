---
title: Rails::Rack::Metal
date: 2008-12-17 00:00:00 -0800
---
As usual there has been quite the storm over a commit in edge rails. [Link](http://is.gd/c1if)

Some people don't understand Metal, but its pretty simple and I wanted to put out a post explaining it and hopefully you learn a little bit.

Metal(s) allows you to design data access points that bypass most of the rails routing and rendering code. So instead of exposing data through normal controllers we can write better performing actions through metal(s).

In this example I am offering a simple service where someone can hit a URL('/grab/&lt;id&gt;' in this case) and get back the User objects name attribute.&lt;/id&gt;

{% highlight ruby %}
# Allow the metal piece to run in isolation
require(File.dirname(__FILE__) + "/../../config/environment") unless defined?(Rails)

class Grab < Rails::Rack::Metal
  def call(env)
    if env["PATH_INFO"] =~ /^\/grab/
      request = env['REQUEST_URI'].split('/')
      if request.size != 3
        response_text = "No ID Provided"
        response_code = 200
      else
        begin
          username = User.find(request[2]).name
          response_text = username
          response_code = 200
        rescue
          response_text = "Request Failed"
          response_code = 400
        end
      end
      [response_code.to_i, {"Content-Type" => "text/html"}, response_text]
    else
      super
    end
  end
end
{% endhighlight %}

This is a very simple example, but more complex ones are VERY easy to implement. Outputting XML or binary data is just as easy.

Its does require a more work to present data through Metal(s) but the speed benefits may come in handy on you're next project. I assume that anyone that has to provide an API would love to avoid some of the other rails &quot;baggage&quot; when processing requests.
