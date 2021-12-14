---
layout: post
title: Holonomic robot build update
subtitle: Setting up the OAK-D and RPLidar
gh-repo: adityakamath/depthai_ros
thumbnail-img: /assets/img/akros_holo_thumb.jpg
share-img: /assets/img/akros_holo_thumb.jpg
gh-badge: [follow]
comments: true
---

Since the last update, I have been waiting on acrylic parts for the OAK-D camera and the [OAK-D](https://store.opencv.ai/products/oak-d) itself. The [OpenCV AI Kit](https://opencv.org/introducing-oak-spatial-ai-powered-by-opencv/) - Depth ([OAK-D](https://docs.luxonis.com/en/latest/pages/products/bw1098obc/)) is a spatial-AI camera with a RGB and stereo cameras with on-board mono/stereo inferencing. After a few delays, apparently due to Brexit, the camera finally arrived at the start of February and the acrylic parts arrived soon after. Once assembled, the platform looks like this:

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_holo_assembly.jpg" />
	<figcaption>The AKROS Holo platform</figcaption>
</figure>

I added some spare LED boards to the front with some acrylic mounts and connected them to the Arduino. For now, it just randomly cycles through the colors but I plan on using them as status LEDs in the future. The OAK-D camera also needs a DC power input, but since I hadn't accounted for that earlier, I still need to power the camera externally. For now, I separated the top part of the robot (the platform with the RPi, the RPLidar and the OAK-D) and I'm using it for testing the sensors.

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_holo_assembly_top.jpg" />
	<figcaption>Disassembled top part used for testing</figcaption>
</figure>

For starters, I only want to set up the ROS packages for the lidar and the camera and make sure I have all the ROS transforms configured correctly. After quite some support from the [Luxonis](https://luxonis.com/depthai) (the company that makes the OAK-D cameras) discord community and the RPLidar ROS packages, I was able to setup both the sensors and their transforms in ROS Noetic on the RPi4. The choice of Noetic was simple, since I plan on migrating to ROS2 Foxy and they both run on the same Ubuntu 20.04 OS. For the ROS setup, I did the following:

* Build [rplidar_ros](https://github.com/adityakamath/rplidar_ros) and [depthai_ros](https://github.com/adityakamath/depthai_ros)
* Configure depthai_ros such that [depthai_image_proc](http://wiki.ros.org/depth_image_proc) can be used to register the depth image and publish a RGB pointcloud
* Configure depthai_ros to run with the OAK-D camera (default package by Rapyuta Robotics uses the [BW1097](https://docs.luxonis.com/en/latest/pages/products/bw1097/) camera) and its URDF (robot description)
* Configure rplidar_ros to run with the [RPLidar A2M8](https://www.slamtec.com/en/Lidar/A2) URDF
* Configure a test package (akros_test) to setup the transforms between the OAK-D and the RPLidar descriptions
* Configure the RViz view to display the RGB pointcloud, the laser scan, the TFs and the sensor models. 

#### Results

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_holo_top_vs_urdf.jpg" />
	<figcaption>Actual setup vs URDF visualization in RViz (values still to be fine-tuned)</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_holo_top_pointcloud.jpg" />
	<figcaption>Pointcloud from the depth_image_proc package</figcaption>
</figure>


<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_holo_top_viz.jpg" />
	<figcaption>RViz view showing the URDFs, the laser scan, depth cloud and the RGB pointcloud</figcaption>
</figure>

#### Next Steps

The next step is to implement [RTAB-Map](http://introlab.github.io/rtabmap/) (Real-time Appearance Based Mapping) using the OAK-D and use it to provide visual-inertial odometry (VIO). I expect to use these odometry measurements in combination with the lidar scan to perform SLAM. I also want the RGB camera to detect obstacles and fiducials using DepthAI and append the map created using RTAB-Map. The OAK-D also has an internal IMU although the driver software is still a work in progress. Once that has been done, I plan on using the IMU measurements with [robot_localization](http://wiki.ros.org/robot_localization) to improve the VIO. 

Meanwhile, the bottom part of the robot is still untouched. I plan on getting some help with this, hopefully a collaborator within Eindhoven itself. For the bottom part, I need to program the Arduino to drive the 4 motors at pre-defined velocities. I also need a battery system that can power the motors (/motor drivers), the Raspberry Pi 4 and the OAK-D continuously for at least an hour. I hope to use [MicroROS](https://micro.ros.org/) here on the Arduino so that I could communicate directly with the ROS2 installation on the RPi4 host.

Finally, and this is definitely a longshot, I need to bridge everything in ROS2 (also using the [ros1_ros2_bridge](https://github.com/ros2/ros1_bridge)) and use the ROS2 navigation stack ([nav2](https://navigation.ros.org/)) to perform autonomous functions on the AKROS holo platform. My immediate next step is to calibrate the OAK-D. In the images above, the RGB pointcloud and the lidar scans do not match. This is because I currently use the parameters of the BW1097 camera and the OAK-D is uncalibrated. I need to get a chessboard printout for this. Even once the camera is calibrated, the depth image being produced has incorrect units, which makes the published depth image to be scaled up. This also needs to be fixed, and I am currently waiting for more people to try it on the discord community. I expect to have more updates in the next post.
