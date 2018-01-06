# CarND-Advanced-Lane-Lines
Project 1 Submission for Udacity Self-Driving Car Nanodegree 

This project includes utilizing gray-scaling, canny-edge detection, masking, and Hough-transforming to attempt to detect lane lines on images and videos. The writeup for my project is also included below in the same markdown.

# Writeup

## Current Pipeline Implementation

Currently, for my lane line detection pipeline, I am using these few techniques.
 
 1. Grayscaling of image from color channels to single grayscale channel
 2. Gaussian smoothing of the input image
 3. Canny edge detection to detect edges using contrast
 4. Creating a 4-sided Region of Interest Mask to detect only edges within the region of interest
 5. Hough Transform on the masked edged detected image to identify lines in the image
 6. Converting the various detected lines into straight lines and drawing them in the image
 
### Grayscaling of image

Grayscaling is done using the grayscale helper function to convert the BGR image read by OpenCV into grayscale, and is quiet straightforward.

### Gaussian Smoothing of Image

Gaussian smoothing of the image is done to reduce the effect of noise on the image, we used the helper function gaussian_blur to carry out the gaussian smoothing, and finally we choose a size of 5 for the kernel. (Interesting note, it meant that we could have skipped that part as the canny function does a gaussian smoothing with size of 5 for the kernel by default).

### Canny Edge Detection + Region of Interest Masking

We then do canny edge detection to detect edges within the entire picture, following which created a polygon region of interest to detect only areas which are likely for the lane lines to occur when considering the camera to be in the front of the car. 

### Hough Transform

We then do a Hough transform using a various parameters, such as 

### Converting detected lines into lane lines

Finally, the most important part of the pipeline is the convering of the various lines and converting them into two lane lines on the left and right. The steps to carry these out is to:

1. Seperating the lines into left lane and right lane based on slopes
2. Computing the average of all the ending lines for each side
3. Computing the slope and intercept for each side based on the averages
4. Extrapolating the lines using the slopes and intercepts

One thing I did was to incorporate the idea of storing the historical slopes and intercepts so that we can construct a sliding window aveerage of the slopes and intercepts. Furthermore, we also introduce a check to reject lane lines with slopes or intercepts that change too rapidly as they may be lines that are formed from picking up edges not related to the lane lines. In those cases, we use back the historical values of the slope and intercept, which is an advantage of storing recent values of the slope and intercept.

## Potential Shortcomings + Possible Improvements

From the second video, we can see that the lane line is still not perfect, there are misalignments during the course of the video, and 
can be better improved by either more tuning of the parameters. Another possible method is to keep the historical values of the two points used to plot the lane lines for each side as moving averages as well. 

In the challenge video, at some point in time, I observe that the car passes by a tree which project shadows. This would likely cause many random edges that are not lane lines to be detected due to contrast and would need some method to deal with it. One possible improvement for this would be outlier detection algorithms, but unfortunately I was not able to find a good one to work as of the project submission.
