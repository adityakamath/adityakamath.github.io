---
layout: post
title: Comparing different SLAM methods
subtitle: Experimenting with slam_karto and slam_toolbox
gh-repo: adityakamath/akros
thumbnail-img: /assets/img/akros_slam_toolbox_thumb.jpg
share-img: /assets/img/akros_slam_toolbox_thumb.jpg
gh-badge: [follow]
tags: [akros, robotics, software, localization, ros, slam]
comments: true
---

This week, as planned, I tried out [Steven Macenski's slam_toolbox package](https://github.com/SteveMacenski/slam_toolbox) alongside [slam_karto](http://wiki.ros.org/slam_karto), the ROS wrapper for the Karto mapping library, another popular SLAM method. Before I began, I reorganized the project directory structure to resemble that of the [Turtlebot3 repo by Robotis](https://github.com/ROBOTIS-GIT/turtlebot3), which makes things easier as it separates the bringup, SLAM and navigation launch file in different packages. 

Next, I created maps of my studio using all the different techniques - [hector_slam](http://wiki.ros.org/hector_slam), [gmapping](http://wiki.ros.org/gmapping), slam_karto and slam_toolbox. I ran the experiment by first creating the following launch files: 1 launch file to drive the robot (runs the [PS4 controller nodes](https://github.com/adityakamath/ds4_driver), and the [rosserial node](http://wiki.ros.org/rosserial_python)), 1 launch file to bringup the devices (the RPLidar and the Intel T265 camera), and the final launch file to include the bringup launch file and the correct SLAM launch file based on a launch file argument. During each run, the 1st and the 3rd launch files were run on separate terminals with the correct input argument. During each run I also made sure that the robots started and ended at the same positions in the environment. I also used a similar route each time. The video below shows a sped up version (4x) of the four runs. 

[![AKROS: trying different SLAM methods](https://adityakamath.github.io/assets/img/akros_slam_test_ss.png)](https://www.youtube.com/watch?v=4jr_IaAeu-M "AKROS: trying different SLAM methods")
  
<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_slam_maps.jpg" />
	<figcaption>Maps created from the different SLAM methods. The [map_server/map_saver](http://wiki.ros.org/map_server) node was used to save these maps.</figcaption>
</figure>
  
Here are my conclusions:
  
**HECTOR_SLAM**
Final conclusion: decent option if you have a lidar-only system (and low vibrations), definitely much better maps when reliable odometry is also used.

This is an interesting SLAM package because it works both with and without odometry info. It also has a neat [hector_trajectory_server node](http://wiki.ros.org/hector_trajectory_server) that makes the trajectory data available via a topic which can then be used to visualize the robot's path using Rviz or Foxglove. I first tried hector_slam using only the lidar data - but this was not quite a success. Due to the robot's fast movements and jerky motion at times, I could only get good maps when I drove really slow and did not make any rotations. However, with the odometry from the Intel T265, things got much better (as seen in the video) - I could now drive faster and make sharp turns. But now, the SLAM algorithm seemed to be heavily dependent on the odometry, and the T265 is not always reliable. Now, I was able to get really good maps, as long as the T265 odometry did not fail (which it usually does when I either block vision or accelerate quickly. 

**GMAPPING**
Final conclusion: Great maps for small spaces, perfect starter SLAM package

I don't have much to say about GMapping. Its the standard SLAM package in ROS, and I've used it since 2016. It provides really good maps, certainly much better than hector_slam. Unfortunately, it needs an odometry source to work well, so this cannot be used in a lidar-only system. Luckily, I have the t265. GMapping doesn't seem to work well in really large spaces (like warehouses), so while its really good for a home/studio environment, there is room for improvement when in bigger spaces (using slam_toolbox is an alternative). 
  
**KARTO**
Final conclusion: Another great starter SLAM package for ROS learners, I haven't personally tested this in larger spaces, but provides Gmapping-like results in small spaces.

This was not part of my plans but from everything I've read, slam_karto is another great SLAM package to start off with. Just like GMapping, it provides really good maps, but uses odometry and unlike hector_slam, does not fail when odometry fails from time to time (the odometry eventually recovers, but while its in the failure state, the SLAM pose does not drift). slam_karto also provides a visual feedback of the traversed path of the robot, which can be visualized in Rviz or Foxglove.
  
**SLAM_TOOLBOX**
Final conclusion: This package has the most options compared to the other methods - online/offline configurations, lifelone mapping and localization modes. In small spaces, the generated maps are just as good as the gmapping maps but slam_toolbox is more reliable. I haven't tried it in larger spaces..

This package provides a lot more options compared to the other methods - synchronous/asynchronomous mapping, lifelong mapping, offline mapping, map-merging tools, an interactive mode. A lot more information can be seen on their [github repo](https://github.com/SteveMacenski/slam_toolbox/tree/noetic-devel). I certainly had fun trying some of these methods out - but I couldn't find any visible differences between some of these nodes. I assume this is because the space I mapped was quite small and not very dynamic, but I still need to dig further and play with some parameters... For now, the generated maps are comparable to the maps created with gmapping, but slam_toolbox is in general quite reliable.  so I plan on continuing to use the slam_toolbox package. I haven't tried the interactive mode and the map-merging tools yet. I also made my own launch file that allows me to launch any of these modes from a single command (with arguments) and sets the correct parameters for each mode.

For me, I found slam_toolbox to be the most reliable out of the four methods I tested. It also implemented a lot of different use-cases, and provided tools, all optimized for large scale mapping. Also looking at anecdotal experience documented online, I definitely think slam_toolbox is the right choice. However, for small places, gmapping and slam_karto provide similar results, sometimes gmapping is much better as well. All this makes me really curious about what [Nav2](https://navigation.ros.org/) is like..
  
At the end of this experimentation process, I also added some finishing touches. I put everything together in 1 launch file. It still uses 3 launch files, but the SLAM launch file references the other two and can be run simultaneously. I also added hector_trajectory_server to each SLAM node because I find it really useful to visualize the robot's trajectory while doing SLAM, might provide some good insights in the future, when I plan on mapping my room autonomously. For now, the general SLAM launch file references the async online mode of slam_toolbox and it works really well. I also have the option to change it to something else by setting an argument while running the launch file.
  
To summarize, I now have a robot capable of performing different SLAM methods, both with and without using odometry. I also have well configured launch files to run any SLAM method and all other required nodes using a single command. I also have a directory structure set up, which makes it easier to implement the [ROS1 navigation stack](http://wiki.ros.org/move_base?action=AttachFile&do=view&target=overview_tf.png) using [move_base](http://wiki.ros.org/move_base). However, before setting up move_base I want to get the OAK-D up and running again. I had stopped working on it a few weeks ago and there have been some new updates in their ROS driver, which I'm really keen to check out. 
