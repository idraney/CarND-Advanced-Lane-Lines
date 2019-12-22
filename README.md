[//]: # (## Writeup Template)

[//]: # (### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.)

[//]: #(---)



# Advanced Lane Finding Project - README.md

The goals of the Advanced Lane Finding project are as follows:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1_1]: ./output_images/calibration2_corners_plot.png "Finding Corners"
[image1_2]: ./output_images/calibration2_undistortion_plot.png "Applied Undistortion"
[image1_3]: ./output_images/calibration1_undistortion_plot.png "Applied Undistortion"

[image1]: ./output_images/calibration1_undistorted_plot.png "Undistorted"

[image2]: ./test_images/test1.jpg "Road Transformed"
[image3]: ./examples/binary_combo_example.jpg "Binary Example"
[image4]: ./examples/warped_straight_lines.jpg "Warp Example"
[image5]: ./examples/color_fit_lines.jpg "Fit Visual"
[image6]: ./examples/example_output.jpg "Output"
[video1]: ./project_video.mp4 "Video"



## Rubric Points
[//]: # (## [Rubric Points] https://review.udacity.com/#!/rubrics/571/view )

The rubric points of the Advanced Lane Finding project were considered individually.  Each point of the rubric is addressed in this implementation.  The original project rubric can be seen [here](https://review.udacity.com/#!/rubrics/571/view).

---

[//]: # (### Writeup / README)

[//]: # (#### Provide a Writeup / README that includes all the rubric points, and how each point was addressed.  The writeup may be submitted as a markdown MD or PDF file.   This submission is a markdown file.)

[//]: # ([Here] https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md is a template writeup for this project you can use as a guide and a starting point.)

[//]: # (You're reading it!)



### Camera Calibration

#### State how the camera matrix and distortion coefficients are computed.  Provide an example of a distortion corrected calibration image.

[//]: # (You need to update this with your own description and image file)

The code for this step is contained in the first code cell of the IPython notebook located in [P2.ipynb](./P2.ipynb).

To begin, the image "object points", or (x, y, z) coordinates of the chessboard corners, were prepared.  It is assumed that the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, the variable `objp` is just a replicated array of coordinates, and `objpoints` are appended with a copy of it when all chessboard corners in the test images are detected successfully in `for()` loop iterations.  The array variable `imgpoints` is appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection in the same `for()` loop.  

### Camera calibration and Image Undistortion

#### Undistort the chessboard images using the camera matrix and distortion coefficients.

Using the detected corners from the previous section, the function `cal_undistort()` was created to undistort the images.  The calibration step was achieved by calling the `cv2.calibrateCamera()` function, which takes the `imgpoints` and `objpoints` arrays, as well as the image size, and returns the camera matrix, distortion coefficients, rotation vectors, and translation vectors.  Thie image, the camera matrix, and distortion coefficients are then sent to `cv2.undistort()`, which returns the undistorted image.

![alt text][image1_1]
![alt text][image1_2]

In the example images below, the original image was not included in the camera calibration step because the full view of its 9 x 6 corners are not shown.  Hence, the image was not included in the calculation of the distortion coefficients.  However, applying the the distortion coefficients that were calculated from the images with the complete corner set in view results in an undistorted image.

![alt text][image1_3]

Output images appear in the [./output_images](./output_images) folder.



### Camera Calibration

#### State how the camera matrix and distortion coefficients are computed.  Provide an example of a distortion corrected calibration image.

After the `for()` loop iterations were complete, the arrays `objpoints` and `imgpoints` were used to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  The distortion correction was applied to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]


### Pipeline - Single (Still) Images

[//]: # (You need to update this with your own description and image file)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

#### 2. Discuss how color transforms, gradients, or other methods to create a thresholded binary image were used.  Identify where this was used in the source code.  Provide an example of a binary image result.

[//]: # (You need to update this with your own description and image file)

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps at lines # through # in `another_file.py`).  Here's an example of my output for this step.  (note: this is not actually from one of the test images)

![alt text][image3]

#### 3. Discuss how the perspective transform was performed.  Identify where this was used in the source code.  Provide an example of a resulting transformed image.  

[//]: # (You need to update this with your own description and image file)

The code for my perspective transform includes a function called `warper()`, which appears in lines 1 through 8 in the file `example.py` (output_images/examples/example.py) (or, for example, in the 3rd code cell of the IPython notebook).  The `warper()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
src = np.float32(
    [[(img_size[0] / 2) - 55, img_size[1] / 2 + 100],
    [((img_size[0] / 6) - 10), img_size[1]],
    [(img_size[0] * 5 / 6) + 60, img_size[1]],
    [(img_size[0] / 2 + 55), img_size[1] / 2 + 100]])
dst = np.float32(
    [[(img_size[0] / 4), 0],
    [(img_size[0] / 4), img_size[1]],
    [(img_size[0] * 3 / 4), img_size[1]],
    [(img_size[0] * 3 / 4), 0]])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 585, 460      | 320, 0        | 
| 203, 720      | 320, 720      |
| 1127, 720     | 960, 720      |
| 695, 460      | 960, 0        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Discuss how lane-line pixels were identified, and how their positions were fit with a polynomial.  Identify where this was used in the source code.

[//]: # (You need to update this with your own description and image file)

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image5]

#### 5. Discuss how the radius of curvature of the lane and the position of the vehicle with respect to center were calculated.  Identify where this was used in the source code.

[//]: # (You need to update this with your own description and image file)

I did this in lines # through # in my code in `my_other_file.py`

#### 6. Provide an example image of the result plotted back down onto the road such that the lane area is identified clearly.

[//]: # (You need to update this with your own description and image file)

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![alt text][image6]

---

### Video Pipeline

#### Provide a link to the final video output.  The pipeline should perform reasonably well on the entire project video.  In other words, wobbly lines are acceptable, but catastrophic failures that would cause the car to drive off the road are not!.

[//]: # (You need to update this with your own description and image file)

Here's a [link to my video result](./project_video.mp4)

---

### Final Discussion

#### Discuss any problems or issues you faced during the implementation of this project.  

[//]: # (Where will your pipeline likely fail?  What could you do to make it more robust?)

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  

I like pizza.