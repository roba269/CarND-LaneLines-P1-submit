# **Finding Lane Lines on the Road** 

This is a write-up for Project 1 - Finding Lane Lines in Udacity's Self Driving Car Nanodegree program by Liang He.

### Pipeline description

My pipeline is mostly following the steps in the course videos, with a few additional tricks which will be described later.

* Covert the image to gray scale and apply white and yellow color masks to enhance the corresponding color.
* Perform Canny edge detection
* Select two regions for two lanes separately
* Perform Hough transform to get line segments
* Extrapolate two lanes from line segments

Some details:

* By coverting images to HSV space, it's easier to fetch yellow color mask, and the result is slightly better.
* We can set two regions for left lane and right lane separately, so that when doing extrapolation, we can avoid clustering two groups of segments explicitely.
* The slope of lane segments will fall into a relatively narrow range, for example, we don't need consider slope 0.1 or 10.
* There are multiple ways for extrapolating, I found that `cv2.FitLine` is easy to use. Internally `cv2.FitLine` uses liner regression to fit the points.

Here are some example output below. Please check `test_images_output` and `test_videos_output` directories for all of them.

[solidWhiteRight]: ./test_images_output/solidWhiteRight.jpg "solidWhiteRight"
[solidYellowCurve]: ./test_images_output/solidYellowCurve.jpg "solidYellowCurve"

![alt text][solidWhiteRight]
![alt text][solidYellowCurve]

### Potential shortcomings with current pipeline

When testing on static images, everything looks good. But when running on videos, it's easy to notice that the lanes are jittering, sometimes significantly.

When the color of lane is not significantly different from the background, the result will be not super good. For example, the result is very off in the middle of the last challenging video. 

The regions and some color thresholds are hard-coded, not general enough.

### Suggest possible improvements to your pipeline

A possible improvement for the jittery motion can be taking previous frames into consideration. But I feel hesitate to do this because it will significant change our framework.

The parameters of Canny detection and Hough transformation can be more finely turned. Though I took lot of effort tuning them, I think there is still room for improvement.

We should try to eliminate the hard-coded values when possible.

Regarding segments extrapolation, I think there are more sophisticated approaches than liner regression.

