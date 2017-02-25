#**Finding Lane Lines on the Road** 

##Project 1

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on my work in a written report


[//]: # (Image References)

[image1]: ./test_images/solidWhiteCurve_raw_result.jpg
[image2]: ./test_images/solidWhiteCurve.jpg
[image3]: ./test_images/solidWhiteRight_raw_result.jpg
[image4]: ./test_images/solidWhiteRight.jpg
[image5]: ./test_images/solidYellowCurve_raw_result.jpg
[image6]: ./test_images/solidYellowCurve.jpg
[image7]: ./test_images/solidYellowCurve2_raw_result.jpg
[image8]: ./test_images/solidYellowCurve2.jpg
[image9]: ./test_images/solidYellowLeft_raw_result.jpg
[image10]: ./test_images/solidYellowLeft.jpg
[image11]: ./test_images/whiteCarLaneSwitch_raw_result.jpg
[image12]: ./test_images/whiteCarLaneSwitch.jpg
[image13]: ./test_images/solidWhiteCurve_solid_result.jpg
[image14]: ./test_images/solidWhiteRight_solid_result.jpg
[image15]: ./test_images/solidYellowCurve_solid_result.jpg
[image16]: ./test_images/solidYellowCurve2_solid_result.jpg
[image17]: ./test_images/solidYellowLeft_solid_result.jpg
[image18]: ./test_images/whiteCarLaneSwitch_solid_result.jpg
---

### Reflection

###1. Lane Datection - pipeline description.

My pipeline consisted of 5 steps:
- convert the images to grayscale
- blur the images with gaussian_blur
- detect edged with canny function
- mask the images to only keep the region of the image defined by the polygon (I've chosen trapezoid polygon)
- detect line segments with hough_lines transformation

In order to draw a single line on the left and right lanes, I modified the draw_lines() function as follows:
- detect whether each line from hough_lines is left slope or right slope by measuring value of (y2-y1)/(x2-x1)
- fit linear function to each line segment in order to get coefficients of equation "a,b" where y=ax+b
- get median of coefficients "a" and "b" for each side in order to get mean linear equation

Once I have linear equation, I need two point for each side to draw a line: 
- I set y_left_bottom = y_right_bottom = (size of image in Y axis)
- I set y_left_top = y_right_top = (top of the selected mask region)
- Then I need to compute localization of points in X axis: y=ax+b => x = (y-b)/a 

Results of drawing raw lines:

![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]
![alt text][image7]
![alt text][image8]
![alt text][image9]
![alt text][image10]
![alt text][image11]
![alt text][image12]

Results of drawing solid lines:
![alt text][image13]
![alt text][image14]
![alt text][image15]
![alt text][image16]
![alt text][image17]
![alt text][image18]


###2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what would happen when drawing solid lines on video. This lines seems to be a little bit unstable

Another shortcoming could be that y_left_top = y_right_top = (top of the selected mask region)


###3. Suggest possible improvements to your pipeline

A possible improvement would be to remove outliers from coefficients of line segments 

Another potential improvement could be to try different configuration for Canny and Hough transform parameters