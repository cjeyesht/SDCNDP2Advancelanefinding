## Advanced Lane Finding 


The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.


## [Rubric](https://review.udacity.com/#!/rubrics/1966/view) Points



### Camera Calibration

So the camera calibration helps to deal with distortion problems. 

The first step of this camera calibration is to start with calibrating 9x6 chessboard images. Object points (3D points in real world space) and Image points(2d points in the image plane) . Using cv2.findChessboardCorners() to the grayscale image of the input image finds the corner points on the chessboard image and storing into arrays of object and image points. 

The function cv2.calibrateCamera() calibrates the camera using the output objpoints and imgpoints to compute the camera calibration and distortion coefficients. I applied this distortion correction to the test image using the cv2.undistort() function and obtain a undistorted image of the 9x6 chessboard. 

### Color Transform and Gradient 

The next step in the project is to make use of directional gradient threshold and color thresholds to improve the lane detection by increase in details of differentiating the white and yellow lanes also improves the lane detection when the vehicle goes by the shaddy areas. 

- Sobel operator is used to get derivates of x and y for imporved edge detection. 
- Direction gradient is used to identify specific edges in a particular orientation.  
- on line [13] the original and the thresholded image are plotted. 

### Perspective Transform 

The code on the line [17] for pespective transform involves manual calculations of the source and destination points. 


I manually chose the source and destination points from the input image. 

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 225, 725      | 300, 720      | 
| 575, 475      | 320, 1        |
| 720, 475      | 850, 1        |
| 1110, 725     | 850, 720      |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

-Histogram is used to identify and map out the lane lines from the warped image. 


### Implementation of Sliding Windows

This [18] steps invloves in implementing sliding windows to determine where the lane lines go. First all the pixels of the lane lines both right and left lanes are found seperately. 
- A polynomial is fit to the line using np.polyfit.
- Once the lanes are predicted then a margin is determined to which activated pixels fall into the green shaded areas from the image. 


### Radius of curvature and center offset 

The code for finding the radius of curvature and center offset can be found in line [20]. The code prints out the radius of curvature and center offset values at the end. 

Radius of curvature: 5667.32 m
Center offset: 0.39 m

meter per pixel calculation     
    ym_per_pix = 30/720 # meters per pixel in y dimension
    xm_per_pix = 3.7/700 # meters per pixel in x dimension
    
### Inverse transform 

This part of the code [21] gets the output as the margin of the lanes added to the input image.

### Last step to the image frame

This code from the line [22] involves all the output from the above functions adding altogether to get the final frame. The final output frame invloves displaying the lane detected. And then displaying the radius of curvature and the center offset value in meters. 


[23] the code on this line shows how the pipeline works on a test image. 
[24] the code on this line shows how the pipeline works on a test video. 

Here's a link to the project video output. - https://classroom.udacity.com/nanodegrees/nd013/parts/168c60f1-cc92-450a-a91b-e427c326e6a7/modules/5d1efbaa-27d0-4ad5-a67a-48729ccebd9c/lessons/7cb63828-36aa-4cea-9239-700b5ea41f0b/concepts/0a96d23f-6c22-4053-a7f6-83e12ce5a6ec 

Challenge video output - https://classroom.udacity.com/nanodegrees/nd013/parts/168c60f1-cc92-450a-a91b-e427c326e6a7/modules/5d1efbaa-27d0-4ad5-a67a-48729ccebd9c/lessons/7cb63828-36aa-4cea-9239-700b5ea41f0b/concepts/0a96d23f-6c22-4053-a7f6-83e12ce5a6ec

Harder challenge video output - https://classroom.udacity.com/nanodegrees/nd013/parts/168c60f1-cc92-450a-a91b-e427c326e6a7/modules/5d1efbaa-27d0-4ad5-a67a-48729ccebd9c/lessons/7cb63828-36aa-4cea-9239-700b5ea41f0b/concepts/0a96d23f-6c22-4053-a7f6-83e12ce5a6ec

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The code worked fine for the project video. It worked decently with the challenge video. The most difficult part of this project was tuning the pipeline to fit the harder challenge video. 

The harder challenge video had lots of quick and sharp turns and also sudden change in lights on the lane lines for which I was not able to edit my pipeline. Altering the margin limit might get us a little improvement, Improving the perspective transform will surely help on getting a better output but still I felt more advanced techniques has to be used in the case of the harder challenge to obtain a good output.  