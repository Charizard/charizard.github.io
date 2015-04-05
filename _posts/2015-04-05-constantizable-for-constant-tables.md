---
layout: post
title: Constantizable, for constant tables
tags:
- ruby
- rails
- constantizable
---

It all started with a project in which I was working on, it contained too many constant tables like Country, City etc. It was a team of 3 people including me. We had tough time querying to check if the user belonged to a certain country.

{% highlight ruby %}
if user.country.name == "India"
  # Some logic
else
  # Some logic
end
{% endhighlight %}

The above code was present all over the place. Being obsessed with elegance, we all wanted something like the following,

{% highlight ruby %}
if user.country.india?
  # Some logic
else
  # Some logic
end
{% endhighlight %}

Additionally, there were many places in which we had to query for the country record.

{% highlight ruby %}
  Country.find_by_name("United States of America")
{% endhighlight %}

And again we felt something wrong and wanted it to be more elegant and easy to read, so we decided we wanted to replace it with this,

{% highlight ruby %}
  Country.united_states_of_america
{% endhighlight %}

We wrote a module to be included in all the constant model's and being open source enthusiasts we extracted it into a gem which is hosted in [github](https://github.com/Codebrahma/constantizable).

This module is young, so we need support from people to make it better. So feel free to contribute to the repo.
