---
layout: post
title: Pinguino assembly guide
subtitle: Putting it all together
gh-repo: adityakamath/pinguino
gh-badge: [star, fork, follow]
thumbnail-img: /assets/img/pinguino_build_thumb.jpg
share-img: /assets/img/pinguino_build_thumb.jpg
tags: [pinguino, iot, neopixel, arduino, electronics, chassis, build]
comments: true
---

Finally, after almost a week, my laser cut parts arrived. I already had everything else needed, so it was time to get started with the assembly. First, I attached M2 standoffs on the Arduino MKR Wi-Fi 1010 as shown below. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/pinguino_build1.jpg" />
</figure>

Next, I soldered jumper cables to the PWR, GND and INPUT pins of the NeoPixel ring. The image below shows the parts required

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/pinguino_build2.jpg" />
</figure>

Now, the circular acrylic part is attached to the nylon standoffs on top of the Arduino. The NeoPixel ring is attached on top of it using a glue gun. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/pinguino_build3.jpg" />
</figure>

Next step is to connect the NeoPixel ring to the Arduino. PWR, GND, INPUT is connected to 5V, GND and PIN6 on the Arduino.

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/pinguino_build4.jpg" />
</figure>

The entire assembly is placed upside down and then the base-plate is attached with nuts as shown below. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/pinguino_build5.jpg" />
</figure>

Next, M3 standoffs are connected to the base-plate as shown below. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/pinguino_build6.jpg" />
</figure>

The assembly is now rotated and the top-plate is attached with 4xM3 screws.

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/pinguino_build7.jpg" />
</figure>

Once everything is connected, the Arduino can be plugged in to USB. The image below shows a capture of the rainbow-cycle function as described in the last post. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/pinguino_build8.jpg" />
</figure>

Here's a small video of Pinguino in action:

[![Assembled Pinguino: Rainbow Theatre Chase](https://adityakamath.github.com/assets/img/pinguino_build_ss.png)](https://www.youtube.com/watch?v=EFQ527DgCro "Pinguino fully assembled - Click to Watch!")

For the next time, I plan on implementing some more functionality to the lamp, especially the ability to control it via Discord. But I really hope to get started with the ROSCar again since I have been waiting for some small parts and they are in transit. 
