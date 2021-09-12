---
layout: post
title: Camera Calibration and Fiducial SLAM
subtitle: Using Aruco markers + Foxglove
gh-repo: adityakamath/akros_test
thumbnail-img: /assets/img/akros_fiducial_thumb.png
share-img: /assets/img/akros_fiducial_thumb.png
gh-badge: [follow]
comments: true
---

This week, I accomplished quite a few things. First, I installed a dev tool called [Foxglove](https://foxglove.dev/docs) on my Windows laptop. This tool is essentially [Rviz](http://wiki.ros.org/rviz) (+ some additional features) for Windows and connects remotely to a ROS machine. This saved me a lot of time, since I dont need to setup a ROS machine on my windows PC and start Rviz from there, but not all features always work. Foxglove doesn't have all the features of Rviz just yet, but since its an open source project, I hope to see some new developments in the near future. For the time being, I can still work with it. Here's the [Hector SLAM example](https://adityakamath.github.io/2021-04-19-pointcloud-laserscan-filters/) being visualized using Foxglove:

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_hector_slam.png" />
	<figcaption>The Hector SLAM map, Lidar scan and aruco detections (transforms) visualized in 3D using Foxglove. On the top right is an image feed from the RGB camera of the OAK-D </figcaption>
</figure>
  
Next, I printed some Aruco markers and used the [aruco_detect node](https://github.com/UbiquityRobotics/fiducials/tree/kinetic-devel/aruco_detect) from [this ROS package by Ubiquity Robotics](https://github.com/UbiquityRobotics/fiducials) to detect the Aruco markers that I placed around my studio. This is when I realized that the aruco markers were not being detected in the exact location they were actually in. On debugging, I realized that the OAK-D camera is originally calibrated using a 480p image. Since I was using a 720p image, I had to calibrate the camera using the correct images, and I did that using the [camera_calibration node](https://github.com/ros-perception/image_pipeline/tree/noetic/camera_calibration) from the [image_pipeline package](https://github.com/ros-perception/image_pipeline). I needed a checkerboard and the camera_calibration node provided a handy GUI, so I had the camera calibrated in no time. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_camera_calibration.jpg" />
	<figcaption>Calibrating the RGB camera using the camera_calibration package and a checkerboard.</figcaption>
</figure>
  
Once calibrated, the aruco markers were being detected in the correct location. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_aruco.png" />
	<figcaption>Aruco detections visualized using Foxglove. The correct calibration parameters are used, and fiducials are detected in the correct positions</figcaption>
</figure>
  
Finally, from the same package of Ubiquity Robotics, I used the [fiducial_slam node](https://github.com/UbiquityRobotics/fiducials/tree/kinetic-devel/fiducial_slam) and configured to work with the aruco detections. This package uses these detections to create a map, which it then uses to localize. Here are images of the robot mapping and localizing:

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_aruco_slam.png" />
	<figcaption>The fiducial SLAM package uses the Aruco detections and creates a map by drawing links (blue lines) between fixed markers</figcaption>
</figure>
  
<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_aruco_localized.png" />
	<figcaption>Localization is done by relying on the map and the detected markers. In this image, 3 markers (in green) are detected, the SLAM package then places the other markers according to the map.</figcaption>
</figure>
  
For the next few weeks, I want to work with the mobilenet detection and spatial detection nodes from the [depthai_ros_examples package](https://github.com/luxonis/depthai-ros-examples) by Luxonis. I want to accomplish two things: first, to publish transforms for each detection, and second to publish a preview image with the bounding boxes drawn. But first, I want to make a nice demo video of what I've done so far. Meanwhile, I am also doing some ROS2 tutorials and hope to start making some small ROS2 projects soon. I also recently purchased some ESP-32 boards for side-projects and ever since I read about [microROS on an ESP-32 DevKit-C](https://micro.ros.org/blog/2020/08/27/esp32/), I have been dying to try them out with some ROS2 robots. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/esp32_kits.jpg" />
</figure>
[M5Stack Core Grey](https://shop.m5stack.com/products/grey-development-core), [Open-SmarWatch](https://open-smartwatch.github.io/) and a [TT-Go](http://www.lilygo.cn/prod_view.aspx?TypeId=50033&Id=1126&FId=t3:50033:3). I also have an ESP-32 DevKit-C as well, but not in the picture.
  
I had originally planned to implement RTABMap in the next few months, but looks like this will take a while. I'm still waiting for support from Luxonis about publishing disparity instead of depth, and about depth-RGB alignment. Hopefully by the time I'm done learning about ROS2, microROS, they have some updates and I can implement RTABMap. I'll also probably upgrade to a RPi4 8GB or a Jetson device by then, for better performance.
