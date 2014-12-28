---
layout: post
title: The Basic Approach to any Dynammic Programming Problem
date: 2012-09-11 17:46:32.000000000 +05:30
categories:
- Dynammic Programming
tags:
- DP
- Dynammic Programming
- SPOJ
- TopCoder
status: publish
type: post
published: true
meta:
  _edit_last: '33499249'
  _wpas_done_facebook: '1'
  _wpas_done_twitter: '1'
author:
  login: yuvislm
  email: yuv.slm@gmail.com
  display_name: Yuvaraja
  first_name: ''
  last_name: ''
---
<p>The Following is an Answer given by Quorian <a href="http://www.quora.com/Michal-Danil%C3%A1k">Michal Danilak</a> when asked about DP. I was really impressed by the way he answered, so i am blogging it.</p>
<p>Dynamic programming is a very specific topic in programming competitions. No matter how many problems have you solved using DP, it can still surprise you. But as everything else in life, practice makes you better ;-)</p>
<p>Other answers in this thread mention some nice introductory texts that will help you understand what DP is and how it works. In the following few paragraphs I will try to show you how to come up with solutions for DP problems.</p>
<p>Note: the process of creating a DP solution, I describe below, is directly applicable for all the Div1-250 problems and many of Div1-500 problems on TopCoder which can be solved with DP. Harder problems usually requires some alternation in the process which you will be able to do, after some practice.</p>
<p>Note 2: the source code samples below are written in C++. If you don't know the language or are unsure of something, please ask me in comments.</p>
<p><strong>Iteration vs. recursion</strong></p>
<p>After you read some introductory texts on dynamic programming (which I highly recommend), pretty much all the source code examples in them use bottom-up technique with iteration (i.e. using for-cycles). For example calculating the length of the longest common subsequence of two strings <em>A</em> and <em>B</em> of length <em>N</em>, would look like this:</p>
{% highlight cpp %}
int dp[N+1][N+1];
for (int i = 0; i &lt;= N; ++i)
dp[0][i] = dp[i][0] = 0;
for (int i = 1; i &lt;= N; ++i)
for (int j = 1; j &lt;= N; ++j) {
dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
if (A[i-1] == B[j-1])
dp[i][j] = max(dp[i][j], dp[i-1][j-1]+1);
}</p>
<p>int answer = dp[N][N];
{% endhighlight %}
<p>There are couple of reasons why it is coded in this way:</p>
<ol>
<li>iteration is much faster than recursion</li>
<li>one can easily see time and space complexity of the algorithm</li>
<li>source code is short and clean</li>
</ol>
<p>Looking at such source code, one can understand how and why it works, but it is much harder to understand how to come up with it.</p>
<p>The biggest breakthrough in my learning of dynamic programming was, when I started to think about the problems in the top-down fashion, instead of bottom-up.</p>
<p>On the first look it doesn't look as such a revolutionary insight, but these two approaches directly translates in two different source codes. One uses iteration (bottom-up fashion) and the other one uses recursion (top-down fashion). The latter one is also called the memoization technique. The two solutions are more or less equivalent and you can always transform one into the other.</p>
<p>In the following paragraphs I will show you how to come up with a memoization solution for a problem.</p>
<p><strong>Motivation problem</strong></p>
<p>Imagine you have a collection of <em>N</em> wines placed next to each other on a shelf. For simplicity, let's number the wines from left to right as they are standing on the shelf with integers from 1 to <em>N</em>, respectively. The price of the <em>i</em>-th wine is <em>pi</em> (prices of different wines can be different).</p>
<p>Because the wines get better every year, supposing today is the year 1, on year <em>y</em>the price of the <em>i</em>-th wine will be <em>y</em>*<em>pi</em>, i.e. <em>y</em>-times more than the current year.</p>
<p>You want to sell all the wines you have, but you want to sell exactly one wine per year, starting on this year. One more constraint - on each year you are allowed to sell only either the leftmost or the rightmost wine on the shelf and you are not allowed to reorder the wines on the shelf (i.e. they must stay in the same order as they are in the beginning).</p>
<p>You want to find out, what is the maximum profit you can get, if you sell the wines in optimal order.</p>
<p>So for example, if the prices of the wines are (in the order as they are placed on the shelf, from left to right): <em>p1=</em>1, <em>p2=</em>4, <em>p3=</em>2, <em>p4=</em>3
The optimal solution would be to sell the wines in the order <em>p1, p4, p3, p2</em> for a total profit 1*1 + 3*2 + 2*3 + 4*4 = 29</p>
<p><strong>Wrong solution</strong></p>
<p>After playing with the problem for a while, you'll probably get the feeling, that in the optimal solution you want to sell the expensive wines as late as possible. You can probably come up with the following greedy strategy:</p>
<blockquote><p>Every year, sell the cheaper of the two (leftmost and rightmost) available wines.</p></blockquote>
<p>Although the strategy doesn't mention what to do when the two wines cost the same, this strategy kinda feels right. But unfortunately, it isn't, as the following example demonstrates. If the prices of the wines are: <em>p1</em>=2, <em>p2</em>=3, <em>p3</em>=5, <em>p4</em>=1,<em>p5</em>=4</p>
<p>The greedy strategy would sell them in the order <em>p1, p2, p5, p4, p3</em> for a total profit 2*1 + 3*2 + 4*3 + 1*4 + 5*5 = 49</p>
<p>But we can do better if we sell the wines in the order <em>p1, p5, p4, p2, p3 </em>for a total profit 2*1 + 4*2 + 1*3 + 3*4 + 5*5 = 50</p>
<p>This counter-example should convince you, that the problem is not so easy as it can look on a first sight and I will tell you, that it can be solved using DP.</p>
<p><strong>Write a backtrack</strong></p>
<p>When coming up with the memoization solution for a problem, I always start with a backtrack solution that finds the correct answer. Backtrack solution enumerates all the valid answers for the problem and chooses the best one. For most of the problems it is easy to come up with such solution.</p>
<p>Here are some restrictions I put on a backtrack solution:</p>
<ul>
<li>it should be a function, calculating the answer using recursion</li>
<li>it should return the answer with <em>return</em> statement, i.e. not store it somewhere</li>
<li>all the non-local variables that the function uses should be used as read-only, i.e. the function can modify only local variables and its arguments.</li>
</ul>
<p>So for the problem with wines, the backtrack solution will look like this:</p>
{% highlight cpp %}
int p[N]; // read-only array of wine prices</p>
<p>// year represents the current year (starts with 1)
// [be, en] represents the interval of the unsold wines on the shelf
int profit(int year, int be, int en) {
// there are no more wines on the shelf
if (be &gt; en)
return 0;</p>
<p>// try to sell the leftmost or the rightmost wine, recursively calculate the
// answer and return the better one
return max(
profit(year+1, be+1, en) + year * p[be],
profit(year+1, be, en-1) + year * p[en]);
}
{% endhighlight %}
<p>We can get the answer by calling:</p>
{% highlight cpp %}
int answer = profit(1, 0, N-1); // N is the total number of wines
{% endhighlight %}
<p>This solution simply tries all the possible valid orders of selling the wines. If there are <em>N</em> wines in the beginning, it will try 2^<em>N</em> possibilities (each year we have 2 choices). So even though now we get the correct answer, the time complexity of the algorithm grows exponentially.</p>
<p>The correctly written backtrack function should always represent an answer to a well-stated question. In our case <em>profit</em> function represents an answer to a question: "What is the best profit we can get from selling the wines with prices stored in the array <em>p</em>, when the current year is <em>year</em> and the interval of unsold wines spans through [<em>be,</em><strong><em> </em></strong><em>en</em>], inclusive?"
You should always try to create such a question for your backtrack function to see if you got it right and understand exactly what it does.</p>
<p><strong>Minimize the state space of function arguments</strong></p>
<p>In this step I want you to think about, which of the arguments you pass to the function are redundant. Either we can construct them from the other arguments or we don't need them at all. If there are any such arguments, don't pass them to the function. Just calculate them inside the function.</p>
<p>In the above function <em>profit</em>, the argument <em>year</em> is redundant. It is equivalent to the number of wines we have already sold plus one, which is equivalent to the total number of wines from the beginning minus the number of wines we have not sold plus one. If we create a read-only global variable <em>N</em>, representing the total number of wines in the beginning, we can rewrite our function as follows:</p>
{% highlight cpp %}
int N; // read-only number of wines in the beginning
int p[N]; // read-only array of wine prices</p>
<p>int profit(int be, int en) {
if (be &gt; en)
return 0;</p>
<p>// (en-be+1) is the number of unsold wines
int year = N - (en-be+1) + 1; // as in the description above
return max(
profit(be+1, en) + year * p[be],
profit(be, en-1) + year * p[en]);
}
{% endhighlight %}
<p>I also want you to think about the range of possible values the function arguments can get from a valid input. In our case, each of the arguments <em>be</em> and <em>en</em> can contain values from 0 to <em>N</em>-1. In valid inputs we also expect <em>be</em> &lt;= <em>en</em>+1. Using big-O notation we can say, there are O(<em>N</em>^2) different arguments our function can be called with.</p>
<p><strong>Now cache it</strong>!</p>
<p>We are now 99% done. To transform the backtrack function with time complexity O(2^<em>N</em>) into the memoization solution with time complexity O(<em>N</em>^2) we will use a little trick which doesn't require almost any thinking.</p>
<p>As noted above, there are only O(<em>N</em>^2) different arguments our function can be called with. In other words, there are only O(<em>N</em>^2) different things we can actually compute. So where does O(2^N) time complexity comes from and what does it compute?!</p>
<p>The answer is - the exponential time complexity comes from the repeated recursion and because of that, it computes the same values again and again. If you run the above code for an arbitrary array of <em>N</em>=20 wines and calculate how many times was the function called for arguments <em>be</em>=10 and <em>en</em>=10 you will get a number 92378. That's a huge waste of time to compute the same answer that many times. What we can do to improve this is to cache the values once we have computed them and every time the function asks for an already cached value, we don't need to run the whole recursion again. See the code below:</p>
{% highlight cpp %}
int N; // read-only number of wines in the beginning
int p[N]; // read-only array of wine prices
int cache[N][N]; // all values initialized to -1 (or anything you choose)</p>
<p>int profit(int be, int en) {
if (be &gt; en)
return 0;</p>
<p>// these two lines save the day
if (cache[be][en] != -1)
return cache[be][en];</p>
<p>int year = N - (en-be+1) + 1;
// when calculating the new answer, don't forget to cache it
return cache[be][en] = max(
profit(be+1, en) + year * p[be],
profit(be, en-1) + year * p[en]);
}
{% endhighlight %}
<p>And that's it! With that little trick it runs O(<em>N</em>^2) time, because there are O(<em>N^2)</em>different arguments our function can be called with and for each of them, the function runs only once with O(1) time complexity.</p>
<p>Note: when the values are cached, you can treat every recursive call inside the function as it would run in O(1) time complexity.</p>
<p><strong>Summary</strong></p>
<p>To sum it up, if you identify that a problem can be solved using DP, try to create a backtrack function that calculates the correct answer. Try to avoid the redundant arguments, minimize the range of possible values of function arguments and also try to optimize the time complexity of one function call (remember, you can treat recursive calls as they would run in O(1) time). Finally cache the values and don't calculate the same things twice.</p>
<p>The final time complexity of the solution is:
<strong>range_of_possible_values_the_function_can_be_called_with </strong>x
<strong>time_complexity_of_one_function_call.</strong></p>
