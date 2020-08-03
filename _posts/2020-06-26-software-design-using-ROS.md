---
layout: post
title: Software design using ROS1
subtitle: Implementing sensor data acquisition and actuator control
gh-repo: adityakamath/ros1bot
gh-badge: [star, fork, follow]
thumbnail-img: /assets/img/ros_melodic_thumb.png
share-img: /assets/img/ros_melodic_thumb.png
tags: [roscar, data-acquisition, software, design]
comments: true
---

### The Robot Operating System (ROS)

### Architecture and Functionality Overview

#### Motor Calibration and Control

#### Joystick/Keyboard Teleop Controller

#### PiOLED Display

#### IMU Calibration + Data Acquisition

#### Odometry

#### Remote POV

### Tools and Utilities

#### RViz/WebViz

#### Gazebo

#### Launch Files / Launching at startup

### Next Steps


#### RedGear USB Gamepad with the receiver connected to RPi via USB and attached under the base-plate:

The [recommended method in the documentation](https://github.com/autorope/donkeypart_ps3_controller) is to use a standard Sony PS3/PS4 or Xbox controller connected via Bluetooth. More details can be found [here](https://github.com/autorope/donkeypart_ps3_controller). This method allows the RPi to connect to the Bluetooth controller and then read six-axis data at _/dev/inputs/js0_. However, I had a 3rd party wireless controller [(RedGear Wireless Controller for PS2/PS3/PC)](https://www.amazon.in/Redgear-Wireless-Controller-PS2-PS3/dp/B01B7SJUOA) that sends data to _/dev/inputs/js0_ without any additional installation. However, the receiver is big and I had to attach it under the base-plate using zip-ties. I need to map the receiver buttons to the car controls, I will write more about this in the next post.

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/donkeycar_receiver_ziptied.png" />
	<figcaption>RedGear wireless receiver zip-tied under the base-plate (blue). This is connected via USB to the RPi. The position of the servo driver (orange) has now been moved.</figcaption>
</figure>


> **1\. Adding an IMU to the car body:** Since there is no method of inertial odometry at the moment, I plan on adding an [MPU 6050 IMU](https://docs.donkeycar.com/parts/imu/) to the car body. I have currently ordered it from Amazon Germany and its on its way. I still haven't exactly decided the exact position I will be attaching the IMU. I will be writing a complete post about the IMU, more updates then.
