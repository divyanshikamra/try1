---
layout: post
title:  "Code Explore: Computer Vision"
date:   2020-05-20 13:46:52
comments: true
categories: jekyll update
---

<h1>Invisible Cloak using Open CV and Python</h1>
<p align="center">
  <img src="{{ site.author.blog2 | prepend: site.baseurl }}" alt="author blog2" class="square" width="900" height="400" >
</p>
<blockquote><p>Have you ever seen Harry Potter’s Invisible Cloak; Was it wonderful? Have you ever wanted to wear that cloak? If Yes!! then in this post, we will build the same cloak which Harry Potter uses to become invisible. Yes, we are not building it in a real way but it is all about graphics trickery.</p>
</blockquote>

<p><strong>
In this post, we will learn how to create our own ‘Invisibility Cloak’ using simple computer vision techniques in OpenCV. Here we have written this code in Python because it provides exhaustive and sufficient library to build this program.</strong></p>
<p><strong>
Here, we will create this magical experience using an image processing technique called Color detection and segmentation. In order to run this code, you need an mp4 video named “video.mp4“. You must have a cloth of same color and no other color should be visible into that cloth. We are taking the red cloth. If you are taking some other cloth, the code will remain the same but with minute changes. </strong></p>

<h4>Why Red? Green is my favorite?</h4>
<p>
Sure, we could have used green, isn’t Red the magician’s color? Jokes aside, colors like green or blue will also work fine with a little bit of changes in code.
This technique is opposite to the Green Screening. In green screening, we remove background but here we will remove the foreground frame. So let’s start our code.</p>

<h2>Algorithim</h2>

<ol>
  <li>Capture and store the background frame [ This will be done for some seconds ]</li>
  <li>  Detect the red colored cloth using color detection and segmentation algorithm.</li>
  <li>Segment out the red colored cloth by generating a mask. [ used in code ]</li>
  <li>Generate the final augmented output to create a magical effect. [ video.mp4 ] </li>
  <li>Aliquam tincidunt mauris eu risus.</li>
</ol>


<h2>Code</h2>

```ruby
import cv2 
import numpy as np 
import time 

print(cv2.__version__) 
 
capture_video = cv2.VideoCapture("video.mp4") 
	
time.sleep(1) 
count = 0
background = 0

for i in range(60): 
	return_val, background = capture_video.read() 
	if return_val == False : 
		continue

background = np.flip(background, axis = 1) 

while (capture_video.isOpened()): 
	return_val, img = capture_video.read() 
	if not return_val : 
		break
	count = count + 1
	img = np.flip(img, axis = 1) 

	
	hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV) 

	
	lower_red = np.array([100, 40, 40])	 
	upper_red = np.array([100, 255, 255]) 
	mask1 = cv2.inRange(hsv, lower_red, upper_red) 

	lower_red = np.array([155, 40, 40]) 
	upper_red = np.array([180, 255, 255]) 
	mask2 = cv2.inRange(hsv, lower_red, upper_red) 
	
	mask1 = mask1 + mask2 

	mask1 = cv2.morphologyEx(mask1, cv2.MORPH_OPEN, np.ones((3, 3), 
										np.uint8), iterations = 2) 
	mask1 = cv2.dilate(mask1, np.ones((3, 3), np.uint8), iterations = 1) 
	mask2 = cv2.bitwise_not(mask1) 

	# Generating the final output 
	res1 = cv2.bitwise_and(background, background, mask = mask1) 
	res2 = cv2.bitwise_and(img, img, mask = mask2) 
	final_output = cv2.addWeighted(res1, 1, res2, 1, 0) 

	cv2.imshow("INVISIBLE MAN", final_output) 
	k = cv2.waitKey(10) 
	if k == 27: 
		break

```

Check out [GeeksforGeeks][jekyll] for more info.

[jekyll]:      https://www.geeksforgeeks.org/invisible-cloak-using-opencv-python-project/

