---
layout: post
title: A Facebook Birthday Problem
comments: true
---

<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-76610615-1', 'auto');
  ga('send', 'pageview');

</script>

**Why isn't it anyone's birthday today?**

That was my first thought when I logged into Facebook the other day. If you have fewer than 365 friends, the question is hardly worth asking. As your network builds into the 1000s though, it becomes less obvious. In fact, there seems to be a fun little problem here-- how many friends do I need before I can be reasonably sure that every day is someone's birthday?

This problem ends up being somewhat similar to the classic <a href="http://en.wikipedia.org/wiki/Birthday_problem"> Birthday Problem </a> that is covered in most introductory probability courses. The question asks, "how many people are needed in a room before there's a greater than 50% chance that two people share the same birthday?" It often surprises people to find that it only takes ~23 (check the link if you're not convinced). 

In our case, we can work the math for the facebook birthday problem with the following logic. First, let's assume there are no leap years and that a person is equally likely to be born on any day of the year. Imagine that we have a 365 day calendar spread out on the floor and an unlimited number of poker chips with people's faces to throw onto it. If we assume that all days of the year are equally likely, then our chance of tossing a chip and having it land on any specific day of the calendar is 1/365. So let's toss the first person--we have a 365/365 = 100% chance they land on a new birthday. We grab another chip. When we toss this person, there's 1 space taken up, so there's only a 364/365 = 99.7% chance that the chip lands on a new day.  Not bad odds at all.Fast forwarding a bit, let's go to the 301st person. At this point, we have filled 300 open spots, so there's only a 65/365 = 17.8% chance of landing on a unique birthday.

But we don't really care about the probability that a person lands on a new day--we want the number of chips we would expect to toss. To do this, we need to find the expected value of the number of tosses. Quick review: if we're waiting for something to happen with a probability of *p*, then we can expect that it'll take 1/*p* times for that event to happen. With the first person in our birthday problem, there's an expected value of 1/1 = 1 person. For the second, it's 1/(364/365) = 1.003 people. For the 301st person, it's 1/(65/365) = 5.6 people. For our very last person, it's expected that it will take 1/(1/365) = *365 people*. 

So we start by befriending people until we get a unique person with 365 open spaces on the board (should take 1 new friend). Then, we befriend people until we get a unique person with 364 open spaces (should take 1.003 new friends). And so on, and so on. Mathematically, we can express this as:

<div style="text-align: center;"><img src="https://raw.githubusercontent.com/sandeepba/sandeepba.github.com/master/assets/bday-sum.gif" /></div>



This sum can be calculated in python as follows:

{% highlight python %}
numFriends = 0;

for x in range(0,365):
    # x is the number of taken birth dates 
    # probability of a new birthday (365-x)/365
    # expected value is 1/probability b/c it's geometric
	numFriends += (365/(365-float(x)))

print int(numFriends)

{% endhighlight %}

So there you have it. It takes about **2365** friends before you have a greater than 50% chance of covering all birthdays!  
