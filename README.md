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

Please note that the complete pipeline for still images and videos is contained in the [./P2.ipynb](./P2.ipynb) IPython notebook file.  However, this single file, which demonstrates the pipeline for all images contained in the [./test_images](./test_images) folder, and outputs them to the [./output_images](./output_images) and [./output_videos](./output_videos) for still images and videos, respectively, is too large to display on GitHub.  Of course, all of the still image and video results can be viewed directly from within the [./output_images](./output_images) and [./output_videos](./output_videos) folders, respectively.  The portions of the pipeline as outlined by the project rubric are separated into individual IPython notebooks that can be displayed on GitHub without the need to download to your local machine.  These IPython notebooks are as follows:

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

[image4_1]: ./output_images/test3_undistorted_combined_transformed_histogram_plot.png "Histogram of transformed combined-channel binary thresholded image"
[image4_2]: ./output_images/test3_undistorted_combined_transformed_polyfit_plot.png "Sliding window lane-line identification"
[image4_3]: ./output_images/test3_undistorted_combined_transformed_polyfit_prev_plot.png "Previous polynomial lane-line identification"

[image5_1]: ./output_images/test5_undistorted_combined_transformed_polyfit_prev_radcurve_plot.png "Previous polynomial lane-line identification after radius of curvature and distance from center calculations"

[image6_1]: ./output_images/test5_final_output.png "Final Output"

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

The images below show the results of the perspective transformation.  The first image shows a side-by-side plot of one of the test images in RGB format before and after the perspective transformation.  The second image shows a side-by-side plot of one of the test images with the applied thresholds of the combined S, R, and B channels before and after the perspective transformation.  In both cases, the region of interest (ROI) based on the source and destination points is drawn as a purple polygon.

![alt text][image3_1]
![alt text][image3_2]

All output images can be found in the [./output_images/](./output_images/) folder.



#### 4. Discuss how lane-line pixels were identified, and how their positions were fit with a polynomial.  Identify where this was used in the source code.

[//]: # (You need to update this with your own description and image file)

Lane-line pixels were identified using two different methods: 1) using a sliding window, and 2) using the previously calculated polynomial from image/video data.  In both methods of finding the lane lines, the resulting image of the combined thresholded binary and perspective transform processes was used.  Both methods are demonstrated in the final cells of [./P2_01_04_Lane_Line_Pixel_Identification.ipynb](.P2_01_04_Lane_Line_Pixel_Identification.ipynb).

##### Sliding Window Method

In the sliding window method, a histogram of the lower half of the transformed thresholded binary image was generated using the `hist()` function.  This was performed because the white pixels of theimage were shown to be lane lines after the thresholding and transformation processes.  The function `fit_polynomial()` was then called, which takes the transformed thresholded binary image (grayscale) as input.  The `fit_polynomial()` function then further calls the `find_lane_pixels()` function, which takes the binary thresholded image as input and incorporates the histogram data, then returns the left and right line pixel positions as arrays (`leftx`, `lefty`, `rightx`, `righty`,), as well as the output image (`out_img`) back to the `fit_polynomial()` function.  Finally, the `fit_polynomial()` function calculates and outputs the polynomial coefficients for the left and right lanes (`left_fit` and `right_fit`, respectively) and plots them.  The results of the polynomial curves are also displayed in the output image (`out_img`) that is returned by the `fit_polynomial()` function.  The image below shows the histogram and polynomial results of the sliding window method. 

![alt text][image4_1]
![alt text][image4_2]

##### Previously Calculated Polynomial Method

In the previously calculated polynomial method, previously calculated polynomial coefficients are used to help calculate polynomial coefficients of a new image.  This is useful in video files since lane line locations do not vary drastically between two successive frames in the video.  Utilizing this method is also more efficient, since a histogram does not need to be created for each image or video frame being processed.  Like the sliding window method, the `find_lane_pixels()` function is called, taking in the binary thresholded image as input, and incorporates the histogram data, then returns the left and right line pixel positions as arrays (`leftx`, `lefty`, `rightx`, `righty`,), as well as the output image (`out_img`).  In this case, the output image is not used, but the lane line pixel positions are used to calculate new second order polynomial coefficients for the left and right lanes (`left_fit` and `right_fit`, respectively).  With these polynomial coefficients calculated, the `search_around_poly()` function is called which takes the binary thresholded image as input, as well as optional inputs to display a weighted image and the polynomial lines.  The `search_around_poly()` function grab activated pixels (i.e., nonzero or white pixels) from a selected margin (in the case of this progam, 20 pixels).  The area of search based on activated x-values within the margin of the polynomial function is set, then the left and right line pixel positions are extracted.  New polynomial coefficients are then calculated by calling the `fit_poly()` function.  Finally, the newly identified lane lines are drawn on the resulting image.  The final appearance of the output image that is returned also depends on the `weighted` and `plot_poly` inputs for the `search_around_poly()` function.

![alt text][image4_3]

All output images can be found in the [./output_images/](./output_images/) folder.



#### 5. Discuss how the radius of curvature of the lane and the position of the vehicle with respect to center were calculated.  Identify where this was used in the source code.

