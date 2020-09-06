---
layout: post
title: ROSCar v2 - Final hardware updates
subtitle: Camera mount + New antenna
gh-repo: adityakamath/ros1bot
gh-badge: [star, fork, follow]
thumbnail-img: /assets/img/roscar_update_thumb.jpg
share-img: /assets/img/roscar_update_thumb.jpg
tags: [roscar, robotics, electronics, chassis, build]
comments: true
---

Finally after a month of waiting, I received the Wi-Fi antenna which I ordered from Amazon. I was finally able to replace my bulky antenna with these tiny ones. Using zipties, I simply attached them to either side of the base plate as shown below.

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/roscar_update_ant1.jpg" />
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/roscar_update_ant2.jpg" />
</figure>

I also got a Raspberry Pi camera mount which was GoPro compatible. I found the STL file on thingiverse but I then broke it down to laser-cuttable files which I then cut alongside the parts of my NeoPixel lamp. I then glued the parts together and attached the Raspberry Pi camera. Unfortunately I did not have any compatible hex nuts so I used a glue gun for now. This can be seen in the image below. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/roscar_update_camera.jpg" />
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/roscar_update_gopro.jpg" />
</figure>
	
Finally, I still have one part which I need to 3D print when I next go to work. This is a tiny support part for the Pi OLED display which currently moves around a bit, but its functional for now. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/roscar_update_led.jpg" />
</figure>

I also added a NeoPixel LED strip to the underside of the Pi OLED PCB. Its for future purposes to indicate the current state of the car, but for now it looks really cool! Then I reoriented the servo driver and the IMU and mounted them to the bottom of the base-plate instead of the top. This made the entire assembly a lot more compact. Finally, I replaced Ice cooling tower with the original heat-sink and fan. This application does not heat up the Jetson Nano too much, and also its easier to access the wiring this way. The final results can be seen in the images below:

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/roscar_update_final2.jpg" />
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/roscar_update_final.jpg" />
</figure>

