# **Finding Lane Lines on the Road** 

**Finding Lane Lines on the Road**
Detect the lane lines in road image and video stream using Python

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road in the given video
* Annotate the input video lane lines and make an annotated output video
* The left and right lane lines are accurately annotated by solid lines 

### Reflection
OpenCV module is used for image processing. My pipeline consisted of 5 steps. 

* Convert the images to grayscale
* Perform Gaussian smoothing and apply Canny edge detection.
* Select region of interest and the rest of the image is set to black.
* Apply Hough Transform to detect lane lines
* Annotate the lane lines on the original image

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by

* Calculated slope and center of each line. 
* Then based on the slope, sort it into right or left lane line
* Calculate the average slope and the center of right and left lane
* Then using the Y coordinates, based on Region of Interest, figure out the X coordinates using the avg slope and center point of lane lines [equation used: (y-y') = M (x-x')]

Challenges I faced

* The lane lines were in both yellow and white color
* The lines can be of both curve and straight format
* There is also white car switching the lane

### 2. Potential shortcomings

* Pipeline slope conditions used for detecting right and left lanes is not robust enough for the steep curves in the road
* Processing is quite slow on the video compared to the images

### 3. Possible improvements

* Better average mechanism for finding the slope and center of lanes
* Make all the lane line colors to white before doing canny and hough transform
* Predictable mechanism to detect the curves in the lanes
* Overall the pipeline is very successful to annotate the lane lines in both image and video
