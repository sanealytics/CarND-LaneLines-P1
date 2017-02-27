#**Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)


Here are some results
[image1]: ./test_images/post/processed-solidWhiteCurve.jpg
[image2]: ./test_images/post/processed-solidYellowLeft.jpg
[image3]: ./test_images/post/processed-whiteCarLaneSwitch.jpg


---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps. First, I converted the images to grayscale, then I blurred it with kernel of 5. I then applied a canny trasform with min/max of 50/150, keeping the recommended ratio of 1:3.
I applied hough transform after that. Then I filtered it and merged the final lines using weighted_img()

In order to draw a single line on the left and right lanes, I first created a method get_model() that can create a simple linear regression, and then modified the draw_lines() function by calling it for both left and right. This works on the premise that the lines are straight and follow -

y = slope * x + intercept

The same formula is used to then predict it on two points and add only two lines to the output instead of many before. We use the fact that we know the y dimension (bottom of image to 1/3rd up from there) and predict the x using our model to plot the two lines.


###2. Identify potential shortcomings with your current pipeline


One issue is that it does not use all the data it is given. It is getting a video but processes each frame individually. Another issue is that there is no feedback loop, it is completely unsupervised.

Another shortcoming is that the model is linear and in the normal space. This means it cannot exploit the curvatures, etc done by hough transform in the polar space. I tried a polynomial model but that seemed to overfit rightaway.

Another shortcoming is that the lighting conditions are great for all these videos. I am not sure what would happen with cloudy skies.

###3. Suggest possible improvements to your pipeline

1) Use the polar space instead of cartesian. Each line is a point there. Furthermore we can still use our heuristic of negative slopes mean left line. It will be easier to spot outliers and fit a more robust model.

2) Use data from past video frames. Lines can't change by too much between two frames.

3) Actually take this a step further and try to predict the next frame. This will put is in the familiar supervised learning space and allow us to fit a robust model with a lot of capacity.

4) Use multiple inputs. This relies too heavily on visual, and a single point of failure.
