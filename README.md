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

Please note that the complete pipeline for still images and videos is contained in the [./P2.ipynb](./P2.ipynb) IPython notebook file.  However, this single file, which demonstrates the pipeline for all images contained in the [./test_images](./test_images) folder, and outputs them to the [./output_images](./output_images) and [./output_videos](./output_folders) for still images and videos, respectively, is too large to display on GitHub.  The portions of the pipeline as outlined by the project rubric are separated into individual IPython notebooks that can be displayed on GitHub without the need to download to your local machine.  These IPython notebooks are as follows:

* [./P2_00_01_Calibration_DistCoeffs.ipynb](./P2_00_01_Calibration_DistCoeffs.ipynb)
* [./P2_01_01_Distortion_Correction.ipynb](./P2_01_01_Distortion_Correction.ipynb)
* [./P2_01_02_Color_Spaces.ipynb](./P2_01_02_Color_Spaces.ipynb)
* [./P2_01_02a_Thresholds.ipynb](./P2_01_02a_Thresholds.ipynb)
* [./P2_01_03_Perspective_Transform.ipynb](./P2_01_03_Perspective_Transform.ipynb)
* [./P2_01_04_Lane_Line_Pixel_Identification.ipynb](.P2_01_04_Lane_Line_Pixel_Identification.ipynb)
* [./P2_01_05_Radius_of_Curvature_Position.ipynb](./P2_01_05_Radius_of_Curvature_Position.ipynb)
* [./P2_01_06_Still_Image_Examples.ipynb](./P2_01_06_Still_Image_Examples.ipynb)
* [./P2_02_01_Video_Pipeline.ipynb](./P2_02_01_Video_Pipeline.ipynb)

All output videos can be found in the [./output_videos/](./output_videos/) folder.

All output images can be found in the [./output_images/](./output_images/) folder.