[//]: # (You need to update this with your own description and image file)

The radius of curvature was calculated by sending the polynomial coefficents `left_fit` and `right_fit` to the function `measure_curvature_real()`.  The meters to pixels ratios were established in the y-dimention (`ym_per_pix = 30/720`) and x-dimension (`xm_per_pix = 3.7/700`).  The radius of curvature was finally calculated for each of the left and right lane-lines by the formula R = (1 + (2Ay + B)^2)^(3/2) / abs(2A), where A and B are the first and second polynomial coefficients, and y is the height of the image (720 pixels).

The vehicle position with respect to the center of the lane was calculated by subtracting the midpoint of the image in the x-dimension `(img.shape[1] / 2)` from the midpoint of the first element in the left and right lane pixel arrays `(rightx[0] + leftx[0]) / 2)`.  In the case that this difference is negative, the vehicle has veered to the right of the center of the lane.  If the difference is (non-zero) positive, the vehicle has veered to the left of the lane.  In the case that the difference is zero, the vehicle is driving exactly in the center of the lane.

The text below shows the reported radius of curvature and position of the vehicle with respect to the center of the lane, along with the corresponding image.  The source code and output from all test images can be found in the final cell of [./P2_01_05_Radius_of_Curvature_Position.ipynb](./P2_01_05_Radius_of_Curvature_Position.ipynb).


```python
./output_images/test5_undistorted_combined_transformed.png
    Radius of Curvature:    Left = 1303.504 meters,	   Right = 1438.581 meters
    Vehicle is 0.108 meters left of center
```

![alt text][image5_1]

All output images can be found in the [./output_images/](./output_images/) folder.



#### 6. Provide an example image of the result plotted back down onto the road such that the lane area is identified clearly.

[//]: # (You need to update this with your own description and image file)

In the final step of the image pipeline, the resulting image from the `search_around_poly()` function is transformed to overlay the lane lines in the original image.  This transformation was performed by using the `transform_image()` function, and reversing the source and destination points.  The new transformation points used are as follows:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 980,   0      | 685,  450     | 
| 980, 720      | 1099, 720     |
| 300, 720      | 223,  720     |
| 300,   0      | 597,  450     |

The final output image used the `weighted_img()` function to overlay the transformed identified lane lines over the original image.  The radius of curvature and vehicle position with respect to center were overlaid on the image as `cv2.putText()` objects.  The final process of the still image pipeline can be seen in the final cell of [./P2_01_06_Still_Image_Examples.ipynb](./P2_01_06_Still_Image_Examples.ipynb).

![alt text][image6_1]

All output images can be found in the [./output_images/](./output_images/) folder.



---

### Video Pipeline

#### Provide a link to the final video output.  The pipeline should perform reasonably well on the entire project video.  In other words, wobbly lines are acceptable, but catastrophic failures that would cause the car to drive off the road are not!.

[//]: # (You need to update this with your own description and image file)

The video pipeline is a condensed version that includes all of the steps mentioned above for the still image pipeline, with two major exceptions: 1) the omission of the sliding window method of lane-line identification, and 2) the inclusion of a class that is instantiated to save lane pixel information.

The `Lane_Data` class was necessary to save variables to calculate the polynomial coefficients to identify the lane lines using the previously calculated polynomial method.  Without this, videos produced unexpected results when attempting to find lane-lines.  Below is the source code showing how the class was defined.

```python
# Define class to save data
class Lane_Data:
    leftx = []
    lefty = []
    rightx = []
    righty = []
    out_img = []
    left_fit = []
    right_fit = []
```

In any cell that uses the video pipeline, the class is instantiated prior to processing the video file (e.g., `LD = Lane_Data()`).  The source code of the video pipeline, and the processing of all of the videos included in this project, can be found in [./P2_02_01_Video_Pipeline.ipynb](./P2_02_01_Video_Pipeline.ipynb).

The video for the project with the identified lane-lines can be found at [./output_videos/project_video.mp4](./output_videos/project_video.mp4)

All output videos can be found in the [./output_videos/](./output_videos/) folder.

If the video files are too large to be displayed in the browser from GitHub, please download them to your local computer.



---

### Final Discussion

#### Discuss any problems or issues you faced during the implementation of this project.  

[//]: # (Where will your pipeline likely fail?  What could you do to make it more robust?)

The common approach for all images and video frames was to use the same thresholding methods (threshold binary, threshold gradients, magnitude gradients, and directional gradients combinations) for each of the S, R, and B channels.  While this was effective for the [project_video](./output_videos/project_video.mp4) with a couple minor glitches observed during playback, it did not produce consistent results for the challenge videos, as can be seen in the [./output_videos/](./output_videos/) folder.  The challenge videos show lane line "identification" that could be catastrophic if deployed in a real self-driving car.  

Performing the same threshold processing methods on each of the S, R, and B channels resulted in "overprocessing," in that too much noise was introduced into the images.  In scenarios where the lane lines were the most distinct features on the road, this did not cause issues.  In scenarios where other lines and edges were more pronounced than the marked lane lines in the image, the lane finding algorithm failed due to the amount of noise in the binary image.  In fact, less processing of the R and B channels (e.g., simply using only the binary threshold of the R and B channels) would produce cleaner combined images, and lane-line identification less likely to fail.  In addition to the analysis of RGB and HSL color spaces that were explored in this project, more time could be spent exploring additional color spaces (e.g., LAB, HSV, YCrCb) and channel thresholding and combination methods to produce cleaner filters for lane-lines in the road in an effort to eliminate noise that contributes to false-positives, and hence errors resulting in the polynomial fit calculation.  This would be worthwhile future work to create a more effective method of correctly identifying lane lines in all road conditions.

