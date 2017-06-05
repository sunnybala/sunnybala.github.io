---
layout: post
title: Detecting Fake Videos with Python
comments: true
---

**Someone on the internet uploaded a video where he slapped himself for 24 hours. Did he actually do it?**

I was scrolling through YouTube the other day and saw a video that was going viral. In it, a guy claimed he was going to slap himself in the face for 24 hours. The video was a full 24 hours. I skipped around through the video and, sure enough, it was just him slapping himself. Lots of the comments claimed that the video was fake. I thought so too, but I wanted to know for sure. 

<a href="https://www.youtube.com/watch?v=N2VwIfi6LoY"> <img src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/slapping-preview.png" /></a>

## The Plan

**Write a program to detect if there are any loops in the video.** I hadn't done any video processing with Python before so it seemed like it could be a neat. 

---

Watching a video is like watching a really fast flipbook of pictures. Conveniently, that's also what it looks like when reading the data of the video using python. Each "picture" that we see is one frame of the video. When this particular video plays, it's playing in 30 frames per second. 

In data, each frame is a giant array. This array tells us the color of each pixel at every location (using RGB).  We want to see if any frames appear more than once in the video -- one way to do that would be just counting the number of times we see each frame. 

I did that counting using two dictionaries. One keeps track of any frames I've seen already. The other keeps track of groups of frames that are all exactly the same. As I go through the frames one by one, I first check if I've seen the image before. If I haven't, I'll add it to my dictionary of frames I've seen (seen_frames below). If I have seen that frame before, I'll add it to the other dictionary (dup_frames) in a list along with all the other frames that are exactly like it.

(More info on the code: typically, when doing a counter like this or using the collections.counter object, you'll be hashing the values that you're counting under the hood. Unfortunately, the video frame arrays are not hashable -- to get around this, I converted the arrays to strings before hashing and it worked like a charm. )

Code below:

{% highlight python %}

def find_duplicates():
	# load in the video file
	filename = 'video.mp4'
	vid = imageio.get_reader(filename,  'ffmpeg')
	all_frames = vid.get_length()

	# we'll store the info on repeated frames here
	seen_frames = {}
	dup_frames = {}

	for x in range(all_frames):
		# get frame x
		frame = vid.get_data(x)

		# hash our frame
		hashed = hash(frame.tostring())
		
		if seen_frames.get( hashed, None):
			# if we've seen this frame before, add it to the 
			# list of frames that all have the same 
			# hashed value in dup_frames
			dup_frames[hashed].append(x)
		else:
			# if it's the first time seeing a frame,
			# put it in seen_frames
			seen_frames[hashed] = x
			dup_frames[hashed] = [x]

	# return a list of lists of duplicate frames
	return [dup_frames[x] for x in dup_frames if len(dup_frames[x]) > 1]

{% endhighlight %}

Running this code took about an hour on my macbook pro. Let's take a look at the results:

<div style="text-align: center;"><img src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/matches.png" height="450px"/></div>

Okay, neat. So according to this, frame 5928 is the same as frame 2048454, frame 5936 is the same as 2048462, and so on. Let's visually confirm.

<div style="text-align: center;"><img src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/frame-match.png" /></div>

Perfect. So ** the video is definitely fake. ** However, the number of matches seems super sus -- it's really low. Is it really the case that there's only ~ 25 identical frames? That's hardly 1 second out of the full 24 hour video. Let's take a closer look!

### The Plot Thickens

The program worked the way it was supposed to -- it identified identical frames and let me know that the video was looping. Let's check what the frames look like 20 seconds after the two images above (frame 5936 + 600 and frame 2048462 + 600). 

<div style="text-align: center;"><img src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/identical-prediff.png" /></div>

Wait ... these look identical! And yet they didn't flag as a match?! We can subtract these two frames from each other to figure out what's different. The subtraction works pixel by pixel on the red/green/blue values.

<div style="text-align: center;"><img src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/identical.png" /></div>

Great, we've made some cool glitch art. What actually appears to be happening is that these are just a bunch of artifacts in the frame coming from the fact that the video is compressed. Because of the lossy compression, two frames that originally were identical might get a little distored by this noise and will no longer be numerically the same (though they are visually).


#### ? ? ?


