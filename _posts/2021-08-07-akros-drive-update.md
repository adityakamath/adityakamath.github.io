---
layout: post
title: AKROS Holonomic drive update
subtitle: Driving using a PS4 controller + SLAM
gh-repo: adityakamath/akros_test
thumbnail-img: /assets/img/akros_holo1_thumb.jpg
share-img: /assets/img/akros_holo1_thumb.jpg
gh-badge: [follow]
comments: true
---

I've been on holiday this week, and decided to have a staycation and spare some time working on the AKROS holonomic platform. I got it back from the friend I lent it to, wired it up, tested it and assembled with the [navigation module](https://adityakamath.github.io/2021-08-01-navigation-module-design-update/). I am not using the encoders for now, since I have the Intel Realsense T265 to provide some odometry estimates. First, I wrote the Arduino code to drive the holonomic platform (using [kinematic equations](https://research.ijcaonline.org/volume113/number3/pxc3901586.pdf)) taking velocity in x direction, velocity in y direction and rotation around z values as inputs. I made the robot drive along a pre-programmed path to test if everything worked. Since I was also charging the battery at the time, I decided to map the x, y and z values to RGB values for the neopixel headlights on the robot. This let me test the ROSSerial interface without having to drive the motors. I then added a cmd_vel ([twist](http://docs.ros.org/en/api/geometry_msgs/html/msg/Twist.html) message) subscriber and added the motor/neopixel control functionality to the callback function. Next, I tested if the wiring was correct and assembled the base with the navigation module.

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_base_holo.jpg" />
	<figcaption>Holonomic base platform with the battery pack, an Arduino Mega 2560 and a 4xDC motor driver (2x L29DN). The battery is a 6x18650 battery pack with a DC power output and a regulated USB power output. The USB power output is used to power up the Arduino as explained later</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_motor_driver.jpg" />
	<figcaption>Wiring of the motor driver (on top of the Arduino)</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_buck_boost_converter.jpg" />
	<figcaption>Buck boost converter used to convert the battery output to 9v for the motor driver</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_buck_converter.jpg" />
	<figcaption>3A Buck converter used to convert the battery output to 5v/3A for the Raspberry Pi 4. Its important for this to be rated 3A, I tried a 2A buck converter but this was not able to power the Pi correctly.</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_neopixel_headlight.jpg" />
	<figcaption>Headlights made using 2x neopixel compatible LED boards with 3x LEDs each, connected to the Arduino Mega</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_holo_testing.jpg" />
	<figcaption>Testing the base platform with the navigation module.</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_holo_assy1.jpg" />
	<figcaption>Assembling the navigation module on top of the base platform</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_holo_assd.jpg" />
	<figcaption>Assembled AKROS robot</figcaption>
</figure>

