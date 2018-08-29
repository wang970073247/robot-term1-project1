
## Project: Search and Sample Return

---
[//]: # (Image References)

[image1]: ./misc/rover_image.jpg
[image2]: ./calibration_images/example_grid1.jpg
[image3]: ./calibration_images/example_rock1.jpg 
[image4]: ./calibration_images/rover_nav.png
![alt text][image1]

**The goals / steps of this project are the following:**  

**Training / Calibration**  

* Download the simulator and take data in "Training Mode"
* Test out the functions in the Jupyter Notebook provided
* Add functions to detect obstacles and samples of interest (golden rocks)
* Fill in the `process_image()` function with the appropriate image processing steps (perspective transform, color threshold etc.) to get from raw images to a map.  The `output_image` you create in this step should demonstrate that your mapping pipeline works.
* Use `moviepy` to process the images in your saved dataset with the `process_image()` function.  Include the video you produce as part of your submission.

**Autonomous Navigation / Mapping**

* Fill in the `perception_step()` function within the `perception.py` script with the appropriate image processing functions to create a map and update `Rover()` data (similar to what you did with `process_image()` in the notebook). 
* Fill in the `decision_step()` function within the `decision.py` script with conditional statements that take into consideration the outputs of the `perception_step()` in deciding how to issue throttle, brake and steering commands. 
* Iterate on your perception and decision function until your rover does a reasonable (need to define metric) job of navigating and mapping.  

## [Rubric](https://review.udacity.com/#!/rubrics/916/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup

#### 1. Provide a Writeup / README that includes all the rubric points. 
You're reading it!

### Notebook Analysis
#### 1. Run the functions provided in the notebook on test images (first with the test data provided, next on data you have recorded). Add/modify functions to allow for color selection of obstacles and rock samples.
To detect navigable areas, I used the default color threshold of > rgb(160, 160, 160), which worked well in tests. Obstacles are calculated by taking the opposite of the navigable area and applying the mask of the warped image to only include negated parts in the field of view.


#### 2. Populate the `process_image()` function with the appropriate analysis steps to map pixels identifying navigable terrain, obstacles and rock samples into a worldmap.  Run `process_image()` on your test data using the `moviepy` functions provided to create video output of your result. 
The process_image() function is relatively straightforward, as the only remaining steps after filtering out the navigable terrain, obstacles, and samples are to convert the filtered image to rover coordinates and convert those coordinates to world coordinates based on the robot's pose. I then use these coordinates to update the world map by bumping the color values for each type of coordinate by one.

![alt text][image2]
### Autonomous Navigation and Mapping

#### 1. Fill in the `perception_step()` (at the bottom of the `perception.py` script) and `decision_step()` (in `decision.py`) functions in the autonomous mapping scripts and an explanation is provided in the writeup of how and why these functions were modified as they were.

##### 1. Perception
The main step in perception is to analyse the rover's image.
1. An image of the path is analysed. 
2. 4 points are taken, consider like a grid, and it is warped. Image warping is the process of digitally manipulating an image such that any shapes portrayed in the image have been significantly distorted [Wikipedia]
3. The image is warped as if the image is taken from above, using a size of 10x10 with an offset of 6 (to accommodate the size of the robot). 
4. The warped image is filtered by applying a threshold for 3 scenarios: navigable route, obstacle and rock
5. Filtered images are rotated to fit the rover coordinates, with positive x at the bottom going right and y going up
6. As the current images represent one portion of the map, a pix_to_world function is used to  fit the part of the image onto the world map
7. The worldmap is 10 times the size of the map (an assumption), a 1/10 scale is used
8. The worldmap is overlayed wtih different color for obstacle, navigable and rock for inspection purposes

##### 1. Decision
just as the origin decision.


#### 2. Launching in autonomous mode your rover can navigate and map autonomously.  Explain your results and how you might improve them in your writeup.  

**Note: running the simulator with different choices of resolution and graphics quality may produce different results, particularly on different machines!  Make a note of your simulator settings (resolution and graphics quality set on launch) and frames per second (FPS output to terminal by `drive_rover.py`) in your writeup when you submit the project so your reviewer can reproduce your results.**

ran the simulator at a resolution of 800x600 with fatest graphics quality.

The rover completes all the tasks of the assignment except returning samples to the initial starting position. It is able to map almost all of the terrain with relatively high fidelity . The rover is able to move quite fast, since it can adapt its speed depending on how much open terrain is visible in front of it. Due to simulator issues with the camera occasionally seeing through obstacles, the rover sometimes gets stuck, thinking that it can move forward even if it is against a rock. In the future, this can be fixed by detecting when the position has not changed for some time. Another part to improve would be the wall following, which sometimes fails to detect side corners especially if the rover is moving fast.


![alt text][image4]

