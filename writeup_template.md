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

## Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps. Firstly, I converted the images to grayscale. Then I apply Gaussian smoothing to suppress noise and spurious gradients. Next I use Canny edge detector on this gray scale image and I can get a binary image with white pixels tracing out the detected edges and black everywhere else. After I get the edges, I can create a masked edges image and define a four sided polygon to mask. Then I run Hough on edge detected image. In order to draw two straight lines, I redefined the draw_lines() function to extrapolate the line segments I have already detected to map out the full extent of the lane lines. Thus I get line_image. Finally, I combine the line_image and the original image and the output is exactly what we need.

Specifically, my new draw_lines() functions consists 4 steps.
First, I separate all the line segments by their slopes. Then for the lines on both sides, I calculate their slope and remove the outliers. Then I collect x1s, y1s, x2s, y2s and the slopes and get average position and average slope of left lane and right lane. Then I calculate the mid point position (x0,y0) on each side thus I can get the line function with slope of left_slope/right_slope and passing through point (x0,y0). I define the y coordinate of the lane lines I want to be 0.6*imshape[0] and they starts from the bottom, So the lane lines looks better.


### 2. Identify potential shortcomings with your current pipeline

One shortcoming is the lane lines are easily affected by sun lights. This is what happens in challenge video and the lane edges are not very clear. Right now, the output of challenge video is not satisfying. I will convert images from RGB to HSL color space and try to figure out a way to make the pipeline more robust.


### 3. Suggest possible improvements to your pipeline

One thing is that the region of interest mask is assumed from the center of the image. What if there is a steep slope or a sharp turn ahead. We need to adjust the region of interest for these occasions.

If the car enters some area with no lanes (for instance, the lanes are covered by heavy snow), current algorithm will not work. Lane estimation might be our next step in the algorithm improvement.

