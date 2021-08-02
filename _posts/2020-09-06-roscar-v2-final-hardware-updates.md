---
layout: post
title: ROSCar v2 - Final hardware updates
subtitle: Camera mount + new antenna + other updates
gh-repo: adityakamath/akros_jetson
gh-badge: [follow]
thumbnail-img: /assets/img/roscar_update_thumb.jpg
share-img: /assets/img/roscar_update_thumb.jpg
tags: [roscar, robotics, electronics, chassis, build]
comments: true
---

Finally after a month of waiting, I received the Wi-Fi antenna which I ordered from Amazon. I was finally able to replace my bulky antenna with these tiny ones that actually fit on the car. Using zipties, I simply attached them to either side of the base plate as shown below.

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/roscar_update_ant1.jpg" />
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/roscar_update_ant2.jpg" />
</figure>

I also got a Raspberry Pi camera mount which was GoPro compatible. I found the STL file on thingiverse but I then broke it down to laser-cuttable files which I then cut alongside the parts of my NeoPixel lamp. I then super-glued the parts together and attached the Raspberry Pi camera with bolts. Unfortunately I did not have any compatible hex nuts so I used a glue gun for now. This can be seen in the image below. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/roscar_update_camera.jpg" />
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/roscar_update_gopro.jpg" />
</figure>
	
Finally, I still have one part which I need to 3D print when I next go to work (I'm still working from home, mostly till the end of this year). This is a tiny support part for the OLED display which is not needed for now but definitely good to have since the screen moves around a bit.

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/roscar_update_led.jpg" />
</figure>

I also added a NeoPixel LED strip to the underside of the OLED PCB. For now, it looks really cool but in the future I intend to program it to display the current status of the car. Then, I also reoriented the servo driver and the IMU and mounted them to the bottom of the base-plate instead of the top. This made the entire assembly a lot more compact. Finally, I replaced Ice cooling tower with the original heat-sink and fan. This application does not heat up the Jetson Nano too much so the Ice cooling tower is not needed anyway (probably for a different project), and also its easier to access the wiring this way. The final results can be seen in the images below:

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/roscar_update_final2.jpg" />
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/roscar_update_final.jpg" />
</figure>

This month I am building tutorial applications on the Jetson Nano and sometime in October I plan on setting up a track and driving autonomously. Meanwhile, there's work to be done on the Pinguino lamp.
