---
layout: post
title: Javascript Snake
comments: true
---
**I coded up snake for practice. </a>** 

**2018 Update -- I've since taken the game down since it's not really worth maintaining. Keeping the post up just for the archive.**

As a kid, I spent a lot of time playing flash games online. One of my jobs this summer was converting a flash based dashboard to an iOS friendly one based on Javascript and SVG and it got me thinking– can we expect more and more of the addicting games on sites like MiniClip or Armor Games to similarly move away from flash?  After all, it does seem like the iOS market share is only expanding.

To take a stab at the questions I wrote a basic implementation of the game Snake using Javascript with the d3 library. Below, I’m going to briefly cover some of the lessons I’ve learned.

1. It was a bit of a leap for me to start thinking about the snake as just a Javascript array. In my mind I had assumed that I would need a separate head and then a separate set of body chunk that together make the snake. Once I began to look at the snake this way I could then address the issue of…

2. Speed. I was worried that constantly redrawing the snake between every frame would slow down the browser and possibly cause bugs if the redraws took more time than the scheduled step size (2oo ms) between every movement. However, representing the snake as an array allowed me to take a shortcut. Instead of redrawing the whole snake every time, I could just move the last piece of the snake to the the new position. This worked out especially well for food, as I could just convert a food block to a snake class and it would join the rest.

3. The trickiest bit of this whole endeavor was self collision detection. In a previous matlab game I wrote, I relied on checking the pixel colors as an indication of what I was dealing with. However, as far as I know, the d3 library does all sorts of wonderful things to manipulate SVG but there is no function to return the color of a specific pixel coordinate. Unfortunately, javascript also can’t natively test for equality between objects or arrays. Instead, I had to use the for each function to test my snake elementwise like this:

{% highlight javascript %}
snake.slice(snake.length-score,snake.length-1).forEach(function(e,i,a){
if(
(e[0]==snake[snake.length-1][0])&&(e[1]==snake[snake.length-1][1])
){
end_game();
}
})
{% endhighlight %}

I haven’t tested this for the larger lengths of the snake but performance at the levels I attained weren’t impacted.

Every programming project is a wonderful exercise in problem solving– I’m glad I was able to relax and work out the kinks of this!
