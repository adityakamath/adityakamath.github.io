---
layout: post
title: Switching drive modes on the Jetbot
subtitle: And mapping drive modes with NeoPixel LEDs
gh-repo: adityakamath/arduino_sketchbook_ros
gh-badge: [follow]
thumbnail-img: /assets/img/jetbot2_driving_modes_thumb.jpg
share-img: /assets/img/jetbot2_driving_modes_thumb.jpg
comments: true
---

This weekend, I worked on the different driving modes of the Jetbot2. As explained in a previous blog post, the robot is designed to have 3 driving modes - full teleop, semi-autonomous (linear velocity via teleop, angular velocity via the autonomous node) and fully autonomous. Each mode is given a number - 1, 2 and 3 respectively. To switch between the modes, I used the 'select' button on the joystick and wrote an additional function that increments a counter from 1 to 3 every time the button is pressed. A mode 0 is also introduced - as the default mode before any of the three driving modes is set. This mode is only available when the robot is turned on, and is kept aside as a test node, to test if all functionalities of the robot are working before driving it with 100% velocity scaling. This node was set up such that the mode number is published as an integer every time the select button is pressed. 

To indicate the switching between modes, the Arduino Nano and the NeoPixel LED strip was used. Using [rosserial](http://wiki.ros.org/rosserial) on the Arduino Nano, a subscriber was written to read the incoming integer and switch the LED colors accordingly. For the LEDs, 5 modes were chosen - 3 drive modes, 1 idle mode, 1 mode for connection loss with the ROS Master running on the Jetson Nano. The three drive modes were given the colors Red, Green and Blue respectively. The idle mode - which is initialized either in Mode 0 or when the sixaxis controller is turned off, was given the color Yellow. The connection-loss mode was given the color Purple. The functionality of the LED switcher is as follows:

* When the robot is turned on, the lights turn Purple. The used needs to press any key on the joystick to put the robot into Mode 0 and turn the lights Yellow
* In this situation, the robot does not turn on as the start button still needs to be pressed on the joystick. Once this is done, the lights remain Yellow but the Jetbot2 can be moved with 75% velocity scaling
* When the select button is pressed, the Jetbot2 enters Mode 1. From this time onwards, Mode 0 is unavailable and every time the select button is pressed, the Jetbot2 cycles from Mode 1 till Mode 3 and then back to Mode 1. The colors change to Red, Green and Blue respectively. 
* When the robot is in any of these modes, the start button can be pressed again to toggle the joystick to the off state. The LEDs turn Yellow but the previous mode is stored. When the start button is pressed again, the previous mode is recovered. 
* When the robot loses sync with the ROS Master on the Jetson Nano, or when the ROS node/launch file is terminated, the lights turn back to Purple. 

Some of these features can be seen in the video below. This video was taken before all the functionalities were written and does not fully show the connection-loss or the Idle modes. 

[![Avoiding edges with the Jetbot2](https://adityakamath.github.io/assets/img/jetbot2_edge_avoidance_ss.png)](https://www.youtube.com/watch?v=9J5rK8DWGGw "[Avoiding edges with the Jetbot2 - Click to Watch!")

Meanwhile, I also found some other fun things I can stick googly eyes on, and this was my favourite: 

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/googly_eyes_random.jpg" />
</figure>

For the next week, I want to work on camera calibration and Aruco/AprilTag/Fiducial Marker recognition using OpenCV. I plan on writing these as a ROS service and node. Meanwhile, it is also holiday time, and I have registered for the Reddit Secret Santa gift exchange. You can use [this link](http://www.redditgifts.com/?inv=ExCc) to sign up yourself, but you've got only a day left!


