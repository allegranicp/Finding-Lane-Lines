**Finding Lane Lines on the Road**

Overview
---
The goals / steps of this project are the following:

* Create a pipeline that finds lane lines on the road using Python and OpenCV. OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.

[//]: # (Image References)
[image0]: ./images/original_img.png "Original Image"
[image1]: ./images/blur_output.png "Gaussian Blur"
[image2]: ./images/canny_edge_detection_output.png "Canny Edge Detection"
[image3]: ./images/hough_lines_output.png "Hough Lines"
[image4]: ./images/result_1.png "Output with Hough"
[image5]: ./images/result_only.png "Output Only"
[video1]: ./solidWhiteRight.mp4 "Video"
[video2]: ./solidYellowLeft.mp4 "Video2"

![alt text][image5]

## [Rubric](https://review.udacity.com/#!/rubrics/1967/view) Points
---

The code for this project is contained in the IPython notebook: "P1.ipynb" 

### Writeup / README

![alt text][image0]

### Pipeline (single images)

#### 1. Conversion and Smoothing

First, I converted the images to grayscale, then I applied gaussian smoothing to reduce the noise in the grayscale image. This will be helpful when performing the Canny edge detection.

![alt text][image1]

#### 2. Canny Edge Detection and Region of Interest

I utilized Canny edge detection to identify the boundaries of the objects and surface lines in the grayscale image, then I defined a region that is most likely to include only the lane lines and masked the rest of the boundaries with black called a region of interest.

![alt text][image2]

#### 3. Hough Lines

Lastly, from the edges or points identified by the Canny algorithm for that specific region of interest, I used Hough transformations to find lines from those point by identifying intersections of curves in Hough space. 
As a result, these lines are drawn over the left and right lanes on the original image as shown below.

![alt text][image3]

#### 4. Result

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by first computing the slope of each of the Hough lines to determine whether the line is on the left or the right. If the slope is negative the line is a right line and if not, the line is a left line. In order to account for potentially noisy lines, I set the threshold for slopes between -0.5 and 0.5 instead of 0.

I calculated the average slope per side, then I calculated the average x and y coordinates per side which gives me the average position of each line. Since a continuous line is characterized by the equation y = mx + b, I used the now known components: y, x and m to calculate the average y-intercept.

Finally, I extend these continuous lines to the top and bottom of the region of interest along the x. As a result, a single continuous line is drawn over the left and right lanes independently on the original image as shown below.

![alt text][image4]

---

### Pipeline (videos)

* Here's a link to my first video [video1](./solidWhiteRight.mp4)
* Here's a link to my second video [video2](./solidYellowLeft.mp4)

---

### Discussion

#### Potential shortcomings

One potential shortcoming would be what would happen when a curve or hill is encountered. The algorithm is accurate on straight lines but will result in extremely inaccurate detection once the vehicle turns a corner or goes around the curve. 

Another shortcoming could be when another vehicle comes into the lane. The current images/frames being used assumes the vehicle is driving in a lane by itself, but as we know human driving is extremely unpredictable. Another vehicle could obstruct the view on the lane line which may cause a miss detection especially if the other vehicle merges into the lane on a curve.

A third shortcoming could be the algorithm sometimes does not find any components of the right or left line so when taking the average to an empty list the result will be “nan” (not a number). This could cause gaps for other sensors subscribing to this data that makes it next to impossible to know with certainty what objects are in the frame in real-time. 


#### Possible Improvements
A possible improvement would be to use a more encompassing model for classifying the lines to account for the changes in the lane lines. The algorithm assumes the road will always be straight which is not realistic, so it essentially uses a straight-line heuristic. If a quadratic heuristic was used it could better account for the uncertainty of the lanes.   

Another potential improvement could be to implement some form of deep learning so that based on what the algorithm has seen or learned thus far; it can predict where the lane line may be next. It will also help with learning some of the uncertainty in human driving to account for any kind of obstructions.



