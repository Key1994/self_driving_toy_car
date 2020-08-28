# How to build a self-driving toy car?


Project Objectives:
=====
    1. Hardware. Design the structure of the car, and realize it with hands-on practice.  
    2. Algorithms. Explore efficient perception algorithms to process the data collected from sensors.  
    3. Software. Write programs to implement the algorithms and output controlling commands.  
    4. Test. Drive the car with these controlling commands.  


Hardware structure.
=====
As shown in Fig 1, the car consists of four modules: perception module, processing module, battery module and execution module.  
The perception module collects environment information through a camera and a untrasonic sensor. The raspberry Pi in processing module processes the collected data and outputs controlling orders to execution module. Then, the four motors could drive the car according to these orders to realize the automation. All of the devices are powered by the battery module.  
Fig 1. The hardware architecture of the car.  
![Hardware architecture](https://github.com/Key1994/self_driving_toy_car/blob/master/Graphs/Hardware%20architecture.png)

With four installed motors, the car can:  
    1. Go forward and backward.  
    2. Turn left and turn right by the differential of left and right wheels.  
    3. Change the spped through pulse width modulation (PWM) control.  


Algorithms.
=====
1. Steering angle predict based on Canny operator.  
_____
* Image collected by the camera.  
* Edge extraction by Canny operator.  
* Define the region of interest (ROI).  
* Hough transform to find the lane lines.  
* Transform the image from perspective to birdview.  
* Calibrate the straight and curved lines with a quadratic polynomial.  
* Calculate the offset to the center of the lane, and output the expected steering direction.  
Based on the steps above, the car is able to drive along the lane lines, even the lane with sharp corner.  
However, it is difficult for the raspberry Pi to predict the real-time steering direction since the large computation. Just as shown in the above video, the car takes a relatively long time to obtain the controlling order, hence it goes with a low speed and many stucks. On the other hand, it is pretty hard to keep the car driving between the lane lines exactly due to the error of prediction. The results suggest that the accuracy of the steering direction prediction remains at around 80%. All in all, the steering direction prediction technique based on Canny operator is available but not satisfactory.  

2. Steering angle predict based on convolutional neural network.
_____
Alternatively, I tried to apply a convolutional neural network to predict the steering angle. Compared to the method based on Canny operator, the CNN loads the images and outputs the predictions directly. Even so, the large computation of convolutional operation is still a challenge for raspberry Pi. Here, I established a simple CNN with the following architecture.  
To train the CNN, I build a training set with 5000 images collected by the camera, and labelled them manually. The trainging was implemented with TensorFlow. The cross entropy was applied as the loss function to evaluated the training results. With the increase of training epoches, the value of loss function decreases gradually. Then, the trained network was tested on a new dataset with 500 images and the prediction accuracy was recorded, as shown in Fig 2.
Fig 2. The loss and accuracy of the CNN under different training epoches.  

Obviously, the CNN trained with 30 epoches gets a sastifactory accuracy. Next, I rewrote the program on the raspberry Pi and load the trained model to drive the car. I expected that the test goes well since the high accuracy the CNN have got on the test dataset. Unfortunately, the car always went off the lane lines and I am trying to find the reasons to solve this problem.   
