---
layout: post
title: Filtering Pointclouds for SLAM
subtitle: Using PCL, laser_filters, and Hector SLAM
gh-repo: adityakamath/akros_test
thumbnail-img: /assets/img/akros_filters_thumb.png
share-img: /assets/img/akros_filters_thumb.png
gh-badge: [follow]
comments: true
---

In the last update, I had setup the required nodes to publish a depth image and convert that into a pointcloud message. This weekend, I worked towards filtering the noise from this depth pointcloud. I started by playing with the [Point Cloud Library (PCL)](https://pointclouds.org/) and the [ROS nodelets](http://wiki.ros.org/pcl_ros) provided by it. To cleanup the pointcloud data, I first ran it through an [SOR (statistical outlier removal) filter](https://wiki.ros.org/pcl_ros/Tutorials/filters#StatisticalOutlierRemoval) after which I downsampled the output using the [VoxelGrid filter](https://wiki.ros.org/pcl_ros/Tutorials/filters#VoxelGrid). I also ran the laser scan message through some [filters](http://wiki.ros.org/laser_filters), which filters out values with an angle range (from behind the laser scanner, which we do not want to use). The resulting filtered pointcloud and laserscan outputs can be seen below:

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_pointclouds_filtered.png" />
	<figcaption>Cleaned and downsampled depth pointcloud overlayed over filtered Laserscan data</figcaption>
</figure>

Once I had the laserscan and the pointclouds filtered according to my requirements, I set up [Hector SLAM](http://wiki.ros.org/hector_slam) using [this tutorial](https://www.youtube.com/watch?v=Qrtz0a7HaQ4) I found on Youtube. With the default setup, I now have a rudimentary mapping/SLAM application which works sufficiently well. Following images show the outputs of the Hector SLAM application on RViz. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_hectorslam_pc2.png" />
	<figcaption>Map generated using the first acquired laser scan</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_hectorslam_pc.png" />
	<figcaption>Map of my studio, generated after moving the lidar setup was moved around in the horizontal plane</figcaption>
</figure>

I still need to finish working on the depth-RGB alignment using the interfaces provided by Luxonis. I will need some help from their discord group for this, so I'm not planning on completing it this instant. Instead, I will try to tune the Hector SLAM parameters to get even better performance, and meanwhile I also plan on spending some time implementing some mobilenet applications using DepthAI and ROS. More about this in the next post.
