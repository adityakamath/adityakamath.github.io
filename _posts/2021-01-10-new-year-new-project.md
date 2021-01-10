---
layout: post
title: New year, new project
subtitle: Looking back at 2020 + plans for 2021
gh-repo: adityakamath/akros_jetson
thumbnail-img: /assets/img/akros_holonomic_thumb.jpg
share-img: /assets/img/akros_holonomic_thumb.jpg
gh-badge: [star, fork, follow]
tags: [jetbot2, akros, robotics, electronics, software, holonomic, planning]
comments: true
---

#### Looking back at 2020

2020 has been quite a different year. I has just started my new job as a consultant, and my first assignment at a client, when the Netherlands went into lockdown. My mid-march, I was completely working from home and was left with a lot of spare time. I had already started to build the ROSCar platform, and with the lockdown, I decided to dig into that project during the spare hours indoors. I gave myself the added goal of using ROS1 from scratch, instead of using the software provided by NVidia and from similar platforms like the DonkeyCar. Meanwhile, I also decided to start this blog mainly to log my work but it soon evolved into something I can potentially share with colleagues, collaborators or employers. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/roscar_update_final.jpg" />
	<figcaption>The ROSCar/Jetracer2 platform</figcaption>
</figure>

By November, the ROSCar project was almost at the end. I was able to use ROS on the NVidia Jetson Nano to run NVidia's Deep Learning examples, and then control the robot accordingly. However, while testing, I realized that the RC car was probably too big for my studio. The turning radius was too big, which made it really difficult to make complete turns (programming an autonomous 3 point turn was not something I planned on getting into, yet). I decided to purchase NVidia/Adafruit's Jetbot kit (v1, I believe) and moved the electronics from the RC car platform to this new smaller, differential drive robot. Once that was done, I had to change a small piece of software to drive the differential robot. By the end, I had this:

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/jetbot2_finaltest.jpg" />
	<figcaption>The Jetbot2 platform - based on the NVidia Jetbot with an IMU, Arduino Nano and status LEDs</figcaption>
</figure>

I used this robot platform to capture images, trained a neural network using NVidia's resources and finally developed a edge avoidance demo running completely with ROS Melodic. The results can be seen here:

[![Testing drive modes on the Jetbot2](https://adityakamath.github.com/assets/img/jetbot2_driving_modes_ss.png)](https://www.youtube.com/watch?v=-XmJxytU4ko "[Testing drive modes on the Jetbot2 - Click to Watch!")

I hope to clean up my code and write documentation for it soon, so that others can reproduce it. I think the Jetbot is really handy tool for anybody getting into Computer Vision and Deep Learning because NVidia provides a lot of resources regarding this. However, for aspiring roboticists, getting started with ROS is equally as important, so ROS powered Jetbot2 could be a great place to start. I also designed my system such that the same electronics could be used on an RC car, with only high-level configuration changes in the software. In 2021, one of my goals is to improve the modularity of the system, so that it can work with differential, ackermann and different holonomic drive systems. Until I find a better name, I call it the AKROS project (simply my initials + ROS).

#### Plans for 2021:

For 2021, I decided to stick with the hobby and build a more complex system. In 2020, I backed the OpenCV AI Kit (OAK) on Kickstarter and purchased an OAK-D, which is a stereo camera and a monocular camera with on-board monocular and spatial AI. Also, thanks to the 'innovation budget' my employer's provide, I was able to purchase new components like a Lidar scanner and a Raspberry Pi 4. I had purchased a cheap holonomic platform with 4 motors (with encoders) and omniwheels earlier in 2020, which I also decided to use. During the christmas holidays, I managed to assemble together a platform with all the electronics and some custom acrylic parts. I am still waiting for the OAK-D camera to be delivered, but here's a preview of what the robot looks like:

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/akros_differential_holonomic.jpg" />
	<figcaption>The AKROS Differential robot alongside the Holonomic platform without the OAK-D camera. The cardboard parts are only prototypes/placeholders while I get acrylic parts.</figcaption>
</figure>

In 2021, I have 3 big plans for this robot: 

* I want to explore ROS2 as an option for this holonomic platform. It has a lot of advantages over ROS1 and I really want to learn more about DDSs (Data Distribution Services) and microROS (ROS for microcontrollers)
* Secondly, I want to go back to my roots of mechatronics/control systems and design a robust controller for this holonomic system. I intend to add this to the AKROS platform, with the differential and ackermann drive systems (these are ROS1 packages, so I will use the ROS1-ROS2 bridge). 
* Finally, I want to design and build a universal SLAM/navigation package with the Lidar and the OAK-D camera, using Nav2 (the ROS2 Navigation Stack) on the Raspberry Pi 4. This is something I expect to reuse in different robots in the future.

I certainly don't expect to achieve all these goals, especially since I hope to get vaccinated against Covid-19 this year and hopefully the travel restrictions are eased and I can make up for not going to India last year. However, since I'll be working on all three in parallel, hopefully there is some progress on each topic by the end of 2021.
