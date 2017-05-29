---
layout: post
title: Detecting Fake Videos with Python
comments: true
---

**Someone on the internet uploaded a video where he slapped himself for 24 hours. Did he actually do it?**

I was scrolling through YouTube the other day and saw a video that was going viral. In it, a guy claimed he was going to slap himself in the face for 24 hours. The video was a full 24 hours. I skipped around through the video and, sure enough, it was just him slapping himself. Lots of the comments claimed that the video was fake. I thought so too, but I wanted to know for sure. 

<a href="https://www.youtube.com/watch?v=N2VwIfi6LoY"> <div style="text-align: center;"><img src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/slapping-preview.png" /></div></a>

## The Plan

Write a program to detect if there are any loops in the video. I hadn't done any video processing with Python before so it seemed like it could be a neat. 

---

### Diving In

<div style="text-align: center;"><img src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/frame-match.png" /></div>

### Deeper Dive

The program worked the way it was supposed to -- it identified identical frames and let me know that the video was looping. However, I was suspicious that it wasn't getting all the frames that matched.

<div style="text-align: center;"><img src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/identical-prediff.png" /></div>

We can subtract these two frames from each other to figure out what's different. The subtraction works pixel by pixel.

<div style="text-align: center;"><img src="https://raw.githubusercontent.com/sunnybala/sunnybala.github.io/master/assets/identical.png" /></div>

#### ? ? ?