[//]: # (Image References)

[image0]: ./output_images/calibration1_undistorted_plot.png "Undistorted"
[image0_1]: ./output_images/calibration2_corners_plot.png "Finding Corners"
[image0_2]: ./output_images/calibration2_undistorted_plot.png "Applied Undistortion"
[image0_3]: ./output_images/calibration1_undistorted_plot.png "Applied Undistortion"

[image1]: ./test_images/test1.jpg "Original Road Image"
[image1_1]: ./output_images/test1_undistorted.png "Road Image Undistorted"

[image2_1]: ./output_images/test5_undistorted_RGB_gray_plot.png "RGB image and grayscale image"
[image2_2]: ./output_images/test5_undistorted_RGB_channels_plot.png "RGB channels"
[image2_3]: ./output_images/test5_undistorted_HLS_channels_plot.png "HLS channels"
[image2_4]: ./output_images/test5_undistorted_S_channel_threshold_plot.png "S channel thresholding"
[image2_5]: ./output_images/test5_undistorted_R_channel_threshold_plot.png "R channel thresholding"
[image2_6]: ./output_images/test5_undistorted_B_channel_threshold_plot.png "B channel thresholding"
[image2_7]: ./output_images/test5_undistorted_stacked_combined_plot.png "Stacked and combined channels"

[image3_1]: ./output_images/test2_transformed_plot.png "Perspective transform RGB image"
[image3_2]: ./output_images/test2_undistorted_combined_transformed_plot.png "Perspective transform combined channels"

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

To begin, the image "object points", or (x, y, z) coordinates of the chessboard corners, were prepared.  It is assumed that the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, the variable `objp` is just a replicated array of coordinates, and `objpoints` are appended with a copy of it when all chessboard corners in the test images are detected successfully in `for()` loop iterations.  The array variable `imgpoints` is appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection in the same `for()` loop.  

The code for this step may be viewed in the IPython notebook [./P2_00_01_Calibration_DistCoeffs.ipynb](./P2_00_01_Calibration_DistCoeffs.ipynb) without downloading the full pipeline file [P2.ipynb](./P2.ipynb).



### Camera calibration and Image Undistortion

#### Undistort the chessboard images using the camera matrix and distortion coefficients.

Using the detected corners from the previous section, the function `cal_undistort()` was created to undistort the images.  The calibration step was achieved by calling the `cv2.calibrateCamera()` function, which takes the `imgpoints` and `objpoints` arrays, as well as the image size, and returns the camera matrix, distortion coefficients, rotation vectors, and translation vectors.  The image, the camera matrix, and distortion coefficients are then sent to `cv2.undistort()`, which returns the undistorted image.

![alt text][image0_1]

![alt text][image0_2]

In the example images below, the original image was not included in the camera calibration step because the full view of its 9 x 6 corners are not shown.  Hence, the image was not included as input for the `imgpoints` and `objpoints` arrays.  However, applying the `imgpoints` and `objpoints` arrays that were built from other images resulted in distortion coefficients that could be calculated for this image using `cv2.calibrateCamera()`, which finally resulted in distortion correction with `cv2.undistort()`.

![alt text][image0_3]

The code for this step may be viewed in the IPython notebook [./P2_00_01_Calibration_DistCoeffs.ipynb](./P2_00_01_Calibration_DistCoeffs.ipynb) without downloading the full pipeline file [P2.ipynb](./P2.ipynb).

All output images can be found in the [./output_images/](./output_images/) folder.



### Pipeline - Single (Still) Images

[//]: # (You need to update this with your own description and image file)

#### 1. Provide an example of a distortion-corrected image.

The `cv2.calibrateCamera()` and `cv2.undistort()` functions were applied to the road test images to apply distortion correction.  This was performed using the same `imgpoints` and `objpoints` arrays that were acquired during camera calibration.  It can be seen in the original test image that the white car is fully in view, while in the undistorted image, only part of the white car is visible (i.e., the right tail light of the white car cannot be seen).

Original road test image:
![alt text][image1]

Undistorted road test image:
![alt text][image1_1]

The code for this step may be viewed in the IPython notebook [./P2_01_01_Distortion_Correction.ipynb](./P2_01_01_Distortion_Correction.ipynb) without downloading the full pipeline file [P2.ipynb](./P2.ipynb).



#### 2. Discuss how color transforms, gradients, or other methods to create a thresholded binary image were used.  Identify where this was used in the source code.  Provide an example of a binary image result.

[//]: # (You need to update this with your own description and image file)

All images in the [./test_images](./test_images) folder were distortion-corrected, they separated into their individual R, G, B, H, L, and S channels for analysis.  The S, R, and B channels were selected to create a composite thresholded binary image since these channels could reveal the lane lines through individual channel binary thresholding (`channel_thresh()`), *x* and *y* Sobel operations (`abs_sobel_thresh()`), gradient magnitude (`mag_thresh()`), and gradient direction (`dir_threshold()`).  These operations on the S, R, and B channels with an additional binary threshold of each channel.  Finally, the resultant binary thresholds were combined through OR (`|`) operations to create a single threshold binary image to isolate lane lines.

Below shows an example of taking one of the test images, and separating its individual color space channels: gray, R, G, B, H, S, and L.  The binary thresholds, gradient thresholds, and magnitute gradient, and directional gradient of each of S, R, and B channels are then displayed, in which these thresholds are combined for each respective channel.  Finally, the individually thresholded S, R, and B channels are combined to produce a "filtered" binary image that will be used to identify lane lines after a perspective transformation.

![alt_text][image2_1]
![alt_text][image2_2]
![alt_text][image2_3]
![alt_text][image2_4]
![alt_text][image2_5]
![alt_text][image2_6]
![alt_text][image2_7]

The code for this step may be viewed in the IPython notebooks [./P2_01_02_Color_Spaces.ipynb](./P2_01_02_Color_Spaces.ipynb) and [./P2_01_02a_Thresholds.ipynb](./P2_01_02a_Thresholds.ipynb) without downloading the full pipeline file [P2.ipynb](./P2.ipynb).

All output images can be found in the [./output_images/](./output_images/) folder.



#### 3. Discuss how the perspective transform was performed.  Identify where this was used in the source code.  Provide an example of a resulting transformed image.  

[//]: # (You need to update this with your own description and image file)

The code for the perspective transform includes the function `transform_image()`, which appears in the last cell of the IPython notebook [./P2_01_03_Perspective_Transform.ipynb](./P2_01_03_Perspective_Transform.ipynb).  The `transform_image()` function takes as inputs an image (`img`) and the transformation points (`vertex_TR`, `vertex_BR`, `vertex_BL`, `vertex_TL`, `vertex_TR_xfm`, `vertex_BR_xfm`, `vertex_BL_xfm`, and `vertex_TL_xfm`), where the first four points are the source points, and the last four points (`_xfm`) are the destination points.  The source and destination points are the same for all images and videos in this project since they are all the same size (1280 pixels by 720 pixels).  These transformation points are coded within the `transform_image()` function as follows:

```python
# Source coordinates
src = np.float32(
    [[vertex_TR[0], vertex_TR[1]],
     [vertex_BR[0], vertex_BR[1]],
     [vertex_BL[0], vertex_BL[1]],
     [vertex_TL[0], vertex_TL[1]]])

# Destination coordinates
dst = np.float32(
    [[vertex_TR_xfm[0], vertex_TR_xfm[1]],
     [vertex_BR_xfm[0], vertex_BR_xfm[1]],
     [vertex_BL_xfm[0], vertex_BL_xfm[1]],
     [vertex_TL_xfm[0], vertex_TL_xfm[1]]])
```

Where the pixel locations of each of the transformation points are defined outside of the function:

```python
# Create ROI vertices, starting from Top Right (TR), clockwise 
vertex_TR = (685,  450)
vertex_BR = (1099, 720)
vertex_BL = (223,  720)
vertex_TL = (597,  450)

# Create transformed vertices, starting from Top Right (TR), clockwise 
vertex_TR_xfm = (980, 0)
vertex_BR_xfm = (980, 720)
vertex_BL_xfm = (300, 720)
vertex_TL_xfm = (300, 0)
```
In a table format, the source and destination points are defined as:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 685,  450     | 980,   0      | 
| 1099, 720     | 980, 720      |
| 223,  720     | 300, 720      |
| 597,  450     | 300,   0      |

The images below show the results of the perspective transformation.  The first image shows a side by side plot of one of the test images in RGB format before and after the perspective transformation.  The second image shows a side-by-side plot of one of the test images with the applied thresholds of the combined S, R, and B channels before and after the perspective transformation.  In both cases, the region of interest (ROI) based on the source and destination points is drawn as a purple polygon.

![alt text][image3_1]
![alt text][image3_2]



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