# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteCurve.jpg "Result 1"
[image2]: ./test_images_output/solidWhiteRight.jpg "Result 2"
[image3]: ./test_images_output/solidYellowCurve.jpg "Result 3"
[image4]: ./test_images_output/solidYellowCurve2.jpg "Result 4"
[image5]: ./test_images_output/solidYellowLeft.jpg "Result 5"
[image6]: ./test_images_output/whiteCarLaneSwitch.jpg "Result 6"

---

### Reflection

### 1. Pipeline Description

My pipeline consisted of 5 steps. They are

**1. Grayscale conversion**

**2. Gaussian Blur**

**3. Canny Edge Detection**

**4. Region of interest Extraction**

**5. Hough Transform**

**6. Extrapolation of Lane Lines**

Step 1 or Grayscale conversion stage of the pipeline deals with converting the image to black and white. This eliminates the color information from the image. The next
step deals with smoothing of the image using a Gaussian Blur. In my project, I selected a kernel_size of 5. This eliminates some noise from the black and white image.
In step 3 we apply canny edge detection algorithm to the result from step 2. 

At this point, the lessons recommended that we apply the hough transform. This is what I tried initially. However, this results in detection of several unnecessary points that are 
not part of the lanes. To overcome this, I defined a quadrilateral region of interest and applied it to the output of the canny edge detection algorithm in step 4. The vertices of this 
quadrilateral are 

**(0,540),(400,335),(590,335),(960,540)**

In step 5, I applied the hough transform on this masked image. This reduced the number of lines that I need to look at to come up with my lane lines.

Step 6 deals with drawing a single line on the left and right lanes. For every line defined by points (x1,y1) and (x2,y2), I calculated the slope. 
If the slope was positive, I considered them to be part of left lane. If the slope negative, I considered them to be part of the right lane. However, I noticed a few lines had infinite slope.
These were considered noise and eliminated from consideration. Also, some of the data points indicated that a few points on the right lane had positive slope and a few points on the left lane had negative slope.
In order to eliminate these bad points, I needed to define another condition. I simply looked at the x1 and x2 data. If both were above 480, I considered them to be on the right lane.
If both were below 480, I considered them to be part of the left lane.

I applied this pipeline to the test images and obtained the following results.

![alt text][image1]

![alt text][image2]

![alt text][image3]

![alt text][image4]

![alt text][image5]

![alt text][image6]


### 2. Potential shortcomings with your current pipeline
One big shortcoming of the above method is using 480 along the x-axis to split the points between left and right lanes or to ignore them.
I don't think this is a good idea if the curves get aggressive. I encountered some error while trying to check this on the challenge video.


### 3. Suggest possible improvements to your pipeline

Rather than use a hard-limit of 480, we may want to use distance to conclude if the point is on the left or right. Every point is compared to all points on the left and right lane. 
If the lowest distance is to points on the left lane, the point is on the left lane. Else it is on the right lane.

### 4. Acknowledgements
I found the discussions on the forums very handy. Very many of my class mates had similar questions and answers in those forums helped me understand the material better. They also provided pointers on where I am going wrong and how I could be fixing it.

I also used http://nullege.com/codes/search/matplotlib.pyplot.imsave to save images

I used https://justgagan.wordpress.com/2010/09/22/python-create-path-or-directories-if-not-exist/ to create directories and save images and videos when they did not exist.
