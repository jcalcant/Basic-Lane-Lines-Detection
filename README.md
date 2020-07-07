# **Finding Lane Lines on the Road**


**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of the following steps. As shown in class, first, I converted the images to grayscale, then I applied gaussian blurring to get rid of noise and spurious gradients. I used kernel size 5 since it worked best during the class exercises; also the edges were clearly detected by the following canny edge detector with a threshold rate (1:3). Afterwards, I added plotting of the ROI mask so that I could observe that it covered the area corresponding to the lanes.With this, I defined the polygon's vertices. After filtering the interest area, I applied the Hough lines function so that the points corresponding to a line would be turned into one. I left ro and theta as 2 and pi/180 because I expected lines to be quite straight.To make it a little more robust I elevated the threshold to 40 and max line gap to 20. There were some very small lane lines, so I let the min line length to be of 10. 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by calculating the slopes of each of the lines obtained after applying the Hough lines function. Then, I separated left and right lane lines by the slope sign, thus creating two groups of lines. I rearranged them and concatenated all x together and y respectively so that I could then fit a linear regression model to each lane's (x,y) coordinates. After this I got the model parameters m and b corresponding to the slope and y_intercept. With these parameters, I could now extrapolate for each lane a line starting close to the horizon down to the lowest part of the image.


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the camera is not perfectly centered between both lanes, or when following a curve. Also, light conditions would affect the edge detection.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to have an algorithm to calculate the ROI mask given the image.

Another potential improvement could be to relax the ro and theta parameters in order to capture curves. At the same time, it would be necessary to keep previous lane detection masks, to keep consistency of the current one so that it doesn\'t change radically.

As for light conditions, the Canny edge detector thresholds could be readjusted as a function of a light sensor.
