---
layout: post
title: Let's debug easily - git bisect
date: '2013-08-25T09:26:00+05:30'
tags:
- git
- debugging
- gitbisect
- binarysearch
tumblr_url: http://charizard15.tumblr.com/post/59296117714/lets-debug-easily-git-bisect
image:
  feature: git.png
---
<p class="first-para">Imagine the scenario where your code has been reported for a bug and you don&#8217;t know exactly which part for your commit history induced this particular bug. But you sure know a past commit where this bug never existed (let&#8217;s call it &#8220;good commit&#8221;) and there are around 100 commits in between the &#8220;good commit&#8221; and your current commit(which we can call as &#8220;bad commit&#8221;).</p>

<p>So you are in a situation where you want to review all the 100 commits in between the &#8220;good commit&#8221; and the &#8220;bad commit&#8221;. Not a plausible solution, is it?</p>

<p>So what&#8217;s the way to avoid &#8220;<strong>searching</strong>" all 100 commits to find the buggy code. You got it right. Yes, do a binary search. Since we are lazy, we don&#8217;t want to binary search 100 commits which will take us log<sub>2</sub>(100) = 7 (approx.) steps to find the buggy branch. Luckily we have a life saver tool called <strong>bisect tool </strong>from git. Let us see how it works.</p>

<p>In your current branch, you have to do</p>
{% highlight bash %}
git bisect start
{% endhighlight %}
<p><strong><br/></strong>This command simple starts the bisect tool.Then you have to mark the bad commit as bad. We can do that using</p>
{% highlight bash %}
git bisect bad
{% endhighlight %}
<p>Then we have to tell the bisect tool about the good branch. You can give the commit hash of the good commit. You can obtain the commit hash using </p>
{% highlight bash %}
git log
{% endhighlight %}
<p>and then</p>
{% highlight bash %}
git bisect good [good commit hash]
{% endhighlight %}
<p>Now you will be surprised to see that the bisect tool takes you to the center commit of the good and the bad commit. At this point, you can run your test and see if the bug that was reported exists. If it doesn&#8217;t, then you can mark the commit as good using</p>
{% highlight bash %}
git bisect good
{% endhighlight %}
<p>else you can mark it as bad using</p>
{% highlight bash %}
git bisect bad
{% endhighlight %}
<p>So this takes you to the left or the right of the current commit you are on. This looks so simple, ain&#8217;t it?</p>
<p>Finally at the end don&#8217;t forget to stop the bisect tool using</p>
{% highlight bash %}
git bisect reset
{% endhighlight %}
<p><span>this will reset your HEAD pointer to where you were before you started, else you will end up in a wierd state.</span></p>
<p><strong>Reference:</strong></p>
<p><a href="http://robots.thoughtbot.com/post/57797091118/git-bisect" target="_blank">Thoughtbot Blog post on git bisect</a></p>
<p><a href="http://git-scm.com/book/en/Git-Tools-Debugging-with-Git#Binary-Search" target="_blank">git-scm reference</a></p>
<p>Happy hacking&#8230;.!!!</p>
