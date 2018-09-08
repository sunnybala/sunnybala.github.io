
**The etch-a-sketch is one of the most frustrating drawing toys from childhood. Can we combine some code and motors to make this toy produce nice pictures?** <a href="#bingo"> Yes! <a/> 

<style>
.center {
    display: block;
    margin-left: auto;
    margin-right: auto;
    width: 50%;
}

</style>

## Background
The Etch-A-Sketch was released by The Ohio Art Company in 1960. Ever since then, it's been a staple toy in homes, daycares, waiting rooms and schools -- over 100 million have been sold worldwide. It was even a big enough deal to be a character in Toy Story 1 and 2!

<img src="https://vignette.wikia.nocookie.net/pixar/images/b/bc/DetectiveScene_TS2Etchb4.jpg" style="height:250px;" class="center"/> 

 The popularity of the device is sort of interesting because it has lots of annoying problems:
- knobs only move the cursor up/down/left/right. Diagonals and curves require a steady precision that most children 6+ simply don't have. 
- you can't draw two objects that are not connected.
- No ability to undo small mistakes. If you want to undo something you'll have to redo your whole masterpiece.

## The Plan

**Write a program that uses some motors to draw nice pictures on an etch-a-sketch. ** For the reasons above, recreating pictures by hand is tedious and extremely difficult. My hope was to get to a point where my program could draw things better than I could. I've always wanted to learn more about how to use hardware with python so I was pretty excited to start this one. 

## Hardware Setup

Parts List Recipe:
* Raspberry Pi
* Etch a Sketch
* 1x Piece of Wood
- 2x Stepper Motor
- 2x Controller
- 2x Shaft Coupler
- 8x Jumper Cables (FF)

### 1. Hooking Up Motors to Raspberry Pi

Since the Raspberry Pi is just a tiny computer, we're going to be able to use all the python libraries we know and love. Additionally, it has a set of GPIO pins to use. I connect a few of these pins to each of two controllers + motors. By changing the voltage on these pins, I can make motors to rotate. You can read more about how this works here.

### 2. Connecting Motors to Knobs

With some force, the knobs can be removed from the etch-a-sketch. This reveals a shaft on the device. The motor also rotates a shaft, so to connect them together I used a chinese-finger-trap-like device called a coupler. It's somewhat flexible, with two small sets of screws on either side that allow it to be tightened around each of the shafts. 

picture

### 3. Stabilizing 

There's quite a bit of resistance on the etch a sketch knobs. Without doing anything, if we set the motor to rotate, the knob will actually stay still and the motor itself will rotate! 

I went to a local hardware store with the dimensions of the build and they were able to cut me a block of wood that I could screw the motors into to keep them stable. I glued the wood on the bottom to the etch-a-sketch body. This can be tricky if the wood is not appropriately sized. With a screw too loose and the motor will wiggle around when it's trying to rotate. If it's too tight, the motor will be squeezed against the coupler and not be able to rotate. 

## Software Setup

The descriptions below will be mostly high level but all the code for this project is available on GitHub if you'd like to dive in at a deeper level.  

### 1. Controlling Motors

The raspberry pi python library **rpi** lets us control the motor via the pins. I created a class to represent the motor. I'll be using this in the code is in the following way:

{% highlight python %}

horizontalMotor = StepperMotor(horizontal_pins)
verticalMotor = StepperMotor(vertical_pins)

horizontalMotor.rotate(40)  # move cursor 40 units right
horizontalMotor.rotate(-40) # move cursor 40 units left
verticalMotor.rotate(40)  # move cursor 40 units up
verticalMotor.rotate(-40) # move cursor 40 units down
{% endhighlight %}

This is the core functionality we need. Now, as long as we know a path that we want to travel along, we can come up with a way to convert it to a series of instructions like above. For curves and diagonal lines, we can get the behavior we're looking for by alternating instructions to the horizontal and vertical motors. For example, the code below will create a nice smooth diagonal line for us. 

{% highlight python %}

for step in range(40):
	horizontalMotor.rotate(1)
	verticalMotor.rotate(1)

{% endhighlight %}

### 2. Simplifying a Picture 

I'm not going to kid myself about being able to create intricately detailed pictures using this etch-a-sketch. However, I do want a process that will take the original pictures and strip them down to the main details. We can do this using standard edge detection techniques like the ones built in to *canny*. 

The canny algorithm takes a tuning parameter sigma. By playing around with this, we can get more/less detail in the edges that are picked up. Below I present a few examples of images run through this edge detection process. 

The results here play a big role in what I can / can't draw. For example, landscapes, buildings, patterns and animals look a whole lot better than people. There's a few more examples of this in result section below. 

### 3. Picture to Instructions

This is a tricky step. A picture is just a collection of pixels. I'll be finding a path here by converting my picture into a network/graph. Every pixel will become a node and every node will be connected to other nodes (pixels) within a 1 pixel radius. Visualization below.

Some pictures will have many graphs and not all of them connect. For example, the grass/dirt in front of the tree below. Since the etch-a-sketch can't make jumps, I take the largest available graph that I can. This gives me a nice,  cleaned up image like the one on the right.

Now that I have a single graph of points, I need to come up with what instructions to tell the motors. My strategy follows a standard depth first search approach. I'll start at the leftmost point in the graph. From there, I'll traverse nearby nodes, arbitrarily picking neighbors. If there are no neighbors at a given node, I backtrack to the last juncture and try a different route. My goal is to hit every node. 

I'm totally okay with retracing since the etch-a-sketch lines are pretty thin on first pass anyway. Using the networkx library, I can actually generate these paths quite easily! The output of this process gives me a path (below, left). Finally, I take the path and convert it into a series of steps that I can pass in to my motors. These steps I produce are the result of subtracting locations in the path with the prior location (below, right). 

## Results 

And...viola! The path above is exactly what we needed. The motors are a bit slow, so here's a time lapse of the etch a sketch drawing an elephant. This represents about 20 minutes of time in real life. 


### Elephant

### Low Library at Columbia 

### Great Wave

## Odds and Ends

If you attempt to do something similar, keep an eye out for nasty mechanical bugs. When motors change directions, there can be a bit of "give" resulting in a deadzone. This is highly dependent on your own build. This took me FOREVER to diagnose. To fix it, I drew a series of grids to quantify exactly how many rotation units this deadzone was. I then added logic to my motor class to add extra rotation units when the direction was changing. 