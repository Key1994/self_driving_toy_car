# How to build a self-driving toy car?


Project Objectives:
=====
    1.Hardware. Design the structure of the car, and realize it with hands-on practice.  
    2. Algorithms. Explore efficient perception algorithms to process the data collected from sensors.  
    3. Software. Write programs to implement the algorithms and output controlling commands.  
    4. Test. Drive the car with these controlling commands.  


Hardware structure.
=====
As shown in Fig 1, the car consists of four modules: perception module, processing module, battery module and execution module.  
The perception module collects environment information through a camera and a untrasonic sensor. The raspberry Pi in processing module processes the collected data and outputs controlling orders to execution module. Then, the four motors could drive the car according to these orders to realize the automation. All of the devices are powered by the battery module.  
Fig 1. The hardware architecture of the car.  
![Hardware architecture](https://github.com/Key1994/self_driving_toy_car/blob/master/Hardware%20architecture.png)

With four installed motors, the car can:  
        Go forward and backward.  
        Turn left and turn right by the differential of left and right wheels.  
        Change the spped through pulse width modulation (PWM) control.  


Algorithms.
=====
1. Steering angle predict based on Canny operator.  
_____
>> Image collected by the camera.  
>> Edge extraction by Canny operator.  
>> Define the region of interest (ROI).  
>> Hough transform to find the lane lines.  
>> Transform the image from perspective to birdview.  
>> Calibrate the straight and curved lines with a quadratic polynomial.  
>> Calculate the offset to the center of the lane, and output the expected steering direction.  
Based on the steps above, the car is able to drive along the lane lines, even the lane with sharp corner.  
However, it is difficult for the raspberry Pi to predict the real-time steering direction since the large computation. Just as shown in the above video, the car takes a relatively long time to obtain the controlling order, hence it goes with a low speed and many stucks. On the other hand, it is pretty hard to keep the car driving between the lane lines exactly due to the error of prediction. The results suggest that the accuracy of the steering direction prediction remains at around 80%. All in all, the steering direction prediction technique based on Canny operator is available but not satisfactory.  

2. Steering angle predict based on convolutional neural network.
_____

