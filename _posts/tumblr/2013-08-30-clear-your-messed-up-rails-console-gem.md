---
layout: post
title: Clear your messed up rails console
date: '2013-08-30T19:37:00+05:30'
tags:
- rails
- ruby
- awesome_print
- gem
tumblr_url: http://charizard15.tumblr.com/post/59814666082/clear-your-messed-up-rails-console-gem
---
<p class="first-para">Clearly everybody using rails will have this problem. When your model gets bigger and bigger, you will find it difficult to spot your attributes in the rails console. So let&#8217;s clear this messed up console and format it a little bit. We have a gem called &#8216;<strong>awesome_print</strong>&#8217;. Install it using</p>
{% highlight ruby %}
gem install awesome_print
{% endhighlight %}
<p>Then open your ~/.irbrc file and add these lines to it.</p>
{% highlight ruby %}
# load libraries
require 'rubygems'
$LOAD_PATH<< '/path/to/awesome_print-x.x.x/lib'
require 'awesome_print'
{% endhighlight %}
<p>usually if you use RVM and ruby 1.9.3 your path to awesome_print gem will be like</p>
{% highlight ruby %}
/home/username/.rvm/gems/ruby-1.9.3-p448/gems/awesome_print-x.x.x/lib
{% endhighlight %}
<p>now fire up your rails console and try to print any object prepended with &#8216;ap&#8217;</p>
{% highlight ruby %}
ap User.first
{% endhighlight %}
<p>Hooray!!! We formatted the console. But wait, we missed something. Printing everything with &#8216;ap&#8217; prepended seems inappropriate. So let&#8217;s make awesome_print our default formatter. Now, open your ~/.irbrc again and add this line after requiring awesome_print.</p>
{% highlight ruby %}
AwesomePrint.irb!
{% endhighlight %}
<p>Now you will be able to format everything you print beautifully. And you can add options to awesome print. See the <a href="https://github.com/michaeldv/awesome_print#usage">documentation</a> for more details.</p>
<p>Enjoy hacking&#160;!!!</p>
