---

**Vehicle Detection Project**

[//]: # (Image References)
[image1]: ./output_images/hog.png
[image2]: ./output_images/hog_on_car.png
[image3]: ./output_images/befor_heatmap.png
[image4]: ./output_images/half_image.png
[image8]: ./output_images/boxed_car.png
[image5]: ./output_images/heatmap.png
[image6]: ./output_images/heatmap_label.png
[image7]: ./output_images/image_found.png
[video1]: ./project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

#### 1. Histogram of Oriented Gradients (HOG)

I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:
I have used `YUV` color space and HOG parameters of `orientations=8`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`. Here is an example:

non-vehicle
![alt text][image1]

vehicle
![alt text][image2]

#### 2. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

1. I trained a linear SVM using sklearn library and also used StandardScalar transformation function for normalization the features set.
2. I have used only the Hog transformation feature only with all 3-channel of the image.
 a) For each channel I have used orient=9, pix_per_cell=8, cell_per_block=2 
 b) Combined all the channel and made one feature vector.
3. Divide the training and testing sets 80% and 20% respectively
4. Trained the classifier.
5. Accuracy 98% on the testing training data set

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I cropped the image at Y-axis, the original image size was (0-720, 0-1280) but I focused on (400-660, 0-1280) size only. Also I have used various scale(size) of the boundry box as suggested in the course, So it could help me to identify the cars on the image based on how farthe apart.

![alt text][image3]



#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

I searched on two scales using YUV and all 3-channel HOG features, which provided a nice result. However I did not use color and bin-spatial transformation features.

An half image focused:
![alt text][image4]

Predicted car image on full image with boundry box:
![alt text][image8]
---

### Video Implementation

#### 1. Provided a link of my final video output. 
Here's a [link to my video result](./project_video.mp4)


#### 2. Implementation of the filter for false positives and the method for combining overlapping bounding boxes.

1. I used the SVC classifier to classify the each frame of the video, and draw the rectangle boxes on the image.
2. The positive detections, I created a heatmap and then thresholded that map to identify vehicle positions using "label" function of scipy library.
3. Using the return value of label function calculated the top-left and bottom-right points.


Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

### Here are six frames and their corresponding heatmaps:

![alt text][image5]

### Here is the output of `scipy.ndimage.measurements.label()` on the integrated heatmap from all six frames:
![alt text][image6]

### Here the resulting bounding boxes are drawn onto the last frame in the series:
![alt text][image7]



---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I have not used color and bin-spatial features set in order to find the car on an image. Although, these are the good features to explore but could not find why it is necessary as hog tranformatin function is working find with 98% accuracy.

I learnt the color conversion significance for the object detection as I was working on this project and forgot to convert the color of the image my Classifier was not working well. 

