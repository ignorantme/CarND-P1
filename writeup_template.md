# **Finding Lane Lines on the Road** 





## Work Description
---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./source/houghLines.png "Grayscale"
[image2]: ./source/solidWhiteRight.jpg
[image3]: ./source/solidWhiteRight_output.jpg
[image4]: ./source/challenge3.png
[image5]: ./source/challenge5.png
[image6]: ./source/challenge.jpg

---

### Reflection

### 1. Describe the pipeline. 
The pipeline consisted of 6 steps.

First, the image is converted to grayscale.

Second, Gassian blur is applied to the image.

Third, I use Canny Transform to find the edges on the image.

Forth, give the region of interest with a polygon [(100, 540),(420, 320),(550, 320), (900, 540)].

Fifth, using Hough Transform to find lines with in these limited area. The parameters can be find in the code. Tis step took up many times of fine tuning to get good enough.

![image1]

Sixth, the most important step, find the lane from these line segments (see find_lane() function). 

(1). using the slopes, intercepts and the positions, these line segments are separate into to two categories, left lines and right lines. To make sure only the useful lines are kept, I ignore the lines with infinite slopes and slopes with small absolute values (< 0.5). 

(2). then further exclude the outlier lines whos slope value is outside the range of 1.5 standard deviation from the mean value.

(3). then I tried two ways to get lane from the lines left, I first is used the start and end points of these line segments to fit two lines. The result was good enough on test images, but when applied this method on videos, I found the lines were not stable, and usually not accurate enough. Then I think the line segments should have different weights because longer lines contributes more to the final results. I use the square of length as the weight of each line to calculate the average slope and intercept. 
When porocessing videos, I use the information of previous 4 frames and the current frame to get average slopes and intercepts as the final result.

![image3]

It looks pretiy good.

For the challenge video, first the resolution is different, I first resize it.
The the shadows and high light on the road cause several problems: recognized several fake lines from the shadows, and the yellow lane is difficult to separate from the ground. 
![image4]![image5]
To solve these problem, I first filter out the invalid line segments by there slopes. Then adjust the threshold of Canny Transform. Check out the result of challenge.mp4 in the output folder.

![image6]

### 2. Identify potential shortcomings with your current pipeline


The region of interest is a fixed polygon, for the test images and videos it works fine, but we can assume the car always drving on straight road.

The parameters of Canny Transform and Hough Transform are fixed, they have problem when the road has noises. 

We assume the lanes on the road are always visible, what if the lane breaks for a distance, what if drive at night?

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to dynamiclly change the region of interest on curved road and straight road.

Also dynamically change the parameters of Canny and Hough methods according to the road condition, fixed parameters can not handle all situations.


