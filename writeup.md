
# **Finding Lane Lines on the Road** 

**The purpose of this project is to find lane lines from given images or video streams**

[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./test_images_output/figure-0.png
[image3]: ./test_images_output/figure-1.png
[image4]: ./test_images_output/figure-2.png
[image5]: ./test_images_output/figure-3.png
[image6]: ./test_images_output/figure-4.png
[image7]: ./test_images_output/figure-5.png



### 1. Find lane lines from given images and video streams
**Goal: find lane lines from images**

1. Read images from ./test_images directory and store all images to a list 	
2. Convert all images to grayscales
3. Defined kernel size to 5, then apply gaussian blur on each images
4. Set low threshold to 50 and high threshold to 150, then run Canny function to get edges from each images
5. Form a polygon mask with vertices (0, image_rows),(image_columns/2-10, image_rows/2+50),(image_columns/2+10,image_rows[0]/2+50),(image_columns, image_rows) on each edge images
6. Run Hough Transform on each masked edge images
7. Draw lines on left and right lane lines coresponding to averaged x1, y1, x2, y2 of left line functions and right line functions

**Details of modifying draw_lines functions**
From all images, we can find that the left lane lines are always positive functions, and right lane lines are always negtive functions, Therefore, the slope of left lane lines are always greater than 0, and the slope of right lane lines are always less than 0. After some tuning parameters, I decided to go with the range of 0.3 ~ 0.8 for the left lane lines, and -0.8 ~ -0.3 for right lane lines. Then go through each lines from Hough Transform, divided them to left line group and right line group based on their slopes. Also, I can find the intersection for each lines by b = y -mx, then I average the slopes and intersections for left lane lines and right lane lines. After that, I found the boundray for y1 and y2, y1 will be the lowest y vaule on mask, which is **image columns**, and y2 is the highest y value on mask, which is **image columns / 2 + 60**. Then,  based on y1 y2, I got x1 and x2 respectly for left lane lines and right lane lines. Plug these x1, y1, x2, y2 of left lane lines and right lane lines into cv2.line function to draw the lines

**Output:**
![figure-0][image2]
![figure-1][image3]
![figure-2][image4]
![figure-3][image5]
![figure-4][image6]
![figure-5][image7]


### 2. Potential shortcomings
1. if there are some obvious shade on the image even more obvious then the lane, then after apply my pipelines, the lines will locate on the edge of shade rather than the lane lines. 
2. if the slopes of the lane lines are vertical, or all positive/negtive, then my pipeline is not able to draw the lines on lanes.


### 3. Suggest possible improvements to your pipeline
1. Apply a proper mask to hide the obvious shade then it will locate the lane lines.
2. Trying to find the middle point of the road ( between left lane lines and right lane lines). If x1, x2 on the left of the middle point, then it is left lane lines, if x1, x2 on the right of the middle point, then it is right lane lines. if cross the middle lines, we just ingore. This should also be able to draw the lines on lanes.
