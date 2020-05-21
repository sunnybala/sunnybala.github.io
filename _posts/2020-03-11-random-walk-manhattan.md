---
layout: post
title: A Random Walk Through Manhattan
comments: true
---


<style>
.center {
    display: block;
    margin-left: auto;
    margin-right: auto;
}

img height="400" {
	height: 250 px;
}

</style>

[**jump to code**](https://github.com/sunnybala/sunnybala.github.io/blob/master/assets/notebooks/manhattan_random_walk.ipynb)

<img height="900" src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/animated_compress_7.gif" 
class="center"/>

At the time of writing this, we're currently under stay at home orders in observation of the Covid-19 pandemic. Taking those nice long walks around Manhattan at this time is ill-advised, but how about a digital one? In particular, here I'll explore how to generate random walks within a map network. 

What would happen if every time you walked up to an intersection you just turned in a random direction? Here's one run of the simulation. Like real people, the random walk gets pretty lost in the dense streets of lower Manhattan. If you're interested in generating one of these for yourself or just seeing the code, feel free to follow along with the [**jupyter notebook**](https://github.com/sunnybala/sunnybala.github.io/blob/master/assets/notebooks/manhattan_random_walk.ipynb) via my GitHub. Making this was so much easier with the amazing [osmnx](https://github.com/gboeing/osmnx) module, which allows you to grab map networks from OpenStreetMap. 

