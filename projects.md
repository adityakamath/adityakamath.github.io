---
layout: page
title: Projects
subtitle: Things I've been working on (2011 - Present)
---

Some of my favourite robotics projects - a representation of the kind of projects I would like to work on and write about on this blog. I have also worked on other projects, but they are not very relevant to this website. You can have a look at these experiences on my LinkedIn (link at the bottom of the page).

### [Autonomous ROS1 Robot - AKROS](https://github.com/adityakamath/akros)
#### 2021 |

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/akros_holo_circle.png" />
</figure>

Through 2021, I built and programmed an autonomous mecanum-wheel based robot using ROS1 on a Raspberry Pi 4. I used a 2D LiDAR, a visual-inertial odometry camera (Intel T265), and wheel odometry to build a robot that can autonomously map and navigate in my studio apartment. I also implemented additional features such as teleop assist/mixing, closed loop (holonomic) motion control, life-long SLAM and behavior trees.

### [Jetson Robot Platforms - JetRacer, JetBot](https://github.com/adityakamath/jetbot2_ws)
#### 2020 |

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/jetbot_2.png" />
</figure>

During the Covid-19 lockdown, I decided to upgrade my Donkey Car into an NVidia JetRacer (based on the NVidia Jetson Nano), with custom add-ons like an IMU and teleop control. Due to the lack of driving space, I moved everything to a JetBot platform (differential drive). To demonstrate the Jetson Nano's AI capabilities, I trained an edge detection model to drive the robot on a desk without falling off.

### [Remote Computing for Soccer Robots using 5G mm-Waves](https://research.tue.nl/nl/publications/enabling-remote-computation-for-soccer-robots-using-5g-mm-waves-d)
#### 2018/2019 | BlueSPACE, Tech United, TU Eindhoven

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/pdeng_thesis.png" />
</figure>

For my PDEng Thesis, I designed and built a functional prototype for 5G-Robotics demonstrations using RoboCup MSL football playing robots. This experimental setup was built using Tech United's TURTLE platform and BlueSPACE's mm-Wave setup, in order to validate that a distributed robotic/motion control system (operating above 1 kHz) can still maintain its real-time properties over a 5G network.

### [Multi-Drone Positioning and Formation Flying](https://www.tue.nl/en/research/aiming-at-the-sun-with-flying-drones/)
#### 2018 | European Space Agency (ESTEC), TU Eindhoven

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/formation_flying.png" />
</figure>

Our team of 12 designed a multi-quadrotor demonstrator to simulate a multi-satellite space mission by ESA for the ESA ESTEC Open Day 2018. As Software Architect and Designer, I led the development of a multi-camera positioning system to localize two quadrotors in 6dof for accurate trajectory following and control.

### [Autonomous Drone Referee](http://cstwiki.wtb.tue.nl/index.php?title=Drone_Referee_-_MSD_2017/18)
#### 2018 | Tech United, Eindhoven University of Technology

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/drone_referee.png" />
</figure>

In a team of 7, we designed and implemented an autonomous drone concept to referee RoboCup MSL football matches. The drone was programmed to track a calculated "bubble of active play" and provide recommendations based on the status of the tracked ball (goal, throw in) and the tracked player robots (collisions, fouls). This was demonstrated on Tech United's RoboCup field using their TURTLE robot platform.

### [AGV Localization in Changing Indoor Environments](https://research.tue.nl/nl/studentTheses/a-study-of-mobile-robot-localization-in-changing-indoor-environme)
#### 2016/2017 | Prodrive Technologies, TU Eindhoven

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/master_thesis.png" />
</figure>

For my Master Thesis, I worked with Prodrive Technologies to study and improve their current AGV localization strategy for their next generation of autonomous 2D LiDAR based robots. Multiple strategies were proposed, implemented, and validated in a simulated environment using ROS and Gazebo. The leading strategy involved the inclusion of an IMU to correct for any localization failures, and predictive re-mapping of the environment to correct for changes in the environment. This project is not open-access.

### [Maze Solving Robot](http://cstwiki.wtb.tue.nl/index.php?title=Embedded_Motion_Control_2016)
#### 2016 | TU Eindhoven

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/embedded_motion_control.png" />
</figure>

As a part of a master's course, our team of 3 programmed a ROS-based holonomic robot platform to complete a maze using only 2D LiDAR data. We implemented the pledge algorithm and random walk to solve simulated and physical mazes with 90 degree turns, interactive doorwars, and open spaces. We participated in two challenges during the course, and ranked second, thus winning a crate of (well-deserved) beer!

### [Autonomous Navigation of a Lunar Robot](https://oaji.net/articles/2014/489-1409643664.pdf)
#### 2013/2014 | MIT Manipal

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/bachelor_thesis.png" />
</figure>

For my Bachelor End Project, I designed and simulated an autonomy strategy for the lunar excavator robot built by our student team RoboManipal in 2012-2013. This involved the study and implementation of robot perception, trajectory planning and finally motion control to complete the NASA Lunabotics Mining Challenge 2013 problem statement autonomously.

### [NASA Lunabotics Mining Competition 2013](https://www.nasa.gov/pdf/726112main_Lunabotics%202013%20Press%20Kit_Layout%201.pdf)
#### 2012/2013 | RoboManipal, MIT Manipal

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/nasa_lunabotics.png" />
</figure>

Our 12-member team participated in the 4th NASA Lunabotics Mining Competition at Kennedy Space Center in Florida, USA in 2013. I was the Student Team Lead and Systems Engineer (Electronics/Software). As system engineer, I designed the electronics, communications and control software for our lunar excavator robot which teleoperated over a high-latency wireless network. The team ranked 19th from 50 teams.

### [ABU Robocon 2012](https://www.youtube.com/watch?v=Ljupjcuj8JI)
#### 2011/2012 | RoboManipal, MIT Manipal

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/abu_robocon.png" />
</figure>

Our student team built one autonomous robot and one manually operated robot for the ABU Robocon 2012 national-level competition. I was involved in the design of the electronics (IR sensor arrays, relay boards, Arduino shields) and development of the autonomous functionalities based on grid-based positioning algorithms. The autonomous robot was programmed to navigate on a flat surface with a grid, grab an object and place it autonomously at a pre-defined location.
