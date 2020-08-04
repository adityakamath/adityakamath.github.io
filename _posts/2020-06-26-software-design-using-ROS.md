---
layout: post
title: Software design using ROS1
subtitle: Implementing sensor data acquisition and actuator control
gh-repo: adityakamath/ros1bot
gh-badge: [star, fork, follow]
thumbnail-img: /assets/img/ros_melodic_thumb.png
share-img: /assets/img/ros_melodic_thumb.png
tags: [roscar, ros, data-acquisition, electronics, software, design]
comments: true
---

> The Robot Operating System (ROS) is an open-source, meta-operating system for your robot. It provides the services you would expect from an operating system, including hardware abstraction, low-level device control, implementation of commonly-used functionality, message-passing between processes, and package management. [[ROS Wiki](http://wiki.ros.org/Documentation)]

ROS, however is not really an operating system, but can be instead classified as *middleware* that works on operating systems like Ubuntu, and now recently, also Windows. This means that ROS essentially acts as a middle-man or an interface between multiple computational processes. ROS also hosts a *HUGE* library of community-developed open-source packages. This is a great advantage for roboticists as many standard robotics algorithms/processes/strategies are avaialble as open source packages and you no longer need to re-invent the wheel for common applications. ROS also provides a great deal of other tools and utilities such as simulation environments, debugging tools, logging sensor measurements and data analysis. 
	
ROS its not only extensively used in research but also in the [industry](https://www.robotics.org/content-detail.cfm/Industrial-Robotics-Industry-Insights/ROS-Industrial-for-Real-World-Solutions/content_id/7919). Which is why I was surprised when none of the 'open-source' self-driving robot kits (Donkey Car, NVidia JetRacer, Amazon DeepRacer) used ROS but instead their own software. So, I decided to build a ROS-based self-driving car myself. I decided to use ROS1 for this project, as I already had previous experience with it. [ROS2](https://index.ros.org/doc/ros2/) has only recently been released and I'm currently in the process of learning it. 

### Design and Implementation
The ROS communication infrstructure uses a pub/sub architecure, which allows computational processes (called Nodes) to publish or subscribe to pre-defined data types (called Messages) from pre-defined communication channels (called Topics). Nodes can be publishers (like an odometry node that publishes odometry sensor measurements), or subscribers (like a motor controller node that subscribes to control inputs) or both (like a sensor fusion node that subscribes to multiple sensors and publishes a filtered sensor measurement)

<pub/sub image>

Since the last blog, I have built and tested the following nodes:

#### Motor Calibration and Control
For motor control, I use the PCA9685 I2C servo control board, which communicates with the Jetson Nano using the I2C protocol. I found a handy package built by Bradan Lane called 
[ros-i2cpwmboard](https://gitlab.com/bradanlane/ros-i2cpwmboard/-/tree/447d86954565a8516fb2c1200521ee0b0a2e66a1). This package is written in C++ and communicates ROS messages to the I2C board. This package subscribes to PWM data and communicates these signals to the board and then to the motors. 

On top of this, I wrote a low_level_controller node in Python that subscribes to standard velocity commands from other nodes and converts that to PWM data which is then published. For this computation, I needed to calibrate the throttle and steering to measure the center values, which was then stored in a YAML file. The low level control node reads this configuration file and uses the calibrated values stored in it. 

<motor control image>

I mean to automate this calibration step so that I can use it as a utility for other cars I might build in the future. But I'll leave to later.  

#### Joystick/Keyboard Teleop Controller
Now that the motor control is done, I need to provide a way to give it some velocity commands. For starters, I used the [teleop_twist_keyboard](http://wiki.ros.org/teleop_twist_keyboard) package, which is a standard ROS package to provide velocity commands from your keyboard. I used this to test the motor controller and once that was working fine, I moved on to configuring the Sixaxis joystick. 

For the joystick, I used the (joy)[http://wiki.ros.org/joy/Tutorials/ConfiguringALinuxJoystick] package and its tutorials based on C++. Since I have a 3rd party controller, I had to map the buttons to the package. This mapping was again stored in a YAML file and the implemented node was modified to read the config file and use its values. 

<joystick control image>

#### IMU Calibration + Data Acquisition
Once again I found a handy library for the IMU calibration and data acquisition. However, this one is not based on ROS. The package is called the [RTIMULib](https://github.com/jetsonhacks/RTIMULib) and it is very efficient C++/Python library for off-the-shelf IMUs. RTIMULib works with the IMU I'm using - MPU6050, so I could directly use this library. To calibrate the IMU measurements, I first used the library and its included Qt based GUI to automatically calibrate and store the calibration results in a .ini file. The RTIMULib was then included in a C++ program, and the calibrated sensor values were used to measure angular velocities and accelerations. A ROS publisher was then implemented to publish these IMU measurements to the rest of the ROS system. 

<imu daq image>
	
I have to be honest, I've never implemented or configured an IMU this quickly in my previous projects. The RTIMULib library is _that_ amazing. 

#### Odometry
For the Odometry, I have a ring with seven magnets on the main shaft of the vehicle. These sensors are detected by a hall-effect sensor attached to the base of the car. The hall-effect sensor works on a 5V logic level while the Jetson Nano GPIO only provides 3.3V. I did not have a logic level cconverter lying around, so I decided to use an Arduino Nano for this purpose. I was meant to use an Arduino Nano anyway, to implement start/stop buttons and notification lights, so I did not have to make many changes to the hardware setup. The sensor measurements are read by the Arduino Nano, which then computes the number of rotations of the shaft and sends its via serial protocol over USB. I use the [rosserial](http://wiki.ros.org/rosserial) package to package and communicate the data between Arduino and ROS over serial. 

I haven't gotten around to implement/test this node completely, so I will be updating about this in a future blog. 

#### Remote POV

<remote pov image>

### Tools and Utilities

#### RViz/WebViz

#### Launch Files / Launching at startup

### Next Steps
