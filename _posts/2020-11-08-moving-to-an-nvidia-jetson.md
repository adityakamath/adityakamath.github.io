---
layout: post
title: Moving to an NVIDIA JetBot with ROS
subtitle: + Testing a RPLidar A2 for another project
gh-repo: adityakamath/ros_jetbot2
gh-badge: [star, fork, follow]
thumbnail-img: /assets/img/jetbot2_build_thumb.jpg
share-img: /assets/img/jetbot2_build_thumb.jpg
tags: [jetbot, jetbot2, robotics, ros, data-acquisition, communication, electronics, software, design, build]
comments: true
---

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/roscar_inhand.png" />
	<figcaption>ROSCar v2 - assembled car from last update</figcaption>
</figure>

#### ROSCar(/Bot) Update

Since the last update, I have spent time doing NVIDIA's [2 Days to a Demo](https://developer.nvidia.com/embedded/twodaystoademo) program that contains a lot of resouces about training, retraining and deploying AI models with some of NVidia's own python notebooks and ROS. I have learnt mainly about classification, object detection and semantic segmentation using a monocular camera. And since I am using ROS, I am now able to integrate any of these AI applications with the rest of the ROSCar platform. This brings me to the JetBot. 

Unfortunately, I am in between apartments right now and the studio I stay in does not have a lot of space. This was making it difficult for me to drive and train my RC car indoors. I decided to move everything to the NVidia Jetbot platform, since I am using almost the same components. The first step to this was to disassemble everything from the ROSCar and reassemble the electronics to verify all the wiring. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/roscar_chassis_removed.jpg" />
	<figcaption>ROSCar payload disassembled by removing only 4 pins</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/roscar_electronics.jpg" />
	<figcaption>ROSCar electronics disassembled and wired for testing</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/jetbot2_assembled_temp.jpg" />
	<figcaption>Assembled ROS JetBot</figcaption>
</figure>

I was able to fit almost everything in, but I did have to use a lot of 'jugaad' i.e, some improvised, make-shift solutions for some of the parts using my handy glue gun and some zipties. I had to solder a hat for the Jetson Nano that provided access to the I2C bus and connected the PiOLED. I also had to find a way to mount the IMU, which does not exist in the original Jetbot. My version of the jetbot also included the Arduino Nano and the NeoPixel LEDs, which also needed to be wired in. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/jetbot2_motorhat.jpg" />
	<figcaption>Adafruit Featherwing MotorHAT</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/jetbot2_motorhat_wired.jpg" />
	<figcaption>Adafruit MotorHAT wired underneath the Jetbot platform</figcaption>
</figure>

The biggest update was the change in the motor control board. I tried to reuse the PCA9685 servo control board, but for that I would need another motor driver to drive the DC motors. I did have a L29DN motor driver around but that did not work with my power bank.  So, I decided on the [Adafruit MotorHAT featherwing version](https://www.adafruit.com/product/2927), which is used in the traditional versions of the JetBot. With this I had an upgraded JetBot ready to test. Most of the software was reused from the ROSCar. The Adafruit MotorHat software was reused from [dusty-nv's JetBot_ROS package on Github](https://github.com/dusty-nv/jetbot_ros). Here are some pictures and videos, I call it the Jetbot2:

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/jetbot2_inhand.jpg" />
	<figcaption>ROS1-powered Jetbot2</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/jetbot2_vs_roscar.jpg" />
	<figcaption>JetBot2 next to the old ROSCar platform</figcaption>
</figure>

[![First test of the Jetbot2](https://adityakamath.github.com/assets/img/jetbot2_first_ss.png)](https://www.youtube.com/watch?v=QPyIpB4Qv88 "First test of the Jetbot2 - Click to Watch!")

The ROS package by dusty-nv also comes with a Gazebo model for a slightly different version of the JetBot. Here's the comparison between the JetBot2 and dusty-nv's JetBot Gazebo simulation. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/jetbot2_vs_gazebo.jpg" />
	<figcaption>Jetbot2 compared with the Jetbot Gazebo Model by dusty-nv</figcaption>
</figure>

This entire experience of rebuilding the ROSCar into an entirely different robot made me appreciate ROS even more. Due to it's modular nature, it took me less than a day to reuse ROSCar software and configure everything to work with the JetBot2. The I2C bus is definitely a boon, allowing compatible sensors to be daisy-chained to a single bus and easily configured with ROS/C++/Python tools and libraries. This was especially useful since I was able to replace the motor driver from the I2C chain without messing with the IMU and the PiOLED also connected in parallel. 

#### Getting a RPLidar

For an unrelated project that I plan on beginning next year, I ordered a RPLidar and a Raspberry Pi 4 4GB. I love that my workplace pays for innovative personal-projects like this, which allowed me to go for a slightly better version of the RPLidar - the A2M8, which costed me around 320 euros from my annual innovation budget. I plan on using this lidar along with an OpenCV Depth AI Kit (OAK-D), which I ordered from Kickstarter and expect early next year. With these two sensors I want to implement indoor SLAM, Localization and Navigation for autonomous robots. I plan on getting some help from a colleague to build the robot platform for me, which of course can reuse ROSCar or JetBot2 packages if needed. However, while this project is expected to begin only next year, I got a chance to play with the lidar and get it running with ROS on the Jetson Nano (on the JetBot2). 

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/rplidar_with_rpi4.jpg" />
	<figcaption>New toys from the office: RPLidar A2M8, RPi 4 4GB, 64GB SD Card</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/rplidar_with_jetbot2.jpg" />
	<figcaption>RPLidar ROS Package running off the Jetbot2's Jetson Nano</figcaption>
</figure>

<insert lidar video>
[![Testing the RPLidar with Slamtec RobotStudio and ROS Melodic](https://adityakamath.github.com/assets/img/testing_rplidar_ss.png)](https://www.youtube.com/watch?v=3pMYaUD-vEk "Testing the RPLidar A2M8 - Click to Watch!")

#### Next Steps

My final steps for the ROSCar/JetBot2 project are to clean up the code, write some scripts for small things like changing the LED colors according to the robot state, or adding a record/pause button the joystick to record and stop rosbags. Finally, I want to build a small application with the completed platform. Hopefully, I have more updates next month. 
