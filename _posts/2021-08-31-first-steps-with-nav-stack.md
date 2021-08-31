---
layout: post
title: First steps with the ROS Navigation Stack
subtitle: Experimenting with gmapping, hector_slam and amcl
gh-repo: adityakamath/akros
thumbnail-img: /assets/img/akros_holo1_thumb.jpg
share-img: /assets/img/akros_holo1_thumb.jpg
gh-badge: [follow]
tags: [akros, robotics, software, motion control, ros, slam]
comments: true
---

Since the last update, I've been slightly busy with other things and hardly had time to work on the robot. However, in the few spare days I had, I was able to make quite a few changes and test out different SLAM methods and start with setting up the [ROS Navigation Stack](http://wiki.ros.org/navigation). I'll first start with the hardware changes. I tried out two alternatives to replace my battery and finally replaced it with a [mini UPS system](https://www.amazon.nl/gp/product/B07DPTF9VW/ref=ppx_yo_dt_b_asin_title_o01_s02?ie=UTF8&psc=1). These photos describe my original battery, and the alternatives I tried: 

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_og_bat1.jpg" />
	<figcaption>The original battery. I bought it from AliExpress. It is supposed to be for bicycle lights and for charging phones but I used the 12v DC power output to run both the motors and RPi using different power regulators</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_og_bat2.jpg" />
	<figcaption>This battery runs using 6x 18650 batteries. The batteries are of reputed brands and work fine individually. However, after about a few charges/discharges, the power board (which is embedded into the cap) stopped working. Not surprised.</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_npf_bat.jpg" />
	<figcaption>My first alternative - NP-F batteries. These ones are NP-F750 batteries with adapters that provide 8v and 12v DC outputs. I first saw them on [JetsonHacks' amazing video](https://www.youtube.com/watch?v=B4afWen1CsY) about these batteries and how they can be used to power Jetson devices. In parallel, and using the same power regulators as before, they could run the entire system, and also be hot-swappable. Definitely a win! But unfortunately, they both don't fit into the AKROS chassis. So, I'm keeping them for another project (or a future iteration of the same project)</figcaption>
</figure>
  
<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_ups_bat1.jpg" />
	<figcaption>Finally, this mini UPS I bought from Amazon. It is rated 36Wh, and provides 1x 5v2A output, 1x 12v1A output and 1x 12v2A output. These three outputs power the motor driver, the power hungry RPi4 and all sensors/peripherals. Its also got a very handy on-off button, and a useful battery level indicator. </figcaption>
</figure>
  
<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_ups_bat2.jpg" />
	<figcaption>This UPS is an even bigger win than the NP-F batteries. This is because I can charge the robot while its running. So, when the battery backup is low, I can simply plug the charger in without having to disconnect the RPi4. So, I don't have to frantically run to save the generated maps, I can simply plug it in and take my time..</figcaption>
</figure>
  
Once the battery was replaced, I decided to drive the robot around to test its capacity. I expected it to last about an hour (double that of my original battery), but it lasts only between 40-45 minutes, which is still much better than the original battery system. The new UPS is almost half the weight of the original battery and takes less space - this makes the robot lighter and more compact since I could reduce the spacer heights as well.
  
While driving the robot around, I made sure to also run SLAM algorithms in the background - firstly, to test the battery under full load and secondly, to get some nice maps of my studio. I tried both [gmapping](http://wiki.ros.org/gmapping) and [hector_slam](http://wiki.ros.org/hector_slam), and played around with parameters till I got acceptable maps from each of these methods. However, one method was the clear winner:
  
<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_hector_map.jpg" />
	<figcaption>Map created using Hector SLAM. This took quite some parameter tuning and the resulting map is not bad, however needs to be post-processed before it can be used for localization. One advantage that Hector SLAM has over GMapping is that it can make maps without an odometry source. However, I do have a pretty good odometry source, so I'll keep this method for a project without odometry...</figcaption>
</figure>
  
<insert map from gmapping>
<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_gmapping_map.jpg" />
	<figcaption>Map created using GMapping. It is pretty much perfect and definitely the winner against Hector SLAM. GMapping uses the odometry provided by the T265 camera.</figcaption>
</figure>
  
Next, I saved these maps using map_saver and then launched the localization node using [AMCL](http://wiki.ros.org/amcl) with each map. I tested the AMCL node in two ways - first, by driving the robot manually and comparing the laser scans with the map. Next, I moved some things around and tried driving the robot around manually. 
  
<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_amcl_test.jpg" />
	<figcaption>The AKROS robot being tested in my studio. It is being driven manually using a PS4 controller</figcaption>
</figure>
  
<insert testing image>
<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_amcl_foxglove.png" />
	<figcaption>Foxglove viz for the experiments. I've chosen to see compressed images from one of the T265's two fisheye lenses for debugging. The errors in the debug panel shows when the Arduino lost connection with the ROS master. This is a common problem that I haven't solved yet. Fortunately, the Arduino reconnects automatically, and I've written recovery functions in the Arduino code.</figcaption>
</figure>
  
Finally, I drove the robot to a random location, waited for it to localize correctly and then picked up the robot and placed it at another position. I repeated this multiple times, and also moved/rotated the robot while moving it. I wanted to test if the [Intel T265](https://www.intelrealsense.com/tracking-camera-t265/)'s ([RIP](https://discourse.ros.org/t/intel-cancelling-its-realsense-business-alternatives/21881)) 3D tracking could allow AMCL to localize correctly even if it received incorrect laser data... and it does! On average, between these three tests, the localization was accurate up to ~10cm. However, it should be noted that I used the map created by gmapping (the better one), was driving around in a very small space (~30m2) and the robot was never in transit for more than a few seconds (odometry/IMU drift would have taken longer). This last test took only about 2 minutes to perform, here's a video that shows what I did:
  
[![90 second localization experiment](https://adityakamath.github.io/assets/img/akros_amcl_test_ss.png)](https://www.youtube.com/watch?v=17wJ505WDYo "[90 second localization experiment - Click to Watch!")
  
For now, I'm more than happy with the results, although I hope to improve it even further once I implement odometry using the motor encoders. My next step is to first play with [Steven Macenski's slam_toolbox](https://github.com/SteveMacenski/slam_toolbox), and then focus on tuning the [move_base](http://wiki.ros.org/move_base) parameters (I already have the ROS package set up). I also plan on cleaning up my launch files and directory structure (I currently have all my launch files in a single ROS package), and I'm using the [Robotis Turtlebot3 repository on Github](https://github.com/ROBOTIS-GIT/turtlebot3). For this week, my focus is on the slam_toolbox and hopefully I can write another blog post about it this weekend..
