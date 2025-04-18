---
layout: post
title: Visualizing DepthAI and SLAM outputs
subtitle: Pointclouds + Map + Fiducials + Object Detections
gh-repo: adityakamath/depthai_nodes
thumbnail-img: /assets/img/akros_foxglove_thumb.png
share-img: /assets/img/akros_foxglove_thumb.png
gh-badge: [follow]
comments: true
---

Last weekend, I really got playing with [Foxglove Studio](https://foxglove.dev/docs) - a really cool app to visualize ROS topics/messages from a remote desktop. It works on Windows, which is a real plus because I dont have to go through the process of launching ROS from windows, then setting environment variables to connect to the remote ROS master, and then launching Rviz. Foxglove does all this for me!

I started with [my depthAI ROS example](https://github.com/adityakamath/depthai_nodes) on based on [this repo](https://github.com/luxonis/depthai-ros-examples) from Luxonis. My example published the depth output, spatial object detection output (detections in 3D world coordinates, 2D bounding boxes), RGB image used by the neural network, and a full frame RGB camera output. From previous weekends, I had a URDF for the RPLidar + OAK-D setup, which made it easy to setup a launch file to publish the lidar scan, the depthAI outputs in their correct frames with all the correct transforms between these frames. These launch files are in my AKROS_test repo [here](https://github.com/adityakamath/akros_test). The RPLidar + OAK-D setup is seen in the image below:

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_holo_assembly_top2.jpg" />
	<figcaption>AKROS test setup</figcaption>
</figure>

In this launch file, I added a few other nodes/nodelets: First, [depth_image_proc](http://wiki.ros.org/depth_image_proc) to convert the depth image to a ROS compatible pointcloud. This pointcloud is downsampled and filtered. Second, [hector_slam](http://wiki.ros.org/hector_slam) which uses the RPLidar scan to create a map and localize within it. Third, a converter node to convert the full frame RGB image from an NV12 encoding to a ROS compatible encoding (BGR8). This converter also subscribes to the object detection output and publishes a new image with bounding boxes drawn. This can be turned off using a launch parameter. Finally, this ROS compatible full-frame image is used by [aruco_detect](http://wiki.ros.org/aruco_detect), which detects pre-determined fiducials from the RGB image and provides transforms to the detected fiducials. As in the converter node, a launch parameter can be used to publish images with the fiducial detections drawn. All of this was visualized using Foxglove as can be seen in the video below:
  
[![OAK-D + RPLidar - SLAM, Object Detection Demo](https://adityakamath.github.io/assets/img/akros_foxglove_demo_ss.png)](https://www.youtube.com/watch?v=J-kTdkJawAM "[OAK-D + RPLidar - SLAM, Object Detection Demo - Click to Watch!")
  
My next step is to make nodelets of everything since there is a lot of big pointclouds and images being moved/copied around, which could easily be just a pointer instead. I've already created a nodelet from the DepthAI nodes, and am currently working on making the converter node into a nodelet. I am also waiting for some more support from Luxonis about their depth_align features (that publishes depth image aligned with the RGB camera frame), and also the on-board IMU. After that, I plan on taking a break from this project and learn some ROS2. Maybe as an exercise I can convert/migrate my existing work to ROS2. But my main goal with ROS2 is to work with microROS. I have also been playing with some ESP32 projects/development boards/kits (and some PCB designing) and would love to implement microROS in these projects. Here's a sneak peek of what I'm working on, more info in a separate post about this soon..
  
<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/esp32_sneak_peek.jpg" />
	<figcaption>AKROS micro (?) test setup with an ESP32-Devkit C and 4 DC motors</figcaption>
</figure>
