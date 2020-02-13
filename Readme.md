# **Behavioral Cloning Project** 

The goals of this project are the following:
* Use the simulator to collect data of good driving behavior
* Build, a convolution neural network in Keras that predicts steering angles from images
* Train and validate the model with a training and validation set
* Test that the model successfully drives around track one without leaving the road
* Summarize the results with a written report

My project includes the following files:
* model.py containing the script to create and train the model 
* drive.py for driving the car in autonomous mode
* model_1.h5 containing athe first trained convolution neural network 
* model_2.h5 containing a trained convolution neural network with Nvidia architecture
* video.py for making video from autonomous driving of the trained system
* run1.mp4 a video recorded during autonomous driving
* writeup_report.md summarizing the results



## Step 1: Collecting the data using the simulator
Data collection for this project has been done in three stages as follow:
- In the first stage, the car was driven manually two laps around the first track.
- Since the curvature of the track is mostly in the left direction, I also turned the car for 180 degree and drivied in the opposit direction to train the system for the right curvature as well. This was done for one lap around the track. 
- In order to train the system to recover from the left and right sides, several times I intentionally deviated from the center line toward the sides and then recorded the recovery process. 
After the collection process, I had about 10000 images (only center camera). All the images have been saved in IMG folder plus the driving_log.csv including the driving measurements of relevant images.  

## Step 2: Data augmentation
In order to generate more data and also teach the network to back to the center in the case of drifting to the sides, I have also used the images from left and right cameras besides the centeral camera. To do so, I have considered a correction factor for steering measuremnts of images from the left and right cameras. 
At the end, all images and their corresponding steering measurements have been flipped to generate even more data. Accordingly, I got six times more data compared to the recorded data. 


## Step 3: Preprocessing data
Augmented data has been processed in two stages:
- Normalizing the data: The value of each pixel is an integer number between 0 and 255. I have normalized the pixel values in the range of -0.5 and 0.5 with the mean value equals to 0. 
- Trimming the image: Since the upper part of each image mostly include the skys and trees, I have cropped those parts. Also, the bottom part of the image which shows the hood has been cropped.  

**Note:**
With a view to the efficent use of memory, I used the Python generator, to generate data for training rather than storing the training data in memory. 


## Step 4: Building a convolution neural network 
- Model 1: LeNet Architecture
First, I used the LeNet architecture for my convolutional neural network. So, my model consisted of two convolution layers with 5x5 filter sizes, a 2x2 max pooling layer immediately following each of my convolutional layers, and three fully connected layers (size = 128, 84, 1). To introduce nonlinearity, the RELU activation function is used in the convolutional layers. Running the model showed that loss of validation set was much higher than training set. So, I used dropout layers in order to reduce overfitting. However, I found a few spots that car deviates from center line and struggles to recover. Because of that, I tried more complicated arcitecture. 

- Model 2: Nvidia Architecture
In the second model, I used Nvidia model architecture. This model includes five convolutional layers as follow:
- layer 1: Convolution, number of filters- 24, filter size= 5x5, stride= 2x2
- layer 2: Convolution, number of filters- 36, filter size= 5x5, stride= 2x2
- layer 3: Convolution, number of filters- 48, filter size= 5x5, stride= 2x2
- layer 4: Convolution, number of filters- 64, filter size= 3x3, stride= 1x1
- layer 5: Convolution, number of filters- 64, filter size= 3x3, stride= 1x1
- layer 6: fully connected layer, size = 100
- layer 7: fully connected layer, size = 50
- layer 8: fully connected layer, size = 10
- layer 9: fully connected layer, size = 1

To introduce nonlinearity I also included ELU activation function in all layers.ELU is very similar to RELU except negative inputs. They are both in identity function form for non-negative inputs. On the other hand, ELU becomes smooth slowly until its output equal to -α whereas RELU sharply smoothes.
In addition, to avoid overfitting two fully connectedall layers except the last one have dropout. 


## Step 5: Training and validating the model 
In order to gauge how well the model was working, I splitted my data into a training (80%) and validation (20%) set after randomly shuffling data. The mean square error of steering angle was my loss function. My model used an adam optimizer. So, the learning rate was not tuned manually.
Reviewing the mean square error history of first model on training and validation sets reveals that after 5 epochs there is no significant changes in loss. So, 5 epochs is enough to reach the reasonable performance in the first model.  

[image1]: ./examples/loss1.png "Loss Visualization in Model 1"

The second model was trained for 15 epochs. Here, the validation loss oscillates alot. However, it is not that different from the training set and is bounded in a reasonable range.

[image2]: ./examples/loss2.png "Loss Visualization in Model 2"

The results of model 1 and model 2 ware savea in model_1.h5 and model_2.h5 respectively.


## Step 6: Testing the model
The final step was to run the simulator to see how well the car was driving around track one. Using the Udacity provided simulator and my drive.py file, the car can be driven autonomously around the track by executing 
```sh
python drive.py model_2.h5
```
Without data agugmentation, I found a few spots where the vehicle fell off the track. However, at the end of the process and training the system with all augmented data, the vehicle was able to drive autonomously around the track without leaving the road. A video (video.mp4) made of the autonomous driving of the vehicle has been saved in the project folder. 


