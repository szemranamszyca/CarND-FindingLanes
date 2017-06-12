# **Finding Lane Lines on the Road** 

## Arkadiusz Konior - Project 1.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Pipeline description.

My pipeline consisted 6 major steps. It was small class named Imgprocessor, which contained only two methods - constructor and pipeline. In pipeline method I invoked helpers function.

At the very beginning, I made some helpers variables, such as ysize, xsize and vertices of triangle, which was my region of interests on image.

First, I converted input image to greyscales. Then, I called gaussian blur with kernel size = 9. Third step was to ran Canny alghoritm to detectd edges, with thresholds between 200 and 300.

Next lines of code are preparations for Hough alghoritm. I defined all needed parameters and provide them to Hough alghoritm with image already reduced to my region of interests. This region was a trianagle (its vertices were defined at the beginning, based on image dimension).

On the output I get numpy array with detected lines, which I tried to extrapolated (**Step 5.**) I made a new helper function called:

*get_avg_line(lines, slope)*

Lines provided to this functions should be already grouped by descending or ascending slope. To made it, lines could be segrate by *get_lines_by_slope* function. Output is dictionary with two indexes:

* inc for ascending lines
* dec for descending lines

*get_avg_line* calculated average of x, y and based on that I could calulcate m-parameter for linear function:

*y = ax + m* -> *m = y - ax*

Using *np.poly1d* I get a model to extrapolated my average line, depending on if it was descending (x = 0 to near to screen center) or ascending (x near the scrreen center to the end).

It may be confusing, that descendings line "looks like" ascending, but please notice that y axis was "reverted" and higher values were at the bottom.

In my Imgprocessor class I saved average slope and use it for further calculations. I thought it would help my to prevent my pipline from short one-frame sharps, when extrapolated line was way out of a lane.

Finally, as a last step, I simply provided coordinates of points to drew a lines and wrote the image with detected lanes on it.

### 2. Identify potential shortcomings with your current pipeline

As I mentioned above, sometimes there were one-frame sharps, when detected line was far from a lane. Also, the challenge video cannot be computed. Probably because of some problems with average slope / x-y average calculations (in some frames there's no detected lines at all?)

### 3. Suggest possible improvements to your pipeline

Better parameters for Hough alghoritm probably could help from this one-frame sharps. Also, now code little is little messy. Refactoring, isolate some new methods could be an improvment. 
