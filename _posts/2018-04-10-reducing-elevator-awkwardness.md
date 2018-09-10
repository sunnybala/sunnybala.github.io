---
layout: post
title: Reducing Elevator Awkwardness With Python
comments: true
---

<style>
.center {
    display: block;
    margin-left: auto;
    margin-right: auto;
}

</style>

*Elevators are awkward.* Our office recently recently had some work done on the stairs and required everyone to take elevators for several weeks. This led to some congestion and tightly packed spaces -- it was a great way to feel close to co-workers! There wasn't anything unusual about this. At every floor, a few people people would get out. The remaining people would shift around a bit to use up the new, free space. This got me thinking. How do people determine where to stand? <a href="#bingo"> **What are the optimal arrangements to minimize awkwardness?** <a/> 

## The Plan

Simulate groups of *n* people in an elevator. Find some quantitative way to measure elevator awkwardness and determine the arrangements that minimize it. 

### 1. Measuring Awkwardness

What makes an elevator uncomfortable? One natural idea is how close people are to each other. For example, the situation on the left seems far more awkward than the one on the right.

<img src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/awk_ele_lr.PNG" class="center"/>

Another factor to consider is that

