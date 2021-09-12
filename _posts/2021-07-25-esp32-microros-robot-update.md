---
layout: post
title: ESP32 MicroROS robot update
subtitle: Experimenting with different ESP32 kits
gh-repo: adityakamath/akros_micro_test
thumbnail-img: /assets/img/akros_ttgo_feather_thumb.jpg
share-img: /assets/img/akros_ttgo_feather_thumb.jpg
gh-badge: [follow]
comments: true
---

Since my last update, I haven't really worked on the ESP32 robot other than just driving it around my room. However, I've been thinking and sketching different ideas for the eventual PCB and 3D printed chassis. I wanted the robot to be 4WD, but also low cost, and compact. For the first step, I decided to make requirements for a PCB. To start off with, I decided on using the ESP32 instead of the teensy because of the ESP's Wifi and Bluetooth capabilities. However, in last post I mentioned that there weren't enough pins on the ESP-32. I needed 12 PWM pins (3 pins per motor) to drive the motors and 8 interrupt pins (2 pins per motor) to read the encoders. This won't leave any spare IO pins for peripherals.

However, if I run the motors using I2C, I would need only 4 cables from the ESP32. So, I decided to use the Adafruit DC Motor Featherwing. I salvaged it out of the Jetbot2 (which was gathering dust in a corner) and soldered it alongside an ESP32 development board called TT-GO. Its an ESP-32 board with an onboard OLED screen. I was looking for an ESP32 board with a screen so, this at least ticks one box. I added an MPU6050 IMU on the same I2C bus and voila, I had the same platform as last post, in a much more compact form. Here are some photos:

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_ttgo_featherwing.jpg" />
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_micro_ttgo.jpg" />
	<figcaption>TT-GO development board with an Adafruit DC Motor featherwing. Second panel has an MPU-6050 IMU (motors will be soldered on to this panel)</figcaption>
</figure>

This is when I realized that all my efforts were for nothing. I found <this> blog post about using the M5Stack Core with micro-ROS. The Core is a stackable ESP32 kit that comes with an in-built IMU, screen and 3 buttons. I stacked on a 4 channel DC motor module (this module also reads quadrature encoder data) to the core, and within seconds, I had a third variation of the same kit. Here is a comparison picture of the three variations of the micro-ROS/ESP-32/4WD motor control module:

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_micro_ttgo_m5stack.jpg" />
	<figcaption>The ESP32 4WD robot in comparison with the TT-GO assembly and the M5Stack Core (with the 4 channel DC motor module)</figcaption>
</figure>

My next step is to start playing with micro-ROS, do some tutorials, examples and build my own applications. I want to try it out on all three platforms. As of now, I know micro-ROS works on the ESP32 Dev Kit C. I'm still unsure about the TT-GO, while there are quite a few articles about microROS and the M5 Stack Core (although these are in Japanese). Meanwhile, I'm also starting to design an enclosure for the electronics, battery, 4 motors and their encoders. So, more progress about this soon. Meanwhile, I've updated the design of the AKROS holonomic robot, and I'm taking a week off my regular job to work on it, I'll explain these details in another in a couple of weeks.
