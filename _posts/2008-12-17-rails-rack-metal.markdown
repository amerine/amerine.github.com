--- 
title: Rails::Rack::Metal
typo_id: 17
layout: post
---
<p>As usual there has been quit the storm over a commit in edge rails. <a target="_blank" href="http://is.gd/c1if">Link</a>&nbsp;</p>
<p>Some people don't understand Metal, but its pretty simple and I wanted to put out a post explaining it and hopefully you learn a little bit.&nbsp;</p>
<p>Metal(s) allows you to design data access points that bypass most of the rails routing and rendering code. So instead of exposing data through normal controllers we can write better performing actions through metal(s).&nbsp;Metal allows you to run ruby right at the webserver layer, and the main reason for that is speed.</p>
<p>In this example I am offering a simple service where someone can hit a URL('/grab/<id>' in this case) and get back the User objects name attribute.</id></p>
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
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>This is a very simple example, but more complex ones are VERY easy to implement. Outputting XML or Binary data is just as easy as long as you know what the requester expects (text/xml for example).&nbsp;</p>
<p>Its does require a more work to present data through Metal(s) but the speed benefits may come in handy on you're next project. I assume that anyone that has to provide an API would love to avoid some of the other rails &quot;baggage&quot; when processing requests.&nbsp;</p>
<p>&nbsp;</p>
