#**Finding Lane Lines on the Road** 

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 9 steps. 

#### Step 1

First of all I converted the image into an array using numpy. This is needed since the gray_image=cv2.cvtColor(image_of_example, cv2.COLOR_RGB2GRAY) accepts an array.

#### Step 2

I then converted the array to a grey image by this gray_image=cv2.cvtColor(repic, cv2.COLOR_RGB2GRAY).

#### Step 3

I then applied a Gaussian Blur to the image, with the definition of a kernel as well. This step was not needed but I wanted to practice all of the steps and still get to something.

#### Step 4

I then applied the formula for the Canny edges. This step will detect edges in the image after this one has been grayed out and blurred as described above.

####Step 5

As an intermediate but needed step, I created a copy of the image to be superimposed later on. This step is needed because is we keep only the transformation image, we then are not able to see the real image as a background. This may work for the car itself, but not for us and for this example.

####Step 6

Because we want to focus the edges image in the area where the lines are, I am then creating a 4-side polygon that embodies the area of interest from the Canny edges image. We will apply the Hough transform to that area only.

####Step 7

I am then creating the parameters that will be applied into the Hough transform. In order to draw a single line on the left and right lanes, I modified the lines() function by the use of these values for the parameters:

rho = 2
theta = np.pi/180 (this one uses Numpy to retrieve the value of Pi)
threshold = 15
min_line_length = 30
max_line_gap = 60

This way the Hough transform has been applied like so:

lines = cv2.HoughLinesP(masked_edges, rho, theta, threshold, np.array([]), min_line_length, max_line_gap)

followed by a for loop to iterate over white lines.

####Step 8

I finally combined the two images (the anotated and the copied) into one with this criterion:

result = cv2.addWeighted(color_edges, 0.8, line_image, 1, 0)

####Step 9

To finish the exercise, I loaded a video (included in the repository as well) that took over the complete procedure abovementioned in a way that the video itself, since is a collection of images, is transformed into the anotated version that was expected.


###2. Identify potential shortcomings with your current pipeline


The shortcoming that I most clearly see is that the values are very sensitive and not every image or video contains lines of the same length and width. This may cause trouble if the input video is of very different nature.

Another drawback is that the transformation algorithm did not account for lines that are not straight. If the lanes are boundaries of a curved road, then the lines procedure we created will not work.


###3. Suggest possible improvements to your pipeline

I would suggest, as a first approximation,  procedure that accounted for any typology of road, be it with curves, ups and downs, etc.