Next, I used the [ds4_driver library](https://github.com/chrippa/ds4drv) and the corresponding [ROS package](http://wiki.ros.org/ds4_driver) to connect the RPi4 with a PS4 controller over bluetooth. Fortunately, for me the [ROS package](https://github.com/naoki-mizuno/ds4_driver) already comes with a twist publisher node. I simply added a 3dof (linear x, linear y, angular z) config file and the twist node was good to go. I also modified the demo node provided to set the PS4 controller's LED to specific colors according to the input values (R=x, G=y, B=rotation). Now, the PS4 controller's LED should match the neopixel headlights on the robot. The next step was to add the ds4_driver nodes along with the [rosserial_python](http://wiki.ros.org/rosserial_python) node in a launch file, and try driving the robot around. I had to tweak the linear/angular gains and max velocity values and finally, I had a platform that I could drive around with a PS4 controller. The following videos show the process and results along the way:
  
[![One day Build - programming the omniwheel base](https://adityakamath.github.io/assets/img/akros_holo_vid1_ss.png)](https://www.youtube.com/watch?v=fXdokZt8AuA "[One day Build - programming the omniwheel base - Click to Watch!")
  
[![Mecannum/omniwheel robot with PS4 controller](https://adityakamath.github.io/assets/img/akros_holo_vid2_ss.png)](https://www.youtube.com/watch?v=iH0Y-vLhRmg "[Mecannum/omniwheel robot with PS4 controller - Click to Watch!")
  
[![Holonomic drive at 75% max speed](https://adityakamath.github.io/assets/img/akros_holo_vid3_ss.png)](https://www.youtube.com/watch?v=EfRlgJKz71Y "[Holonomic drive at 75% max speed - Click to Watch!")
  
[![Holonomic drive at 100% max speed](https://adityakamath.github.io/assets/img/akros_holo_vid4_ss.png)](https://www.youtube.com/watch?v=eQa8TO2GJB0 "[Holonomic drive at 75% max speed - Click to Watch!")
  
The next step was to run the [hector_slam](http://wiki.ros.org/hector_slam) launch file that I had set up some time ago. I modified it so that it also used the odometry provided by the T265 camera. Unfortunately, this is where I faced some issues. The RPi4 is unable to provide enough current to run 4 USB devices. Currently, I have the OAK-D and the T265 cameras attached to the USB3 ports and the RPLidar and the Arduino Mega connected to the USB2 ports. My battery also provides a 5V power supply as a USB output and I have that connected to the DC power port of the RPLidar adapter. I was able to run hector_slam and the drive launch files individually but never together. I decided to get a DC power jack and power the Arduino using the USB 5v port instead. For now, I moved the DC power cable of the RPLidar to my PC's USB port for testing. Unfortunately, this did not work either. As a last-ditch attempt, I decided to unplug the OAK-D since I wasn't using it anyway. Suddenly, both launch files could be run together. Success! I even unplugged the RPLidar's USB power cable and it still worked. This definitely solves my issues for now, but I need to investigate this further because I need the OAK-D in the future. I think powering the OAK-D using external DC power might fix it. If not, maybe moving to a Jetson Nano solves the issue, since it has 4 USB3 ports. Meanwhile, here is a video of hector_slam and the holonomic drive node in action, visualized using Foxglove. 
  
[![AKROS: Hector SLAM with RPLidar and T265](https://adityakamath.github.io/assets/img/akros_holo_vid5_ss.png)](https://www.youtube.com/watch?v=Yr-N-6RIQSU "[AKROS: Hector SLAM with RPLidar and T265 - Click to Watch!")
A few interesting things to notice here - first, at between [9-15 seconds](https://youtu.be/Yr-N-6RIQSU?t=8), we see the laser scan jump around as the odometry frame drifts. There are a few reasons for this. For starters, I used the camera and ROS nodes as-is. I need to spend some time and calibrate any parameters. Secondly, the room was dark and hence, visual tracking might not be entirely correct. Thirdly, the top panel vibrates due to the driving, which would cause the odometry to drift. I need to fix this using some vibration damping spacers. During the video, you can also see me moving the robot linearly quite a few times. I found this to be incredibly usefule since in my previous experiences, localization/mapping issues have occured mostly due to fast rotations. Finally, at around [2:12](https://youtu.be/Yr-N-6RIQSU?t=132), you can see the robot moving back to its origin, and the visual tracking latching on to the position. This compensates for the odometry drift and the odometry frame aligns with the map frame. This is an incredibly cool application, and I'm surprised it took only minutes to set up. One of the reasons why I enjoy working with ROS.
  
For my next steps, I need to fix the URDF, setup the [ROS Navigation Stack](http://wiki.ros.org/navigation) (using [GMapping](http://wiki.ros.org/gmapping), [AMCL](http://wiki.ros.org/amcl) and [move_base](http://wiki.ros.org/move_base); I need to choose between GMapping and Hector SLAM) and meanwhile also fix the OAK-D power issue. I also want to test the battery life of the entire system, for now I can drive around for a solid 30 minutes while also running SLAM on the RPi. 
