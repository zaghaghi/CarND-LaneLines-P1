**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[gray]: ./images/gray.png "Grayscale"
[edge]: ./images/edge.png "Canny Edge"
[mask]: ./images/mask.png "Masked"
[lines]: ./images/lines.png "Lines"
[lanes]: ./images/lanes.png "Lanes"
[overlay]: ./images/overlay.png "Overlay"

---

### Reflection

### 1. Pipeline

My pipeline consisted of N steps. First, I converted the images to grayscale, then I applied gaussian blur filter with 5x5 kernel to smooth the image.

![gray][gray]

After that I applied canny edge detection with 50 and 150 thresholds.

![edge][edge]

Then I masked the edge detected image with a defined region.

![mask][mask]

After masking edege detected image, I applied hough_lines to detect lines with these parameters:
* rho: 2
* theta: 1 degree
* threshold: 15
* min_line_length: 40
* max_line_gap: 20

![lines][lines] 

After finding lines in masked edge image, In applied a function for detecting lanes. This function that I wrote has 5 sub-steps:

1. Compute slopes(m) and intercepts(b) of all line segements
2. Seperate left and right line segments using slope sign. I defined two lists, one for positive slopes and one with negative slopes. In this way I can split lines in two different sets, one set is related to left lane and the other one is related to right lane.
3. Compute average and standard deviation of slopes in each set. I used average and stdev to remove unwanted lines, in other words I removed lines that have slopes greater than avg + stdev and smaller than avg - stdev.
4. Compute average on remaining slopes and intercepts of line segements in each set.
5. Compute moving average of slopes and intercepts of lines segments to stablize lane lines between each frame. (moving average of n = 7)
5. Finally, build two lines and draw them on an empty image

![lanes][lanes]

After drawing lane lines on an empty image, I merged this image with original color image.

![overlay][overlay]

### 2. Identify potential shortcomings with your current pipeline


I found these shortcommings:

* The road must be clear in the lane where the car is driving.
* When road turns hard to left or right.
* Shadows of trees on the road or other light conditions.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to normalize light of original image before converting it to gray image. This could help tackling different light conditions.

I Tested morphological filters (dilate filter followed by erode filter) after gaussian blur and before canny edge detection. I thought this filter would help in connecting small lane lines to each other and gain better results but it did not what I thought. 