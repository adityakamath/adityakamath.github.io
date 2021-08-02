---
layout: post
title: New year, new project
subtitle: Looking back at 2020 + plans for 2021
gh-repo: adityakamath/akros_jetson
thumbnail-img: /assets/img/akros_holonomic_thumb.jpg
share-img: /assets/img/akros_holonomic_thumb.jpg
gh-badge: [follow]
tags: [jetbot2, akros, robotics, electronics, software, holonomic, planning]
comments: true
---

Looking back, 2020 has been a really weird year. I had just started my new job as a consultant, and my first assignment at a client, when the Netherlands went into lockdown. By mid-march, I was completely working from home and was left with a lot of spare time. I had already started to build the ROSCar platform, and with the lockdown, I decided to dig into that project during the spare hours indoors. I gave myself the added goal of using ROS1 from scratch, instead of using the software provided by NVidia and from similar platforms like the DonkeyCar. Meanwhile, I also decided to start this blog mainly to log my work but it soon evolved into something I can potentially share with colleagues, collaborators or employers. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/roscar_update_final.jpg" />
	<figcaption>The ROSCar/Jetracer2 platform</figcaption>
</figure>

By November, the ROSCar project was almost done. I was able to use ROS on the NVidia Jetson Nano to run NVidia's Deep Learning examples, and then control the robot accordingly. However, while testing, I realized that the RC car was probably too big for my studio. The turning radius was too big, which made it really difficult to make complete turns (programming an autonomous 3 point turn was not something I planned on getting into, yet). I decided to purchase NVidia/Adafruit's Jetbot kit (v1, I believe) and moved the electronics from the RC car platform to this new smaller, differential drive robot. Once that was done, I only had to change a small piece of software from ackermann to differential steering. By the end, I had this:

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/jetbot2_finaltest.jpg" />
	<figcaption>The Jetbot2 platform - based on the NVidia Jetbot with an IMU, Arduino Nano and status LEDs</figcaption>
</figure>

I used this robot platform to capture images, trained a neural network using NVidia's resources and finally developed a edge avoidance demo running completely with ROS Melodic. The results can be seen here:

[![Edge avoidance on the Jetbot2](https://adityakamath.github.io/assets/img/jetbot2_edge_avoidance_ss.png)](https://www.youtube.com/watch?v=9J5rK8DWGGw "Edge avoidance on the Jetbot2 - Click to Watch!")

I hope to clean up my code and write documentation for it soon, so that others can reproduce it. I think the Jetbot is really handy tool for anybody getting into Computer Vision and Deep Learning because NVidia provides a lot of resources regarding this. However, for aspiring roboticists, getting started with ROS is equally as important, so the ROS powered Jetbot2 could be a great place to start. I also designed my system such that the same electronics could be used on an RC car, with only high-level configuration changes in the software. In 2021, one of my goals is to improve the modularity of the system, so that it can work with differential, ackermann and different holonomic drive systems. Until I find a better name, I call it the AKROS project (simply my initials + ROS).

#### Plans for 2021:

For 2021, I decided to stick with the hobby and build a more complex system. In mid-2020, I had backed the OpenCV AI Kit (OAK) on Kickstarter and purchased an OAK-D, which is a stereo camera and a monocular camera with on-board monocular and spatial AI. Also, thanks to the 'innovation budget' my employer provides, I was able to purchase new components like a LiDAR scanner and a Raspberry Pi 4. I had also purchased a cheap holonomic platform with 4 motors (+ encoders) and omniwheels earlier in 2020, which I decided to use for this project. During the christmas holidays, I managed to assemble together a platform with all the electronics and some custom acrylic parts. I am still waiting for the OAK-D camera to be delivered, but here's a preview of what the robot looks like:

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_differential_holonomic.jpg" />
	<figcaption>The AKROS Differential robot alongside the Holonomic platform without the OAK-D camera. The cardboard parts are only prototypes/placeholders while I get acrylic parts.</figcaption>
</figure>

In 2021, I have a few big plans for this robot: 

* I want to explore ROS2 as an option for this holonomic platform. It has a lot of advantages over ROS1 and I really want to learn more about DDSs (Data Distribution Services) and microROS (ROS for microcontrollers)
* Firstly, I want to design and build a universal SLAM/navigation package with the Lidar and the OAK-D camera, using Nav2 (the ROS2 Navigation Stack) on the Raspberry Pi 4. This is something I expect to reuse in different robots in the future.
* Parallelly, I also want to work on a positioning system using UWB for indoor use, GPS for outdoor use and an IMU for correction. I plan on designing and fabricating my own PCB for this process, using the Teensy 4.0 (and potentially microROS)
* Finally, a robust controller for this holonomic system. I intend to add this to the AKROS platform, with the differential and ackermann drive systems (these are ROS1 packages, so I will use the ROS1-ROS2 bridge). 

I also want to find a collaborator who could take over the motor control task since the SLAM package and the PCB would take most of my time. That being said, I certainly don't expect to finish these goals 100%, especially since I hope to get vaccinated against Covid-19 this year and find some time to travel. However, with two people and three projects running in parallel, hopefully there is some progress on each topic by the end of 2021.
