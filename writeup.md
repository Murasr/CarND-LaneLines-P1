
**Finding Lane Lines on the Road**

The goals of this project are the following:
* Make a pipeline that finds lane lines on the road



### Reflection

### Image Pipeline

The steps followed in lane finding pipeline are as follows:
1) Convert Color to gray scale image

2) Do smoothening by applying gaussian kernel of size 5

3) Apply canny edge with lower threshold as 50 and upper threshold as  150.
   Canny edge Output for test images can be found at ./test_images_output/canny_out_\*.jpg
   
4) Apply a polygon ROI to the canny output. The four vertices are choosen by going through all images and choosing an appropriate proportion
   vertices = np.array([[(imshape[1]*0.12,imshape[0]),(imshape[1]*0.48,imshape[0]*0.60), (imshape[1]*0.53,imshape[0]*0.60), (imshape[1]*0.95,imshape[0])]], dtype=np.int32)
   ROI Output for test images can be found at ./test_images_output/roi_out_\*.jpg
   
5) Apply hough transform with following parameters:
    rho = 2           # rho grid is given 2
    theta = 1
    threshold = 15    # minimum 15 intersections is requied
    min_line_len = 5  # discards line segments with length lesser than 5
    max_line_gap = 1  # minimum gap to connect segments is 1
    
    Hough transform output for test images can be found at ./test_images_output/hough_lines_out_\*.jpg
    
6) From hough line output, seggregate lines with positive and negative slopes.

7) Find the top most and bottom most points of all lines with negative slope.
   The top most and bottom most points form the left lane.
   
8) Find the top most and bottom most points of all lines with positive slope
   The top most and bottom most points form the right lane.
   
9) Now extrapolate the left lane and right lane to the Y boundaries of ROI (Top: imshape[0]*0.60, Bottom: imshape[0])
   It is done by the method "extrapolate_to_y(line, y_value)" which returns intersection point of line extrapolated to line y=y_value
   
Test Images Output:
./test_images_output/canny_out_\*.jpg       --> Output after applying canny edge detection
./test_images_output/roi_out_\*.jpg         --> Output after applying ROI to canny output
./test_images_output/hough_lines_out_\*.jpg --> Output after applying Hough transform
./test_images_output/final_out_\*.jpg       --> Final output after extrapolating the hough output
   
Input Files: 
./test_videos/SolidWhiteRight.mp4
./test_videos/SolidYellowLeft.mp4

Output Files:
./test_videos_output/SolidWhiteRight.mp4
./test_videos_output/SolidYellowLeft.mp4


### Shortcomings

1) The extrapolation should be improved because the lane output is always straight line. Curvature is not taken into account
2) The Width of the lane is not properly calculated
3) At some instances, the lanes deviate too much
4) Lanes at the very far distance are not properly detected


### Suggestions to improve 

1) Consider connecting individual hough lines instead of calculating top most and bottom most point

