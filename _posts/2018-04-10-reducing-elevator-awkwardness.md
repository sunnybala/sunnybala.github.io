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

## Approach: A Step in the Right Direction

What makes an elevator uncomfortable? One natural idea is how close people are to each other. For example, the situation on the left seems far more awkward than the one on the right.

<img src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/awk_ele_lr2.PNG" class="center"/>

Based on this, we might want to try just using the distance formula as our proxy for awkwardness. For example, my discomfort in the elevator could be a function of my distance to each person in the elevator, like: 

<img src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/awk_ele_form1.PNG" class="center"/>

This sort of makes sense. Under this scenario, this simulation could work like the pseudocode below. 
* For every person in the elevator
	* Calculate the comfort formula. Record it. 
	* Figure out a direction to for the person to step in.
	* Move the person to the new spot.
* Repeat this for every person in order over and over again

The hope is that after many many times of each person choosing a direction and stepping in it, they'll arrive at stable situations. 

Choosing this drection was a bit tricky. There's lots of different options. The quantitative way might suggest just taking the derivative of the comfort formula from earlier with respect to x and y. This will tell us about how many units of change we can expect in the comfort level for a 1 unit step in the x or y direction. Not bad, though the derivative isn't the prettiest thing in the world.

formula

However, maybe this isn't so realistic. Consider the example below. If you were the blue circle and could only step left or right, which way would you step? If everyone is in a line, the derivative ends up being really simple. For every 1 unit you move away from a person, you'll earn 1 unit of comfort. In this example, if you move right, you'll get 1 unit from person 1, 1 unit from person 2, and lose 1 unit because you're getting closer to person 3. On the other hand, if you move left, you'll lose 1 unit each from person 1 and person 2 and gain one unit because of person 3. You can imagine that each person is basically pushing you in some direction from different sides and that you'll move in whichever way more people are pushing. 

The derivative approach would tell you to step right, though most people might look at this and decide left makes more sense. This tells us there's something missing from this formula approach. In reality, maybe you don't care about getting an extra 2 inches away from someone on the other side of the elevator. Instead, you might **really** care about getting an extra 2 inches away from someone who you're extremely close to. 

In that case, we might be able to do this in an even simpler way. If all you care about is where you are relative to the nearest person, we can use a simpler rule for determining the direction to step in. Simply identify the nearest person, and take a small step in the exact opposite direction! We'll still do the procedure for person by person and repeat many times to converge to a stable arrangement.

## Code
The code for this project is available on GitHub.

I hadn't made animations before but after playing around with tutorials about matplotlib, I eventually got up and running. The good news is that I didn't need anything fancy -- just a box to represent the elevator and a bunch of moving circles inside to represent people. 

The general structure of the code looks like this in pseudocode.

{% highlight python %}
def simulate(n_people = 10):
	# number of people in elevator
	# set up graphics (scatterplot with big circles) and boundaries
	# Create people data
	people = pd.DataFrame()

	# Initialize the people in random positions in a 1x1 square
	people['position'] = np.random.uniform(-.9,.9,(n_people, 2))

	def getStep(person_i):
		# identify nearest person to person_i
		step = -(nearest_person_location - person_i_location)
		return step
		
	def update(frame_number):
		# this gets called every frame of the animation automatically
		
		# for every person figure out steps
	    for idx in range(n_people):
			# figure out the step
		    steps[idx] = getStep(idx,METHOD)
		
		# for every person update positions
	    for idx in range(n_people):
		    people['position'][idx] += steps[idx]

	# Construct the animation, using the update function as the animation
	# director.
	animation = FuncAnimation(fig, update, interval=10)
	plt.show()
{% endhighlight %}

On the surface this looks not so bad, especially if you gloss over the details on how exactly to make matplotlib draw things the way you want to. In the real code I make a few modifications. 

Firstly, I add a parameter to try different ways to each person's next step. For example, instead of the "step away from nearest person" approach, I did try the derivative as well. 

Secondly, there's a lot of things that can go wrong. One example happens when multiple circles are in a perfect line, like the example of when the derivative method fails. Stepping away from your neighbor is great, but if you're in a perfect horizontal line with them you might actually be better of taking one step down or up rather than left or right. To avoid edge cases like this, when adjusting positions I actually add in a *small* amount of random noise to both the x and y coordinates. This can help make sure we don't have situations like below.

Finally, in order to get these to converge. 


## Results

