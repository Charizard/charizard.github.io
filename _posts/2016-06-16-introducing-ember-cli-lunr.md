---
layout: post
title: Introducing ember-cli-lunr
tags:
- ember.js
- lunr.js
- ember-addon
- search
- indexing
---

I was working on client side searching in an ember application. The requirements was very simple, consider the following array of strings,

{% highlight javascript %}
"We aim to be simple to integrate with any modern Rails application",
"We need an application that can display the current state of the orders",
"The intent behind it was to keep setup as easy as possible"
{% endhighlight %}

and I search for the query `"application"`, I should get the following as output.

{% highlight javascript %}
"We aim to be simple to integrate with any modern Rails application",
"We need an application that can display the current state of the orders"
{% endhighlight %}

But, if I search for `"current orders"`, I should get the following as output,

{% highlight javascript %}
"We need an application that can display the current state of the orders"
{% endhighlight %}

So clearly, it was keyword based searching.

I ended up finding just the right library, [lunr.js](http://lunrjs.com/). I wanted it to work with my ember model's very robustly. So I ended up creating a ember-addon for it and named it (according to the pseudo addon naming convention) as [ember-cli-lunr](https://github.com/Charizard/ember-cli-lunr).

Hope you all find this addon useful in your ambitious search applications. And please help me make this ember addon better by submitting PR, feature request, issues or just a thank you (your appreciation moves me forward).
