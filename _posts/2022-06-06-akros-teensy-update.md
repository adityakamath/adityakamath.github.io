---
layout: post
title: AKROS(2) Teensy Update
subtitle: and planning the next build
gh-repo: adityakamath/arduino_sketchbook_ros
thumbnail-img: /assets/img/akros_teensy_update_thumb.jpg
share-img: /assets/img/akros_teensy_update_thumb.jpg
gh-badge: [follow]
comments: true
---

I finally got some extra time thanks to a long weekend, so I decided to bring the AKROS robot out of its box and make some upgrades. Last week, I received two [Cytron MDD3A](https://www.robotshop.com/en/cytron-3a-4-16v-dual-channel-dc-motor-driver.html?gclid=CjwKCAjwy_aUBhACEiwA2IHHQHVZGaHLqIQZsYQgQrlNIkyMsG6BufyfBEak4E051PMAyHLbUJTHRBoCd3sQAvD_BwE) dual motor drivers which are compatible with the [Teensy 4.1 Expansion board](https://www.tindie.com/products/cburgess129/arduino-teensy41-teensy-41-expansion-board/) as I wanted. My plan was to remove the Arduino Mega and the 5V L298n 4 channel motor driver from the AKROS robot, replace it with the Teensy board and the Cytron motor drivers, then redo the wiring and modify/upload the code. I planned on using the same [ROSSerial](http://wiki.ros.org/rosserial) based code for ROS1 Noetic (ROS2/microROS is for later), but I wanted to use VSCode and PlatformIO instead of the Arduino IDE this time. Here's how it went:

### Step 1: Remove the Arduino Mega and the existing motor driver

This part went really quickly as I had done the exact same thing once before, so I knew what I had to do. I first clicked pictures of the existing wiring of the direction/speed pins, the encoder and the Neopixel pins. Once I had a picture of each cable and its original connection, I removed it. Once all cables were removed, getting the PCBs out wasn't difficult - just a few screws and both the motor driver and the Arduino were out.

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_motor_driver.jpg"/>
	<figcaption>The wiring of the original motor driver. As seen, the motor driver is attached on top of the Arduino Mega using spacers.
	</figcaption>
</figure>

### Step 2: Attach the  Teensy 4.1 board and the Cytron motor driver

The Teensy board is in the same form factor as an Arduino Mega, so it was a direct replacement. However, unlike the original motor driver, the Cytron motor drivers couldn't fit directly on top of the Arduino with just spacers as the holes didn't align. So, I had to design and 3D print an adapter piece. Now, using spacers the adapter piece is assembled on top of the Teensy board and then the Cytron motor drivers are mounted on top of the adapter piece. Since the new motor drivers don't have massive heat sinks like the old one, the entire assembly fits in the base module of the robot. All the header pins of the Teensy board are accessible with angled connectors, but with straight Dupont connectors, you need to bend the pin a little bit when the adapter piece is present. I could make some changes to the design to make space for the connectors, but for now it works.

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_teensy_cytron_adapter.PNG"/>
	<figcaption>The entire assembly but with an Arduino Mega instead of the Teensy 4.1 only for illustration. The adapter is shown (in off-white), between the Arduino Mega and the two Cytron MDD3A motor drivers.
	</figcaption>
</figure>

### Step 3: Wiring

This is the part which I thought would be a piece of cake, but it wasn't. What I did not consider is that the MDD3A motor drivers are controlled using two PWM pins, and not two digital pins and one PWM pin like the L298n motor drivers. So, I couldn't just redo the wiring with the same pins as before. Fortunately this did not affect all pins - the encoder pins and the Neopixel pins remained in mostly the same place. As for the motor driver, it used four fewer pins, so it did make some things easier for me. Once again, I meticulously clicked pictures of every step so that I knew not only the connection but also the corresponding pin numbers. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/akros_cytron_motor_driver.jpg"/>
	<figcaption>The Cytron motor drivers shown on top of the white adapter piece, attached to the Teensy 4.1 Expansion board below. The wiring could definitely be neater. To connect this to the Raspberry Pi, I am using a micro-USB cable that angles up, so the cable does not pop out from the side. However, the opening is a really useful access point to the USB host and the Ethernet connection. But while its not used, I might add a 3D printed part to close this opening.
	</figcaption>
</figure>

### Step 4: Fixing the code

Once the wiring was done, it was time to fix the code. First thing I had to do was change my [config file](https://github.com/adityakamath/arduino_sketchbook_ros/blob/main/akros_teensy_pio/include/akros_holo_teensy_config.h) - I fixed the pin numbers for the Neopixel pin, the encoder pins and the motor driver pins. In this new case, each motor x has a Mx_a and Mx_b pin instead of also having a Mx_en pin. So, the Mx_en pins for each motor were also removed. Now, the functionality needs to be changed. In the [original code](https://github.com/adityakamath/arduino_sketchbook_ros/blob/main/akros_holo_drive/akros_holo_drive.ino), I have a function called spin_motor(int motor, double velocity) which rotates a specific motor according to an input velocity. Based on the direction, the digital directional pins are set and the absolute speed is written to the PWM enable pin. In the new code, the directional pins also set the PWM, so [the code was changed](https://github.com/adityakamath/arduino_sketchbook_ros/blob/main/akros_teensy_pio/src/main.cpp) and the enable pins were removed. The code difference can be seen here:

```
//Original Code:
//Writes digital/PWM values to motor direction/enable pins for the specified motor
void spin_motor(int motor, double velocity){
  if(velocity > 0 ){
    digitalWrite(inPins[2*motor], HIGH);
    digitalWrite(inPins[2*motor + 1], LOW);
  }
  else{
    digitalWrite(inPins[2*motor], LOW);
    digitalWrite(inPins[2*motor + 1], HIGH);
  }
  int motor_pwm = constrain(abs((int)velocity), MIN_PWM, MAX_PWM);
  analogWrite(enPins[motor], motor_pwm);
}
```


```
//New Code:
//Writes PWM values to motor pins for the specified motor
void spin_motor(int motor, double velocity)
{
  int motor_pwm = constrain(abs((int)velocity), MIN_PWM, MAX_PWM);
  if(velocity >= 0)
  {
    analogWrite(inPins[2*motor], motor_pwm);
    analogWrite(inPins[2*motor + 1], 0);
  }
  else
  {
    analogWrite(inPins[2*motor], 0);
    analogWrite(inPins[2*motor + 1], motor_pwm);
  }
}
```

### Step 5: Uploading and testing

Uploading the code to the Teensy was extremely straightforward. Add the used libraries to [platformio.ini](https://github.com/adityakamath/arduino_sketchbook_ros/blob/main/akros_teensy_pio/platformio.ini) or the lib directory, add the config file to the include directory, and add the Arduino code to main.cpp within the src directory, with a ```#include <Arduino.h>``` on the top. Then simply build and upload the code. I have created a [template repository on Github](https://github.com/adityakamath/rosserial_teensy_pio) what you can use to create your own ROSSerial/PlatformIO projects. However, it is important to note that I am using a [modified rosserial_arduino_lib](https://github.com/adityakamath/rosserial_arduino_lib), if you want to use the original libraries (by Joshua Frank, and can be found [here](https://github.com/frankjoshua/rosserial_arduino_lib), please replace the link in platformio.ini.

However, I did have some concerns. In the past, when I have uploaded rosserial code from the Arduino IDE on my Windows 10 laptop, ROS Noetic has troubles connecting to the board and sometimes also displays the 'version mismatch' error. In this situation, I have usually found the Arduino installation on my RPi or Jetson Nano to be most reliable as they to link the correct ROSSerial libraries. I thought I could have the same issues with PlatformIO, but clearly I was mistaken. Everything works as expected, and every time I make some changes to the code or the rosserial_arduino_lib, nothing breaks on Windows 10 or when connected to ROS Noetic. There also seems to be no bandwidth issues as all publishers and subscribers were functioning really well all the time.

Once everything was working, I also made a small video showing the new motor drivers in action. The motor and drivers make a different sound and its probably more silent than with the previous motor drivers. The motor drivers also come with one green LED each to indicate power, and four red LEDs to indicate direction (one for each motor and direction). Thanks to the translucent walls, it makes the base module light up even more as shown in the video below. While some people seem to like it, I'm already considering new parts with black acrylic.

[![Cytron Motor Driver Test](https://adityakamath.github.io/assets/img/akros_cytron_test_ss.PNG)](https://www.youtube.com/watch?v=pjsmB1NON2o "AKROS Cytron Motor Driver Test")

### Step 6: Transition to ROS2

While my plans for the AKROS robot and the Teensy board is to implement [microROS](https://micro.ros.org/) with [ROS2 Galactic](https://adityakamath.github.io/2022-06-01-ros2-setup-on-windows/), it is still a long way off. I'm currently learning and experimenting with ROS2 and still need some time to start implementing it on my robot. I was planning on using the Nanosaur to learn ROS2 but now I have a few problems - the Jetson Nano does not turn on. I tried multiple base boards and also different expansion shields but nothing worked. I have two power banks and neither of them work. The second issue is still the 3D printed front cover - still haven't had the time to print a replacement for it. Meanwhile I started by forking and building the linorobot2 project alongside the ROS2 demos and examples. Since the linorobot2 can also be configured to run on a mecanum wheeled platform like mine, I decided to continue using this as a tool for learning ROS2 and also as a reference for porting the AKROS stack to ROS2. So putting the Nanosaur aside for now, I can certainly reuse the parts. The Jetson Nano also seems to work fine with an external power supply, so that can be reused too.

 Meanwhile, I also have some intermediate goals with the AKROS robot. For starters, I have a microSD card reader on the Teensy on which I have a 16GB microSD card. I want to experiment with this, and use it to potentially store the config file or for data like diagnostic logs or trajectories. Secondly, I want to add an IMU to the Teensy board to improve odometry from the low level controller. When the robot gets stuck, the motors can still rotate freely, which triggers the encoders and hence the odometry estimate is no longer correct. The IMU can fix this as it can detect that the robot has not moved and correct the encoder odometry estimate. Finally, I want to interface a (Flysky) RC receiver to it so that I can control the base module without the Raspberry Pi. The goal here is to have the Raspberry Pi only for the navigation module and SLAM, while the base module is responsible for motion control, teleoperation and some local state estimation.

## The next build

For my next build, I definitely wanted to make a bigger robot and work with some BLDC motors like hoverboard hub motors. However, I don't have a lot of space and its still a few months till I move into a bigger apartment. So for now, the next build has to be smaller. Since the last few months, I have also been working on a low-cost, miniature version of the AKROS robot. I want to take my time and be really detail oriented with this one as I want to potentially start selling it as a learning tool. I also want to keep it simple and use a microcontroller instead of a Raspberry Pi or an Nvidia Jetson device, as I want this robot to be controlled over wifi using an external host computer. While I have worked out most of the details, one thing that I'm still struggling with is the microcontroller selection. I've been playing around with some microcontroller platforms this past week, like - [ESP32](http://esp32.net/), [Raspberry Pi Pico RP2040](https://www.raspberrypi.com/documentation/microcontrollers/raspberry-pi-pico.html), the Teensy 4.1 (with the expansion board) and the [Arduino Portenta H7](https://store.arduino.cc/products/portenta-h7)

The Teensy 4.1 board is for the AKROS2 project I explained earlier, so I will ignore this for now. The Portenta is definitely overkill for this project, so although I've tested that it works, it is going back in its box. I'm now left with the ESP32 and the RPi Pico, and I've been struggling to choose between the two - mainly because of Wifi and Bluetooth which the ESP32 provides but the RPi Pico does not. On the other hand, there's the [Arduino Nano RP2040 Connect](https://docs.arduino.cc/hardware/nano-rp2040-connect), which is a RPi Pico with a Wifi/Bluetooth module and a few extra features put together by Arduino. It costs twice as much as some ESP32 boards (even more for some boards), but it comes with an onboard microphone, a six axis IMU, 16MB Flash, and is also possibly more reliable than ESP32 in terms of documentation, support and community. The Nano RP2040 also costs as much as a [RPi Zero 2 W](https://www.raspberrypi.com/products/raspberry-pi-zero-2-w/) + a RPi Pico + IMU (including tax and shipping), which is a combination I can try, but the RPi Zero 2 W and the popular IMU chips are [out of stock literally everywhere](https://www.youtube.com/watch?v=u1vuz8EtC9s). I still have to make some decisions here, maybe I can start by drawing up a pros and cons list. Meanwhile, I have been busy trying to design the PCB for this project, I've got the schematic for the four channel motor driver, the battery/charging system as well as the sensors which will be added to the robot. Only thing remaining is the microcontroller which I still need to decide. 

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/makros_microcontroller_test.png"/>
	<figcaption>The microcontroller boards that I've been using to test VSCode, PlatformIO and microROS. On the top left is the Adafruit Macropad RP2040 which has a RPi Pico RP2040 onboard alongside a screen, 4x3 key matrix, a rotary encoder and Neopixel LEDs. On the top right is the M5Stack Core Gray, which has an ESP32, a screen with buttons, an IMU, a speaker and exposed GPIO pins. This is stacked on top of a quad motor and encoder driver module by M5Stack. On the bottom is the Arduino Portenta Breakout Board with an Arduino Portenta H7 Lite Connected attached on top.
	 </figcaption>
</figure>

This week, I also received two new microcontroller devices - the Arduino Nano RP2040 Connect, and the Raspberry Pi Pico. I still haven't had the time to play with them yet, although I did click some pictures and also used up the stickers the Arduino came with.

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/makros_arduino_rpi_rp2040.jpg"/>
	<figcaption> Arduino Nano RP2040 Connect (top) and a pack of 5 RPi Pico (bottom). The Arduino came with stickers that I hadn't seen for a very long time.
	</figcaption>
</figure>

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/makros_comparison_rp2040.jpg"/>
	<figcaption> Size and pinout comparison between the Arduino Nano RP2040 and the RPi Pico. Both have the same chip but the Arduino Nano RP2040 uses some of the pins for Wifi, IMU and the inbuilt microphone, so fewer pins are exposed and the board is more compact.
	</figcaption>
</figure>

I have also been testing various [N20 micro metal gear motors](https://www.adafruit.com/product/4640) with different gear ratios and speeds, and I've ended up selecting some 100RPM motors + encoders with some decent torque. I also looked for some mecanum wheels, and found some [really nice ones on Pimoroni](https://shop.pimoroni.com/products/mecanum-wheels-pack-of-4?variant=31590631997523). They're made for the N20 motors, come in a pack of four (which is convenient) and are of really good quality. However, they are quite pricy (at around 24 Pounds for the set of 4 + shipping), and its a shame they dont sell them individually or in Left/Right pairs. I broke one of them due to my own fault and had to buy the entire set of 4 again.

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/makros_motors_wheels_battery.png"/>
	<figcaption> 100 RPM motors with Pimoroni mecanum wheels attached, and a 6V 2300mAh NiMH battery to power the entire system. 
	</figcaption>
</figure>

For the next few weeks, I will continue playing around with ROS2 on Windows/VSCode and microROS using VSCode/PlatformIO. I have some ROS extension issues to fix in VSCode, and then I will also be able to use the extension for sourcing the underlay/overlays and building my workspace. I also want to test out several microcontrollers with PlatformIO and microROS - the Arduino Portenta H7 (for the legged robot that I haven't forgotten about), the Raspberry Pi Pico, the ESP32 (using an [M5Stack Core Gray](https://shop.m5stack.com/products/grey-development-core)), and an Arduino Nano RP2040 (these last three for the miniature AKROS robot). More updates next week...

<figure class="aligncenter">
	<img src="https://adityakamath.github.io/assets/img/makros_proto_design.png"/>
	<figcaption> CAD of the mini AKROS robot using Fusion 360. The space in the middle is for the battery, the round holes are for threaded inserts on which either PCBs (controller board, sensors) or the external shell of the robot will be attached. The square holes on either end is for a cliff sensor which will be attached facing down.
	</figcaption>
</figure>