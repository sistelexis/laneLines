# **Finding Lane Lines on the Road** 

## Self-Driving Car Engineer Nanodegree - Term 1- Project 1

### This project goal is to identify lane line in images and videos. To achieve that we will code in Python using the openCV library.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

<img src="laneLines_thirdPass.jpg" width="480" alt="Goal" />

---

### Reflection

### 1. First approach.

#### Part 1

Based on the knowledge from the lesson, I decided to follow some steps, and then try to fine tune parameters.
So after opening an image I followed these steps:
1. convert it to gray scale, in order to have a simpler starting point
2. reduce the amount of small details that would behave as noise for the project using a gaussian blur filter 
3. enhance edges from the image using a canny filter
4. reduce the analysis area of the image to where it really matters
5. use Hough transform to identify point from a same line

Then the results would be overlayed to the starting image to show the results of the pipeline.

Results: after fine tuning all the helper functions parameters, I achieved satisfactory results for this first step

#### Part 2

Once the basic detection achieved, I started researches to understand which could be the best approach to replace all those lines by a single line per lane. So to the first 5 points previously described, another point has been added:

6. define trend lines using the results from the Hough transform

As in part 1, the result would be overlayed to the starting image

Results: the achived results provided clean lines on each side of the road perfectly matching the position of the lane lines.

To stress test the pipeline, it has been tested with the optional challenge video.
At first, before the bridge, since the video was shot during a curb, the trend lines were kind of respecting the lane lines, although jumping quite a bit. But, as soon as the car got near the bridge, the trend lines start going crazing crossing each others like scisors.
Obviously the pipeline had just shown its limits.
So we needed a new approach.

### 2. Second approach.

since the secret of the success appeared to be on defining better trend lines, I concentrated my efforts on part 2.
So I started testing several ideas:
1. left lane and right lane are very different in two points: one is in one half of the image and the other one in the other half, besides having slopes with oposite signals. So, all the lines that did not respect that logic, that would most likely be noise created by other details from the image that not lanes, would be removed, reducing the estimation error of the trend line.
2. since the lanes are paralel to the car, extreme slopes are not possible. So small lines with impossible slopes should be cleared out of the valid line list.
3. since the movie was from a curb, using polinomial trend lines would also allow to reduce the error.

all the attempts have been a failure since on most of them, exceptions were created, and consequently the code failed.
So another approach had to be done. Instead of cleaning noise at the end of the pipeline, why not try to get it clean on the first stages?

### 3. Third approach.
So the idea was to find something different.
From the lesson it has been seen that the gradient provided better results than the color filtering. But on the challenge video instead of having an always sunny straight line, we had curbs, we had shadows and we also had a bridge with a different floor color.
So based on researches, I realized that combining a color filtering with the existing steps (grayscale, blur, edge detection) would provide another step of pre-processing (or step 0) that would give good results on the noise reduction. Those results were even better converting the color code from RGB to HLS

### 2. Potential shortcomings with the final pipeline

since moving from a straight to a open curb, from sunny to shadow, and from regular to changeble floor color proved a valid solution to become a bad one, most likely, small roads, rain might also give a hard time to the final pipeline.

### 3. Suggestion of possible improvements to the final pipeline

Polinomial trend lines might become unavoidable, as weel as other technics for noise of details detection.

### 4. final pipeline - step by step

1. Starting image
<img src="results/1-image.png" width="480" alt="Goal" />

2. Grayscale with color mask
<img src="results/2-grayscale_with_color_detection_mask.png" width="480" alt="Goal" />

3. Color convertion from RGB to HLS
<img src="results/3-HLV_convertion.png" width="480" alt="Goal" />

4. Blur image with Gaussian filter
<img src="results/4-blur_filter.png" width="480" alt="Goal" />

5. Edge detection
<img src="results/5-edge_detection.png" width="480" alt="Goal" />

8. Definition of the section to be analyzed
<img src="results/8-section_filtering.png" width="480" alt="Goal" />

9. Result for basic detection
<img src="results/9-basic_detection.png" width="480" alt="Goal" />

10. Result for advanced detection
<img src="results/A-advanced_detection.png" width="480" alt="Goal" />


