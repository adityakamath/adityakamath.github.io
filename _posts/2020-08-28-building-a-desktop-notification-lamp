---
layout: post
title: Desktop notification lamp - Planning
subtitle: With Arduino MKR 1010 and a NeoPixel LED ring
gh-repo: adityakamath/pinguino
thumbnail-img: /assets/img/neopixel_planning_thumb.png
share-img: /assets/img/neopixel_planning_thumb.png
gh-badge: [star, fork, follow]
tags: [neopixel, arduino, iot, electronics, software, planning]
comments: true
---

Ever since I've started working from home (since mid-March), I have stopped using my phone while I'm working. It is usually lying on my bedside table, but I am able to use apps like Discord and Whatsapp using their web interfaces. However, I do miss a lot of notifications since I'm working and mostly in meetings. On the internet, I did find a lot of these devices that notify you when you get a notification on your phone, there are also apps that show your phone notifications on your desktop. I definitely wanted a physical solution instead of an app, and I decided to go ahead and build one myself. 

I had a [NeoPixel]() ring with 24 LEDs from a previous project which I decided to use for the project. I also had a brand new [Arduino MKR Wi-Fi 1010]() which I also decided to use for this project. Arduino MKR Wi-Fi 1010 is designed for IoT applications over Wi-Fi, so this was perfect for this project. I also quickly designed some parts on Fusion 360 to mount the Arduino and the NeoPixel ring and sent them off for laser cutting. 

Next step was to plan what exactly to implement. This is what I came up with:

To start with, I need a user interface. Although the system is supposed to just access my notifications over the internet and turn the lights to a particular color, I also need a user interface with the following options:
1. A power on/off button
2. A slider button to control the intensity
3. A RGB slider to control the color of the light
4. A lamp face selector - I plan on making different lamp faces like a clock, some animated lights, solid color or just my discord status.
5. Buttons to enable/disable certain notifications - during work hours, I really dont care about Facebook or Instagram messages so I would like to ignore these when I want.
6. Some kind of real time clock that provides accurate time to the Arduino

I used the [Blynk]() app for this, which made the entire UI process extremely quick. I ended up using all my app credits to add the different UI features, but I don't mind paying for more features down the line. For now, the free version is good enough. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/pinguino_blynk_interface.png" />
	<figcaption>Blynk User Interface with on/off button, real-time clock, offline notification, lamp-face chooser, discord notification button, intensity slider and a RGB selection zebra</figcaption>
</figure>

I also tested the hardware to make sure they are working. The NeoPixel ring works with only 3 connections - power, ground and input signal, which I soldered to the LED board. These wires were connected to the 5V, GND and PIN6 respectively on the Arduino MKR board. I ran the sample code from the NeoPixel library and also managed to make some of my own functions to test each LED. Here are some of the cool pictures I ended up with:

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/pinguino_neopixel_test1.png" />
	<figcaption>NeoPixel LED ring connected to the Arduino MKR Wi-Fi 1010</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/pinguino_neopixel_test2.png" />
	<figcaption>NeoPixel ring at full intensity</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/pinguino_neopixel_test2.png" />
	<figcaption>Long exposure art using the NeoPixel ring</figcaption>
</figure>

Meanwhile, I also found a spare NeoPixel 8 LED strip, with which I also got some really cool photographs. This LED strip went on to the ROS Car to function as a status indicator. More will be explained in a later post. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/pinguino_neopixel_test0.png" />
	<figcaption>Arduino MKR with a NeoPixel 8 LED strip</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.com/assets/img/pinguino_neopixel_stripart.png" />
	<figcaption>Light painting using the NeoPixel LED strip</figcaption>
</figure>

For this notification lamp, my next steps are divided into three:
1. Test the integration of the Arduino MKR with the Blynk app
2. Test the functionality of the Blynk app integration
3. Set up a Discord webhook and design the functions for displaying the status as a lamp face or notifications when they arrive. 

More in the next post. I hope laser cut parts arrive soon, I paid for standard shipping, so its going to take a while. 
