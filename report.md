# **Finding Lane Lines on the Road** 

## Arkadiusz Konior - Project 1.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Pipeline description

My pipeline consisted of 6 major steps. It was a small class named Imgprocessor, which contained only two methods - constructor and pipeline. In pipeline method I invoked helper functions.

At the very beginning, I made some helper variables, such as ysize, xsize and vertices of triangle, which was my region of interests on image.

First, I converted input image to greyscales. Then, I called gaussian blur with kernel size = 9. Third step was to run Canny algorithm to detect edges, with thresholds between 200 and 300.

Next lines of code are preparations for Hough algorithm. I defined all needed parameters and provided them to Hough algorithm with image already reduced to my region of interests. This region was a trianagle (its vertices were defined at the beginning, based on image dimension).

On the output I got numpy array with detected lines, which I tried to extrapolate (**Step 5.**) I made a new helper function called:

*get_avg_line(lines, slope)*

Lines provided to this function should be already grouped by descending or ascending slope. In order to do this, I segregated lines by *get_lines_by_slope* function. The output is dictionary with two indexes:

* inc for ascending lines
* dec for descending lines

*get_avg_line* calculated average of x, y, based on which I could calulcate m-parameter for linear function:

*y = ax + m* -> *m = y - ax*

Using *np.poly1d* I got a model to extrapolate my average line, depending on whether it was descending (x = 0 to near to screen center) or ascending (x near the screen center to the edge of the image).

Although it may be a little confusing, as this descending line looks like ascending, please notice that y axis is "reverted", and higher values are at the bottom.

In my Imgprocessor class I saved average slope and used it for further calculations. I thought it would help me to prevent my pipeline from short one-frame sharps, when extrapolated line did not cover the lane.

Finally, as the last step, I simply provided coordinates of points to draw lines and saved the image with detected lanes on it.

### 2. Identify potential shortcomings with your current pipeline

As I mentioned above, sometimes there were one-frame sharps, when detected line was far from the lane. Also, the challenge video cannot be computed. Probably, it happens because of some problems with average slope / x-y average calculations (in some frames there's no detected line at all?)

### 3. Suggest possible improvements to your pipeline

Better parameters for Hough algorithm could help me avoid these one-frame sharps. Also, now my code is a little messy. Refactoring and isolating some new methods could be an improvement. 
