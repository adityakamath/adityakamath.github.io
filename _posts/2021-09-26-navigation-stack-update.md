---
layout: post
title: Navigation stack update
subtitle: Testing assisted_teleop and move_base
gh-repo: adityakamath/akros
thumbnail-img: /assets/img/akros_move_base_thumb.png
share-img: /assets/img/akros_move_base_thumb.png
gh-badge: [follow]
comments: true
---

After the last update, I had a robot capable of performing SLAM, building 2D maps and localizing within a map using [amcl](http://wiki.ros.org/amcl). This weekend, I continued further and implemented the [move_base](http://wiki.ros.org/move_base) package to complete the [ROS navigation stack](http://wiki.ros.org/navigation). Before I did that, I was exploring the [navigation_experimental](http://wiki.ros.org/navigation_experimental) package and found the [assisted_teleop](http://wiki.ros.org/assisted_teleop?distro=noetic) node. This node uses the laser scan to create a local costmap. Using this costmap and input teleop commands, the node decides when the robot is too close to an obstacle and accordingly changes the teleop commands provided to the motor controller. For this, I had to modify my twist mixer node to accept three inputs - the original teleop commands, the remapped assisted_teleop commands and the twist messages from the move_base. Using the PS4 controller, I am able to switch between these modes. The following video shows the assisted_teleop node in action:

  [![AKROS: Localization + Assisted Teleop test](https://adityakamath.github.io/assets/img/akros_assisted_teleop_ss.png)](https://www.youtube.com/watch?v=CMW9YnUrFxs "AKROS: Localization + Assisted Teleop test")
  
With the assisted_teleop done, it was time to implement move_base. I used the standard configuration with [base_local_planner](http://wiki.ros.org/base_local_planner), [global_planner](http://wiki.ros.org/global_planner) and default recovery behaviors. I did have to modify some parameters like the linear/angular accelaration and velocity limits and the robot footprint. I also set the holonomic parameter to true, so that the local planner could generate twist messages for the robot accordingly. The video below shows the results of this first experiment, although there are still a lot of work to be done. 
  
  [![AKROS: move_base and ROS navigation stack](https://adityakamath.github.io/assets/img/akros_move_base_ss.png)](https://www.youtube.com/watch?v=DU5ga8xqMbQ "AKROS: move_base and ROS navigation stack")
  
The move_base implementation is not complete yet, since I still need to fine tune the parameters. I also want to experiment with [dwa_local_planner](http://wiki.ros.org/dwa_local_planner) and [teb_local_planner](http://wiki.ros.org/teb_local_planner) since I keep reading about these packages online. Meanwhile, I am also switching to a [LD06 Lidar](https://www.inno-maker.com/product/lidar-ld06/) (since the RPLidar is needed at work), which I just received from China. I want to replicate the move_base implementation with this new Lidar. I have also backed the OAK-D Lite kickstarter campaign and am expecting to receive the camera in December. So, I will also be designing a new LD06 + OAK-D Lite mount to replace the current RPLidar + OAK-D mount. I would expect this to reduce the mass of the robot since the LD06 and the OAK-D Lite are considerably lighter than their counterparts currently on the robot.
   
<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_ld06.jpg" />
  <figcaption>LD06 laser scanner with a 2 Euro coin for scale</figcaption>
</figure>
  
The LD06 is a cute little lidar scanner that works just as well (if not better) as the RPLidar A2. Quite surprising because of its small form factor and a 100 USD price point. To attach the LD06, I first removed the RPLidar and temporarily hot-glued the LD06 to the existing base plate, while I design a new base plate for it. I also attached the USB adapter to the bottom of the plate using a glue gun. I used [Alessio Morale's LD06 ROS package](https://github.com/AlessioMorale/ld06_lidar) to receive the laser scan on ROS. While the package is an excellent starting point, there are a few shortcomings, which I want to investigate..
  
* Unlike the RPLidar which was turned on when the ROS node was launched, the LD06 is always on and the ROS node simply reads the continuous serial data and provides ROS messages. This is slightly annoying because I am not using the LD06 when the ROS node is not running and I would expect the lidar to stop scanning. 
  
* From the sparse documentation available online, it seems like the LD06 can be directly connected to the UART port on a Raspberry Pi instead of the UART-USB converter and the USB port. This is really a plus for my application, but unfortunately the LD06 ROS package does not take this into account. It looks for a /dev/ttyUSB* port to connect to. Once again, this is something I plan on fixing in the future. 
  
But for now, Alessio's ROS package is sufficient. Once I got it up and running, which took me less than a couple of minutes, the next step was to modify my current navigation stack to use the new Lidar. This involved updating the URDF file and remapping the laser scan output to the correct topic names. With this completed, I am now able to run all my demo launch files (SLAM, amcl, move_base) with the new lidar. As for my first impressions, the lidar seems to operate just as well as the RPLidar A2 - the created maps are of similar quality and I see no improvements/degradations in the performance of AMCL and move_base (I did not change any parameters). However, for some unexplained reason, assisted_teleop performs much better with the LD06. 
  
<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_with_ld06.jpg" />
  <figcaption>AKROS Holonomic robot with LD06 instead of the RPLidar A2</figcaption>
</figure>
  
For my immediate next steps, I'm learning about high-level behaviors, using [FlexBE](http://wiki.ros.org/flexbe) and [BehaviorTree.cpp](https://www.behaviortree.dev/), both well known and commonly used packages for designing complex behaviors for robots. I plan on implementing small applications - like a goal sequence publisher for move_base and an alternate cmd_vel mixer/multiplexer using behavior trees. Alongside this, I will also try to fine tune move_base, and also implement the two local planners mentioned above. More updates next week. 
