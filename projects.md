---
layout: page
title: Projects
subtitle: Things I've been working on (2011 - Present)
---

asdfa

### AKROS - Autonomous Holonomic Robot Platform
#### 2021 | Personal Project in Lockdown

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/akros_holo_circle.png" />
</figure>

As the lockdown(s) extended into 2021, I built an autonomous ROS robot from scratch. I used a 2D LiDAR scanner, a tracking/odometry sensor (Intel T265 camera) and a depth camera (OAK-D) to build a robot that can autonomously map and navigate in my studio apartment. The entire system was developed using the ROS (ROS1 Noetic) navigation stack, and migration to ROS2 (and Nav2) is still in progress. [[1]](https://github.com/adityakamath?tab=repositories) [[2]](https://github.com/adityakamath/akros) [[3]](https://github.com/ros-planning/navigation) [[4]](https://navigation.ros.org/)

### Jetson Robot Platforms - JetRacer, JetBot
#### 2020 | Personal Project in Lockdown

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/jetbot_2.png" />
</figure>

During the Covid-19 lockdown, I decided to upgrade my Donkey Car into a JetRacer (ackermann-steered, based on NVidia JetRacer). I also added some custom add-ons like an IMU. Due to the lack of driving space, I moved everything to a JetBot platform (differential-drive). To demonstrate the Jetson Nano's AI capabilities, I trained an edge-detection model to drive the robot on a desk without falling off. [[1]](https://github.com/adityakamath?tab=repositories) [[2]](https://github.com/dusty-nv/jetson-inference) [[3]](https://github.com/dusty-nv/jetbot_ros) [[4]](https://github.com/NVIDIA-AI-IOT/jetracer)

### Remote Computing for Soccer Robots using 5G mm-Waves
#### 2018/2019 | BlueSPACE, Tech United, TU Eindhoven

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/pdeng_thesis.png" />
</figure>

For my PDEng Thesis, I designed and built a functional prototype for 5G-Robotics demonstrations. This experimental setup was built using Tech United's TURTLE platform and BlueSPACE's mm-Wave setup, in order to validate that a distributed robotic/motion control system (operating at 1 kHz) can still maintain its real-time properties over a 5G network. [[1]](https://research.tue.nl/nl/publications/enabling-remote-computation-for-soccer-robots-using-5g-mm-waves-d) [[2]](https://zenodo.org/record/3519223#.X8ko9WhKhPY)

### Multi-Drone Positioning and Formation Flying
#### 2018 | European Space Agency (ESTEC), TU Eindhoven

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/formation_flying.png" />
</figure>

Our team of 12 designed a multi-quadrotor demonstrator to simulate a multi-satellite space mission by ESA for the ESA ESTEC Open Day 2018. As Software Architect and Designer, I implemented the multi-camera positioning system to localize the drones in 6dof for accurate trajectory following and control. [[1]](https://www.tue.nl/en/research/aiming-at-the-sun-with-flying-drones/) [[2]](https://www.4tu.nl/sai/en/valorisation/PDEng%20trainees%20working%20on%20new%20ESA%20space%20missions/) [[3]](https://www.hannovermesse.de/product/pdeng-demonstrator-with-drones/229789/K988717)

### Autonomous Drone Referee
#### 2018 | Tech United, Eindhoven University of Technology

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/drone_referee.png" />
</figure>

In a team of 7, we designed and implemented an autonomous drone concept to referee Robot Soccer matches. The drone was programmed to track a calculated "bubble of active play" and provide recommendations based on the status of the ball (goal, throw in) and the tracked players (collisions, fouls). This was demonstrated on Tech United's RoboCup field. [[1]](http://cstwiki.wtb.tue.nl/index.php?title=Drone_Referee_-_MSD_2017/18)

### AGV Localization in Changing Indoor Environments
#### 2016/2017 | Prodrive Technologies, TU Eindhoven

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/master_thesis.png" />
</figure>

For my Master Thesis, I worked with Prodrive Technologies to study and improve their current AGV localization strategy for their next generation of autonomous 2D LiDAR based robots. Two strategies were proposed, implemented, and validated in a simulated environment using ROS and Gazebo. This project is not open-access. [[1]](https://research.tue.nl/nl/studentTheses/a-study-of-mobile-robot-localization-in-changing-indoor-environme)

### Maze Solving Robot
#### 2016 | TU Eindhoven

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/embedded_motion_control.png" />
</figure>

As a part of a master's course, our team of 3 programmed a ROS-based holonomic robot platform to complete a maze using only 2D LiDAR data. We implemented the pledge algorithm to solve simulated and physical mazes with 90 degree turns, interactive doorwars, and open spaces. We participated in two challenges during the course, and ranked second, thus winning a crate of (well-deserved) beer! [[1]](http://cstwiki.wtb.tue.nl/index.php?title=Embedded_Motion_Control_2016)

### Autonomous Navigation of a Lunar Robot
#### 2013/2014 | MIT Manipal

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/bachelor_thesis.png" />
</figure>

For my Bachelor End Project, I designed and simulated an autonomy strategy for the lunar excavator robot built by our team RoboManipal in 2012-2013. This involved a study and implementation of robot perception, trajectory planning and finally motion control to complete the NASA Lunabotics Mining Challenge 2013 problem statement autonomously. [[1]](https://issuu.com/impactjournals/docs/1._eng-autonomous_navigation_of_a_l) [[2]](http://oaji.net/articles/2014/489-1409643664.pdf)

### NASA Lunabotics Mining Competition 2013
#### 2012/2013 | RoboManipal, MIT Manipal

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/nasa_lunabotics.png" />
</figure>

Our 12-member team participated in the 4th NASA Lunabotics Mining Competition at Kennedy Space Center in Florida, USA in 2013. I played the roles of Team Lead and Systems Engineer (Electronics/Software). As system engineer, I designed the electronics, communications and control software for our differential drive lunar excavator robot which teleoperated over a high-latency wireless network. The team ranked 19th from 50 teams. [[1]](https://robomanipal.wordpress.com/) [[2]](https://robomanipal.com/) [[3]](https://forums.ni.com/t5/Academics-Documents/Telerobotic-Lunar-Excavator-RMX-13/ta-p/3520498?profile.language=en)

### ABU Robocon 2012
#### 2011/2012 | RoboManipal, MIT Manipal

<figure class="aligncenter">
	<img align="right" width="200" height="200" src="https://adityakamath.github.io/assets/img/abu_robocon.png" />
</figure>

Our student team built an autonomous robot and a manually operated robot for the ABU Robocon 2012 problem statement. I was involved mostly in the design of the electronics (IR sensor arrays, relay boards, Arduinos) and development of the autonomous functionalities based on a grid following algorithm. The autonomous robot was programmed to navigate on a grid, grab an object and place it autonomously at a pre-defined location. [[1]](https://robomanipal.wordpress.com/) [[2]](https://robomanipal.com/)
