---
layout: post
title: Assembling the ROS Car version 2
subtitle: ...with new (self-designed) acrylic parts
gh-repo: adityakamath/akros_jetson
gh-badge: [follow]
thumbnail-img: /assets/img/roscar_assembled_v2_thumb.png
share-img: /assets/img/roscar_assembled_v2.png
tags: [roscar, robotics, electronics, chassis, build]
comments: true
---

This post is not really a blog, more a series of photos. Over the last month, I have been playing around with Autodesk Fusion 360 and getting some real experience with designing my own parts. Last week, I was finally pretty satisfied with my design for new base-plates and decided to get them laser cut in acrylic. Also, my old cardboard base-plate was bending due to the weight of the battery pack. 

The design is three-tiered as in the image below. On the bottom most tier, the I2C servo board, the IMU and the battery pack are attached with nylon standoffs and zip ties. On the layer above it, the Jetson Nano and the Arduino Nano are attached. The camera is also to be mounted on this tier, but for now I have a GoPro mount as a placeholder. The topmost tier holds a small prototype board with the PiOLED display, connected to the Jetson Nano via I2C. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/roscar_lasercut_design_v2.png" />
	<figcaption>Fusion 360 screen-capture that shows the design files and how they will be assembled</figcaption>
</figure>
	
I had originally planned on using the laser cutter at work, but due to the Covid-19 pandemic, this was not possible. This is where I found [Snijlab.nl](https://www.snijlab.nl). Within minutes, I was able to upload my design file, choose my material, and pay for the order and shipping. I placed the order on the weekend and I received the parts by Wednesday! 

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/roscar_lasercut_plate.png" />
	<figcaption>The 300x200mm acrylic plate that arrived from Snijlab.nl with the parts cut out. I asked them to tape the bottom so that the laser does not damage the look of the material.</figcaption>
</figure>
	
<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/roscar_lasercut_parts.png" />
	<figcaption>The parts, removed from the plate. This is the actual color of the acrylic, the taped part is on the bottom.</figcaption>
</figure>
	
<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/roscar_lasercut_assembly_wrong.png" />
	<figcaption>The parts assembled with M2.5 screws and hex standoffs (brass and nylon). If you see, this is slightly different to the assembly in the design. This is where I realized that I had assembled the pieces upside down and that I had accidentally asked Snijlab to tape the wrong side.</figcaption>
</figure>
	
<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/roscar_lasercut_assembly_withparts_tape.png" />
	<figcaption>I reversed the parts and this is what the assembly looks like, with all the components attached.</figcaption>
</figure>
	
<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/roscar_assembled_tape.png" />
	<figcaption>The assembled car with everything wired up. I never realized the LEDs on the fan still powered on when the fan was turned off. Makes the car look so much cooler!</figcaption>
</figure>
	
While I was satisfied with the parts, the fact that I had that tape was still bugging me. So, with the scrap acrylic parts, I decided to try and remove the adhesive tape. It took some agressive scratching with a screwdriver but I was finally able to remove most of the adhesive. So, I disassembled the car and removed the adhesive tape. It took a while but it worked. I clicked a new set of pictures to show the different steps of assembly. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/roscar_subassemblies_notape.png" />
	<figcaption>All the tiers with the components attached, with the tape removed from the acrylic parts</figcaption>
</figure>
	
<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/roscar_tier1_attached.png" />
	<figcaption>The car with tier 1 attached. The steering and throttle cables are attached to the I2C servo board. The IMU is attached to the I2C bus on the servo board.</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/roscar_tier2_attached.png" />
	<figcaption>The car with tier 2 attached. The Jetson Nano is connected to the Arduino with a USB cable. The odometry cables will be soldered on to the Arduino prototype board (to be attached). The joystick receiver is connected to the Jetson Nano USB port and placed in the empty space on tier 1.</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/roscar_tier3_attached.png" />
	<figcaption>The car with tier 3 attached. The PiOLED PCB is connected to the Jetson Nano I2C bus (GPIO pins), it is also connected to the I2C servo board with jumper cables. </figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/roscar_assembled_v2.png" />
	<figcaption>Once everything is connected, the Jetson Nano is powered and everything lights up, which means that the wiring is correct. A quick check on the software shows all nodes running as expected.</figcaption>
</figure>
	
Finally, I have 3 hardware updates still to make. Firstly, I need to design and print a mount for the WiFi antenna. Next, I need to add a prototype board with a LED/push-button interface for better control, and then design and print a mount for it. I also need to design and print a GoPro compatible mount for the camera. Finally, once I get started with the ML part, I will have an idea of the perfect orientation of the camera. I can then design and print a fixed camera mount instead of the GoPro links. 

The next blog post is probably going to be in a few months from now. I plan on taking it easy and getting some outdoors time on the weekends, since the weather is so nice. 

