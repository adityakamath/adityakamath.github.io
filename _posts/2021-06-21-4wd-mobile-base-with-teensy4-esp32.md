---
layout: post
title: 4WD mobile robot with ESP32 + Teensy
subtitle: First steps towards ROS2/MicroROS implementation
gh-repo: adityakamath/akros_micro_test
thumbnail-img: /assets/img/akros_micro_step_thumb.jpg
share-img: /assets/img/akros_micro_step_thumb.jpg
gh-badge: [follow]
comments: true
---

Ever since I read [this article](https://micro.ros.org/blog/2020/08/27/esp32/) about [microROS](https://micro.ros.org//docs/overview/features/) (ROS2 for microcontrollers) and ESP32, I've been wanting to implement it myself but I never really got the time. This was because I was working on the DepthAI + OAKD project. But I did manage to buy some components in the meantime - I got 4x [N20 DC motors with quadrature encoders](https://www.adafruit.com/product/4640) from AliExpress, 2x [TB6612FNG dual-H-bridge motor drivers](https://www.sparkfun.com/products/14451) ([here](https://www.hackster.io/news/tb6612fng-motor-driver-better-than-the-l298n-7499a7574e63) is an article about TB6612FNG vs L298N), some ESP32 boards (including the DevKit C from the microROS article) and while I was there, I also purchased a Teensy 4.0 ([apparently this can also run microROS](https://github.com/micro-ROS/micro_ros_arduino)).

So, when I first got some spare time, I drilled some holes into a proto-board and attached the 4 motors to each corner. I also soldered headers to attach the [ESP32 DevKit C](https://www.espressif.com/en/products/devkits/esp32-devkitc/overview). I then soldered headers for the 2 motor driver boards and also soldered the motor's wires to the proto-board. Next, I took a spare IMU and soldered headers for it on the board. Here's how it looked like:

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_micro_step_1.jpg" />
	<figcaption>Step 1: Attached motors and sub-module boards with headers</figcaption>
</figure>
  
By this time, I was getting some idea of what I want to do with this board/robot - I wanted to implement a motor controller board, with closed loop control using encoder and IMU feedback. I wanted this board to be capable of running up to 4 motors independently which will allow different robot configurations - 4W differential, 2W differential, 4W holonomic with mecannum wheels, 3W holonomic with mecannum wheels or also other robot applications where up to 4 DC motors might be required. I also wanted headers to connect external I2C devices such as a [PCA9685 16 channel servo control board](https://www.adafruit.com/product/815). 
  
With an idea in place and the basic robot structure already set up, it was time to do the wiring. I first wired all the Vcc and Gnd pins to respective pins on the ESP32 to see if everything was working. Once this was verified, I connected the IMU SDA/SCL pins to the I2C pins of the ESP32, and the IN1/IN2/PWM pins of each motor to digital pins on the ESP32. I then connected the motor +/- pins to respective pins on each motor driver board. Now, everything except the encoders were connected and this can be seen below:
  
<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_micro_step_2.jpg" />
	<figcaption>Step 2: Connected the motors and motor drivers to the ESP32</figcaption>
</figure>
  
Finally, it was time to connect the encoder pins to the ESP32 board. This is when I realized that I did not have enough pins for each encoder. Since these are [quadrature encoders](https://cdn.sparkfun.com/datasheets/Robotics/How%20to%20use%20a%20quadrature%20encoder.pdf), there are 2 hall effect sensors per motor, which means 8 pins for all 4 motors. This is when I realized that there is space on the board for a Teensy 4.0 and connected the encoders to it. Every digital pin on the [Teensy 4.0](https://www.pjrc.com/store/teensy40.html) is interruptable, so this definitely solved my problem. This is how it looked like:
  
<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_micro_step_3.jpg" />
	<figcaption>Step 3: Connected encoders to the Teensy 4.0</figcaption>
</figure>
  
To top it all off, I took another PCB of the same size and attached it using PCB spacers like seen in the image below. This let me make a platform where I could place and USB power bank. There is also enough space available to solder in additional features such as head/tail lights or other sensors. With the Teensy 4.0, I also have enough IO pins available. This is how the final robot looks like:
  
<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_micro_step_4a.jpg" />
	<figcaption>Step 4: Added platform for battery pack</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_micro_step4b.jpg" />
	<figcaption>Step 4: Added battery pack</figcaption>
</figure>
  
Now it was time to program it. I started with [Blynk IOT](https://blynk.io/), an application I've used before in the Pinguino notification lamp. This app lets me build a GUI using drag-drop elements and using their web interface, and Arduino libraries, I can stream these GUI inputs to any arduino based device (this includes both ESP32 and Teensy 4.0). Using this app, I was able to make a joystick controller to provide linear/angular and scaling commands to the ESP32, which then controls the PWM output of each motor using differential steering. In the following video, I control the robot using this app and drive it around my studio, have a look:
  
[![ESP32 + Teensy 4WD Robot Demo with Blynk IOT](https://adityakamath.github.io/assets/img/akros_micro_blynk_ss.png)](https://www.youtube.com/watch?v=yEHzI37K8TU "[ESP32 + Teensy 4WD Robot Demo with Blynk IOT - Click to Watch!")
  
Finally, no robot is complete without some personality, so I added some googly eyes. 
  
[![Micro: ESP32 + Teensy 4WD Robot](https://adityakamath.github.io/assets/img/akros_micro_googly_ss.png)](https://www.youtube.com/watch?v=gmitsplHoWs "[Micro: ESP32 + Teensy 4WD Robot - Click to Watch!")
  
For now, this application is solely programmed on the ESP32 using the Arduino IDE. It controls the motors in an open loop way since the encoder interrupts are not yet programmed. This would be my first step. The next step would be to make the teensy talk to the ESP32 over serial/I2C/SPI/CAN...there are multiple options available. This would close the loop. The final step would be to fuse the IMU measurements to correct the encoder estimates, and then port everything to micro-ROS. 
  
The next goal of this new project is to design a PCB to integrate all these things. I plan on using a single [Teensy 4.1](https://www.pjrc.com/store/teensy41.html) instead of an ESP32 and Teensy 4.0. The 4.1 has enough pins for 4 motors, 4 quadrature encoders, I2C, Serial/UART, Ethernet, CAN, SPI and even more GPIO to spare. It also has an SD card slot where can record and store the robot trajectories once the encoder/IMU functionality is programmed. More on this soon. 
