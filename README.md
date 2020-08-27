# self_driving_toy_car
How to build a self-driving toy car?

Project Objectives:
    1. Hardware. Design the structure of the car, and realize it with hands-on practice. 
    2. Algorithms. Explore efficient perception algorithms to process the data collected from sensors.
    3. Software. Write programs to implement the algorithms and output controlling commands.
    4. Test. Drive the car with these controlling commands.


Hardware structure.
As shown in Fig 1, the car consists of four modules: perception module, processing module, battery module and execution module.
The perception module collects environment information through a camera and a untrasonic sensor. The raspberry Pi in processing module processes the collected data and outputs controlling orders to execution module. Then, the four motors could drive the car according to these orders to realize the automation. All of the devices are powered by the battery module.
