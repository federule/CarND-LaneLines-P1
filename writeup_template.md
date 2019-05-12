# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I applied the gaussian smoothing with a kernel size of 7, which I found to give me a satysfying result. Then, I'm applying the canny edge detection and i'm defining my region of interest with a 4-side polygon. Lastly I apply the hough transform with my modified draw_lines() function to draw and detect the lane.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by averaging two lines, one with positive slope and the other one with negative slope. To avoid division by zero, I'm currently avoiding any line with slope equal to infinity (vertical lines), and avoiding to draw any line defined by my averaging loop with a slope of 0.  

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the horizon of my image is shorter or the camera is not centered in the same way. My region of interest is fixed and cannot be changed, and I'm not able to detect the left lane in the optional challenge because it's outside. Moreover, the horizon is smaller in that picture, and my interpolated line is too long for this case. Moreover, if the hood of the car is present in the image it should be removed.

My averaging technique is also not really helping, because if I detect a very small line with a completed different slope my average slope would received an effect, even it is not really relevant. 

Having fix parameters for the hough transform also could create issues, because the image may change drastically from frame to frame, so having more freedom could be helpful.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use the edge detection to identify the region of interest, maybe using a horizontal line detected on the top and bottom to define the horizon, the car limit and the best region of interest. Morevorer while detecting a curve line interpolation is probably not the best way, and the region should be modified for this reason too.
To solve the detection of the tree in the optional challenge applying some kind of color detection while using the edge detection may improve reliability of the system (lanes are just yellow and white).

To improve the interpolation some kind of least square technique could be used, maybe splitting the region of interest in two areas and applying the interpolation in left and right region separately. It should overcome the issue of having big changes due to small lines and it should also overcome the issue of vertical lines.

Moreover, having variable parameters, maybe related to the number of edges detected, to the number of lines derived from the hough lines function could improve the stability of the algorithm.

Lastly, using memory from the previous frame (or frames) to detect the next lane could be also very helpful, and it could remove the issue of stray lines that i have in the yellow lane video. Some kind of filtering could be a good starting technique for this.
