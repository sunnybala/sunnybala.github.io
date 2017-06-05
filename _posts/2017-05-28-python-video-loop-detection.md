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

### Diving In

Watching a video is like watching a really fast flipbook of pictures. Conveniently, that's also what it looks like when reading the data of the video using python. Each "picture" that we see is one frame of the video. When this particular video plays, it's playing in 30 frames per second. 

In data, each frame is a giant array. This array tells us the color of each pixel at every location (using RGB).  We want to see if any frames appear more than once in the video -- one way to do that would be just counting the number of times we see each frame. 

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

<div style="text-align: center;"><img src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/frame-match.png" /></div>

### Deeper Dive

The program worked the way it was supposed to -- it identified identical frames and let me know that the video was looping. However, I was suspicious that it wasn't getting all the frames that matched.

<div style="text-align: center;"><img src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/identical-prediff.png" /></div>

We can subtract these two frames from each other to figure out what's different. The subtraction works pixel by pixel.

<div style="text-align: center;"><img src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/identical.png" /></div>

#### ? ? ?


