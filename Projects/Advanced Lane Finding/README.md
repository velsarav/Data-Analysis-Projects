## Advanced Lane Finding

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a threshold binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./writeup/output_chess_corners.png "Chessboard corners"
[image2]: ./writeup/output_chess_undistorted.png "Chessboard undistorted"
[image3]: ./writeup/output_road_undistorted.png "Road undistorted"
[image4]: ./writeup/output_color_threshold.png "Gradients"
[image5]: ./writeup/output_combined_threshold.png "Combined"
[image6]: ./writeup/output_road_warped.png "Perspective Transform"
[image7]: ./writeup/output_histogram.png "Histogram"
[image8]: ./writeup/output_lane_plot.png "Lane points"
[image9]: ./writeup/output_final.png "Lane lines"
[video1]: ./output_video.mp4 "Video"
[video2]: ./challenge_output.mp4 "Challenge vide0"

## Rubric Points

The [rubric](https://review.udacity.com/#!/rubrics/571/view) points were individually addressed in the implementation and described in code and [documentation](./Advanced_Lane_Finding.md)

The code for below steps is contained in IPython notebook  [Advanced_Lane_Finding.ipynb](./Advanced_Lane_Finding.ipynb) 

--- 

### Python Libraries
- [OpenCV](https://opencv.org/) - open source computer vision library
- [Matplotbib](https://matplotlib.org/) - 2D plotting library
- [Numpy](http://www.numpy.org/) - package for scientific computing
- [MoviePy](http://zulko.github.io/moviepy/]) - module for video editing
- [glob](https://docs.python.org/2/library/glob.html) - Unix style pathname pattern expansion
- [pickle](https://docs.python.org/3/library/pickle.html) - Python object serialization

### Camera Calibration 
OpenCV functions `cv2.findChessboardCorners` and `cv2.drawChessboardCorners` were used to calculate the correct camera matrix of the chessboard images using the images provided under  "./camera_cal/"

![alt text][image1]

### Pipeline (single images)

#### 1. Distortion correction
To correct the distortion in the image or camera sensors used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.

Using the `cv2.undistort()` function the distortions on the chessboard is removed and it become straight lines 

![alt text][image2]

Applied the distortion correction to the road image 

![alt text][image3]


#### 2. Sobel operator, Magnitude and gradients threshold, HLS and Color threshold

Used Sobel operator to identify pixels where the gradients of an image falls within a a specified threshold range.
The threshold of  x and y, the overall gradient magnitude and the gradient direction used to focus on the pixels that are likely to be part of the lane lines.

- Calculate directional gradient: `abs_sobel_thresh()`.
- Calculate gradient magnitude: `mag_thresh()`.
- Calculate gradient direction: `dir_thresh()`.


##### 2.1 HLS and HSV

[HLS](https://en.wikipedia.org/wiki/HSL_and_HSV) ( Hue, Lightness and Saturation) space helps us to detect lane lines of different colors and under different lighting  condition

OpenCV provides function for the following color space
- HLS `hls = cv2.cvtColor(im, cv2.COLOR_RGB2HLS)`
- HSV `hsv = cv2.cvtColor(img, cv2.COLOR_RGB2HSV)` 
- LAB `Lab = cv2.cvtColor(image, cv2.COLOR_RGB2Lab)` 
- LUV `luv = cv2.cvtColor(image, cv2.COLOR_RGB2LUV)`
- RGB `rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)`

The above functions helps to convert one color space to another.

- Calculate color threshold: `color_threshold()`.

![alt text][image4]

- Combine the both gradient and color threshold values to get better result: `combined_threshold()`

![alt text][image5]

#### 3. Perspective Transform

Assume the road is a flat plane. This isn't strictly true, but it can serve as an approximation for this project. Using this we can identify four source points for your perspective transform ("birds-eye view")

The `perspective_transform()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  Hardcode the source and destination points in the following manner:

```python
src = np.float32(
    [[(img_size[0] / 2) - 55, img_size[1] / 2 + 100],
    [((img_size[0] / 6) - 10), img_size[1]],
    [(img_size[0] * 5 / 6) + 60, img_size[1]],
    [(img_size[0] / 2 + 55), img_size[1] / 2 + 100]])
dst = np.float32(
    [[(img_size[0] / 4), 0],
    [(img_size[0] / 4), img_size[1]],
    [(img_size[0] * 3 / 4), img_size[1]],
    [(img_size[0] * 3 / 4), 0]])
```

The warped image ![alt text][image6]

#### 4. Identify lane-line pixels and fit their positions with a polynomial

After applying calibration, thresholding, and a perspective transform to a road image, still need to decide explicitly which pixels are part of the lines and which belong to the left line and which belong to the right line.

Plotting a histogram of where the binary activations occur across the image is one potential solution for this.

![alt text][image7]

With this histogram we are adding up the pixel values along each column in the image. In our threshold binary image, pixels are either 0 or 1, so the two most prominent peaks in this histogram will be good indicators of the x-position of the base of the lane lines. We can use that as a starting point for where to search for the lines.

Use sliding windows moving upward in the image (further along the road) to determine where the lane lines go. 

In the next frame of video you don't need to do a blind search again, but instead you can just search in a margin around the previous line position, like in the image.

#### 5. Radius of curvature of the lane

Assume the position of the vehicle with respect to center. We have threshold image with estimated pixels belong to left and right lanes and fit a polynomial to those pixel position. Next we will calculate the  
[Radius of curvature] (https://www.intmath.com/applications-differentiation/8-radius-curvature.php) for the fit.

Function `lane_curvature()` calculates the left curvature , right curvature and center

![alt text][image8]

#### 6. Draw on lane

Function `draw_over_lane()` converted the pixels to meters. It also warped back onto the original image and plotted to identify the lane boundaries. Here is an example of result on test images.

![alt text][image9]

---

### Pipeline (video)

Function `process_image()` provides the image processing pipeline that was established to find the lane lines in images successfully processes the video. 

Here's a [Project video result](./output_video.mp4)


---

### Discussion

* Birds-eye view of the lane to process the image was new to learn.
* Getting the radius of curvature using sliding window mechanism was quite interesting.
* Adding the class based method to get the prior values in sliding window as challenging
* The road color change and the steep curves in challenge video makes the pipeline to fail most of the time.
* Need more technique to smooth the curve and improve accuracy.
* Sliding window is not suffice for hypothetical cases such as road at night or road segment with significant slope will fail in this pipeline. Adding better outliner rejection and low-pass filter will help further.

### Reference
- [Tracking](https://github.com/miguelangel/sdc--advanced-lane-finding)
- [Source and Destination points](https://github.com/dkarunakaran/carnd-advanced-lane-lines-p4)
- [Color Spaces in OpenCV](https://www.learnopencv.com/color-spaces-in-opencv-cpp-python/)
- [Match space](https://github.com/tj27-vkr/RCNN-Vehicle-Tracking-Lane-Detection/blob/master/process_image.py)
