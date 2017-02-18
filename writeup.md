# **Finding Lane Lines on the Road**


**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consists of 6 steps.
1. First, I converted the images to grayscale.
1. Applied Gaussian blur with kernel size of 5.
1. Applied Canny edge detector. Initially, I chose 50 for low threshold and 150 for high threshold. It worked fine for white and yellow samples but failed to find yellow line on challenge sample. So I changed it to 20/60 and it seems okay for most of the sample images and videos.
1. Applied masking for region of interest. It's a trapezoid covers bottom 40% of the image.
1. Applied Hough transform to find lines from image. I used same parameters from previous quiz. This step includes draw_lines() function.
1. Added line image from Hough transform to initial image.

In draw_lines() function, I find lane line and decide left or right side. Here's what I did to achieve goals.
* To find lane lines, I captured several screenshots from sample videos and made criteria below
  * Lane line passes vanishing point.
  * Vanishing point is fixed vertically at top 56%. It is centered horizontally, but moves about 6%.
  * Angle of Lane line changes between 29 and 40 degree.
* To find lines which meet these criteria, I did steps below.
  * Find angle in degree and test if it meets criteria.
  * Extrapolate lines vertically from bottom to vanishing point. Test if upper point is in range.
* If it meets criteria, decide left or right side by its angle.
* Average left lines and right lines.
* Draw each line if its found.

### 2. Identify potential shortcomings with your current pipeline


Major shortcoming is that you have to find vanishing point and lane angles manually and it is fixed during the pipeline. Though you can assume that camera is fixed, vanishing point can be differed if the slope of the road changes. It will be higher if the slope increases, and will be lower vice versa. Lane angle also can be changed according to the lane width or curvature of the road.

### 3. Suggest possible improvements to your pipeline

A possible improvement is adaptively find vanishing point. It can be found extending all the lines found with Hough transform and find most crossing point.

Another potential improvement could be to adjust parameters for Canny or Hough transform. The pipeline know if it's successfully found both left and right lane or not. If it fails it can adjust parameters and try again until it succeeds.
