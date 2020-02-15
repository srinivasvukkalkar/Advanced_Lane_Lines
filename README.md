
### Advanced Lane Finding

#### Goals

1.	Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
2.	Apply a distortion correction to raw images.
3.	Use color transforms, gradients, etc., to create a thresholded binary image.
4.	Apply a perspective transform to rectify binary image ("birds-eye view").
5.	Detect lane pixels and fit to find the lane boundary.
6.	Determine the curvature of the lane and vehicle position with respect to center.
7.	Warp the detected lane boundaries back onto the original image.
8.	Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.


[//]: # (Image References)

[image11]: ./examples/undistort_output.png "Undistorted"
[image12]: ./test_images/test1.jpg "Road Transformed"
[image13]: ./examples/binary_combo_example.jpg "Binary Example"
[image14]: ./examples/warped_straight_lines.jpg "Warp Example"
[image15]: ./examples/color_fit_lines.jpg "Fit Visual"
[image16]: ./examples/example_output.jpg "Output"



[image1]: ./test_images_output/Output_binary_image.jpg
[image2]: ./test_images_output/Undistorted_Warped_image.jpg
[image3]: ./test_images_output/Sliding_Window_image.jpg
[image4]: ./test_images_output/Fit__poly_image.jpg
[image5]: ./test_images_output/Lane_image.jpg
[image6]: ./test_images/test2.jpg

[image7]: ./camera_cal_output/calibration9.jpg
[video1]: ./project_video.mp4 "Video"




#### 1.Camera Calibration



The code for this step is contained in the second and third code cell of the IPython notebook located in "./P2.ipynb" 

Preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world.Assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I got successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  Then applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image7]

#### 2.Apply a distortion correction to raw images.
The images below show the original and undistorted image
![alt text][image6]![alt text][image2]

#### 3. Use color transforms, gradients, etc., to create a thresholded binary image.

Used a combination of color and gradient thresholds to generate a binary image. Here's an example of my output for this step.

![alt text][image1]

#### 4.	Apply a perspective transform to rectify binary image ("birds-eye view").  

The code for my perspective transform includes a function called 'corners_unwarped', in the sixth code cells from the top. The corners_unwarped() function takes as inputs an image (img), as well as source (src) and destination (dst) points. I chose to hardcode the source and destination points in the following manner

src = np.float32([[600, 450], [720, 450], [1180, 720], [280, 720]])
dst = np.float32([[200, 0], [1000, 0], [1180, 720], [200, 720]])

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image2]

#### 5.	Detect lane pixels and fit to find the lane boundary.

Took a histogram of the bottom half of the image,Create an output image to draw on and visualize the result. Found the peak of the left and right halves of the histogram. These will be the starting point for the left and right lines. After visually analysing the I choose the number of sliding windows,Set the width of the windows and Set minimum number of pixels found to recenter window. Extracted left and right line pixel positions. With the help of 'fit_polynomial' function 

![alt text][image4]

#### 5. Calculating the radius of curvature of the lane and the position of the vehicle with respect to center.

I took mean of left and right radii. Considering the image centre as centre of the car and converted this pixel value to metres.Calculated the difference between the lane centre and centre of car. For a negative value took it as left from centre and for positive as right of centre. 

#### 6. Result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in 'pipeline' function.  Here is an example of my result on a test image:

![alt text][image5]

---

### Pipeline (video)

#### 1. Following is the link to the output video showing the lane identification

 [link to my video result][video1]

---

### Discussion

#### Issues faced and further suggestions for improvement

This pipeline does not work on too curvy roads and also with dark shadows on the road. As per my understanding these depend on the color spaces, sobel and threshold combinations. 