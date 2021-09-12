---
layout: post
title: Controlling Pinguino with Blynk
subtitle: And trying out Discord webhook integration
gh-repo: adityakamath/pinguino
thumbnail-img: /assets/img/pinguino_blynk_thumb.jpg
share-img: /assets/img/pinguino_blynk_thumb.jpg
gh-badge: [follow]
comments: true
---

So, I decided to call my desktop notification lamp 'Pinguino'. As in, Ping my Arduino. Over the last two days, I integrated the Blynk GUI that I talked about in the last blog. For this weekend, I wanted to achieve two things: first, to control the NeoPixel ring with the Blynk App. Secondly, for every change made from the Blynk GUI, I want to send a message to a Discord server. The following sections talk about how I did these tasks.

#### Controlling the Arduino MKR Wi-Fi 1010 and NeoPixel ring with Blynk

I first started with running the RGB ring test code in the examples of the NeoPixel library. Once that worked well, I designed the Blynk GUI on the app. These are the features of the app:

1. Power on/off button: digital output to Virtual Pin 0 (VPIN0) - value: 0 when off, 1 when on
2. Lampface dropdown menu: digital output to VPIN1 - value: between 1 and 9 for the various menu items
3. Discord input button: digital output to VPIN2 - value: 0 when off, 1 when on
4. Slider input: digital output to VPIN3 - value: between 0 and 255
5. RGB input: digital output to VPIN4 - value: array of 3 elements, each between 0 and 255

The next step was to read the VPINs from the Arduino MKR board and assign these values to specific functions. A good starting point to do this was the [PuzzleBox](https://create.arduino.cc/projecthub/Arduino_Genuino/puzzlebox-with-mkr-wifi-1010-7a39c4) example on the Arduino Project Hub. 

[![Pinguino Blynk Test](https://adityakamath.github.io/assets/img/pinguino_blynk_ss.png)](https://www.youtube.com/watch?v=Qy86EHXYYbk "Pinguino Blynk Test - Click to Watch!")

#### Sending the Arduino/NeoPixel status info to Discord

Once I had the first task set up, the next step was to send status messages to Discord. Once again, I relied on the Arduino Project Hub to realize this task. I found [this tutorial](https://create.arduino.cc/projecthub/labsud/send-a-message-on-discord-f216e0?ref=search&ref_id=discord&offset=0) to send messages easily from the Arduino MKR 1010 to the Discord Server. I created a webhook on my Discord server, and assiged it to its own private channel which will log the status of the Pinguino. Aptly, I named this webhook as Cobblebot (after Oswald Cobblepot AKA The Penguin). Then, it was easy using the tutorial to send messages to Discord, however, I realized that it took at least a few seconds to send messages as you can see in the video below. I will try using a software interrupt in my next update. 

[![Pinguino Blynk + Discord Webhook Test](https://adityakamath.github.io/assets/img/pinguino_discord_webhook_ss.png)](https://www.youtube.com/watch?v=cjWjZEVDjls "Pinguino Blynk + Discord Webhook Test - Click to Watch!")

#### Next Steps

With the above tasks, I managed to read messages from Blynk on the Arduino and then send messages to Discord. However, it is slightly difficult to send messages from Discord to Arduino - I will need to write a bot for that. I managed to start off with a [YouTube tutorial](https://www.youtube.com/watch?v=9CDPw1lCkJ8) and build a simple bot that replies 'Pong' to an input of 'Ping', but I couldn't go further this weekend. 

Other than this, I also need to fill in the functionalities for the other lampfaces, especially the Clock face which will need input from the realtime clock messages sent via Blynk. Again, something to do next time. 

Moreover, next week I expect the laser cut mounts to arrive and I plan on blogging about the simple Pinguino assembly process. 
