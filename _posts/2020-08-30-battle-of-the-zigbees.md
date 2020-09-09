---
layout: post
title: "Battle of the Zigbees"
author: blak
categories: [ zigbee, review ]
tags: [ ]
image: assets/images/header_battle.jpg
toc: true
---

Ultimate showdown of cheap Zigbee sensors. In one corner is the veteran Xiaomi Aqara and in the other, a relative newcomer, Itead Sonoff. Two shall enter...

## Introduction
Sonoff entered the Zigbee space silently with their [Basic ZBR3](https://zigbee.blakadder.com/Sonoff_BASICZBR3.html) and [S31 Lite](https://zigbee.blakadder.com/Sonoff_S31ZB.html) but the release of the Zigbee Bridge and the accompanying Zigbee 3.0 sensors created a huge buzz in market and a lot of people became interested in the protocol. 

Are those sensors competitive enough to dethrone the current champions from Xiaomi and Aqara?

## Round 1: Temperature and Humidity

Pitting Sonoff Temperature and Humidity sensor ([SNZB-02](https://zigbee.blakadder.com/Sonoff_SNZB-02.html)) against [Aqara](https://zigbee.blakadder.com/Xiaomi_WSDCGQ11LM.html) and [Mijia](https://zigbee.blakadder.com/Xiaomi_WSDCGQ01LM.html) versions.

![](/assets/images/battle/th1.jpg)

First apparent difference is the size, Sonoff sensor is much larger and quite thicker. The other is the design, Sonoff follows a certain aesthetic with two chopped corners and prominent edges. Shell is colored bright white and the plastic used has a cheap feeling and, when tapped, sounds cheap too. 

To access the battery you have to use force to pop the bottom cover off which is held in place by 4 clips. I'm not sure putting in new batteries after its mounted somewhere will be painless. Although, since Sonoff uses a big CR2540 battery that might not be necessary for a long while.

Aqara sensor has a simple square with rounded edges design. Case is from a milky white colored plastic with appropriate gray accents and it feels much nicer than Sonoff's. Battery is accessed simply by twisting the back plate which makes exchanging batteries when its stickered to the wall a lot simpler. It also has a pressure sensor which Sonoff doesn't.

In the image next to those two is the Mijia sensor. It's even smaller but its an old model which is hard to find for a reasonable price.

Price wise Sonoff's price is 8.49$ while Aqara can be found for 11-13$.

Aqara handily wins on build quality, design, ease of use and extra features. Sonoff's advantage is price, Zigbee 3.0 standard and, arguably, a bigger battery. Humidity and temperature readings were consistently close on both so I would assume they're accurate. 

## Round 2: Door or Window Sensor

Sonoff Wireless Door/Window Sensor ([SNZB-04](https://zigbee.blakadder.com/Sonoff_SNZB-04.html)) vs Aqara [MCCGQ11LM](https://zigbee.blakadder.com/Xiaomi_MCCGQ11LM.html).

![](/assets/images/battle/dw1.jpg)

Again, the size and design difference is repeated as well as the build quality. Battery access is easier on the Sonoff because the battery lid is held by side clips and inserting a flat screwdriver in the top and bottom slot will pop the lid off with ease. Sonoff has a bigger CR2032 battery than Aqara's CR1632.

Feature wise, they're the same. Open or close messages and that's it. They both just work. Small magnet is noticeably weaker than Aqara's but that doesn't seem to matter.

Sonoff Door sensor price is 8.49$ while Aqara can be found for under 10$.

Unless you need Zigbee 3.0 or the price is king I'd go for the Aqara in this case. Smaller is better when located in a visible spot.

## Round 3: Wireless Switch

Sonoff Wireless Switch ([SNZB-01](https://zigbee.blakadder.com/Sonoff_SNZB-04.html)) vs Xiaomi Mijia [WXKG01LM](https://zigbee.blakadder.com/Xiaomi_WXKG01LM.html).

While size isn't a big differentiator here, the shape makes all the difference. To activate the Sonoff switch you need to press it in the middle, because of the design pressing the corners won't do anything. Pair that with the chintzy plastic and you get a bad wireless button.

![](/assets/images/battle/wb1.jpg)

It is also poor with features: only single, double and long press events in comparison to Xiaomi button's: single, double, triple, quad, many, long press hold and long press release.

Xiaomi's round shape is very functional, it will always respond to a press and the good tactile and audio feedback makes using all those multipresses very easy.

Price of the Sonoff button is 7.99$ and Xiaomi Mijia can be found for under 8$ and regularly for 12$.

Xiaomi's wireless button, while being on of the first in the market is still the best wireless button in the market and a best buy to boot. There's really no reason to go for Sonoff apart from curiosity. 

## Round 3: Motion Sensor

Sonoff Motion Sensor ([SNZB-03](https://zigbee.blakadder.com/Sonoff_SNZB-03.html)) vs Aqara [RTCGQ11LM ](https://zigbee.blakadder.com/Xiaomi_RTCGQ11LM.html).

![](/assets/images/battle/ms1.jpg)

A size comparison is not appropriate here because they went in very different directions. Personally I prefer Aqara's cylindrical design with an articulating holder but I have nothing against the "eye in a box" style from Sonoff.

The plastic is still the cheap, chintzy one compared to the nice, milky white and gray case from Aqara.

While Aqara does have an illumination sensor, the readings are sent only when a motion is detected so it is not as useful as it could be. It is still good for light check once motion is detected. 

Motion sensing range is similar and both were constantly triggered at 6m range. Sonoff has a 110° field of vision  and would perform better when mounted on the ceiling. Aqara is better suited for wall mounting and the articulating holder makes it easy to adjust the motion sensing area which is not easily doable on Sonoff.

Both sensors have a motion sensing delay once triggered. On Sonoff it is 60 seconds and on Aqara it varies between 60s and <s>180</s> 120 seconds (edit: This depends on the gateway used, the sensor itself has a 60 second delay hardcoded).

Price of Sonoff Motion Sensor is 9.49$ and Aqara is 11.99$-12.99$ with the mount.

I'm really surprised with Sonoff's sensor, specially after a few rounds of utter disappointments. <s>The 60 second delay make it work much nicer.</s> While the mounting options are different and illumination is a nice bonus, Sonoff still gets a win here but it's a close one and will, again, depend on your needs.

## Conclusion

While not perfect, Sonoff still offers a competitive range of sensors with a significant step up to Zigbee 3.0 which should improve compatibility (if they followed the spec). 

Xiaomi devices have a bad rep of falling off the Zigbee network depending on the router devices included. My network runs on [zzh!](https://zigbee.blakadder.com/Electrolama_zzh.html) and [Zigbee2MQTT](https://www.zigbee2mqtt.io/) with a mix of manufacturers and a few [DIY routers](https://zigbee.blakadder.com/ptvo.router.html) and I didn't experience such issues.

Aqara also announced their T1 line of Zigbee 3.0 sensors which should come out later this year. Maybe you should just wait for those and ignore this comparison ;)

P.S. [Never forget](https://youtu.be/1DTYjp4m9EM?t=378):

<div style='position:relative;padding-bottom:56.250%;'><iframe src="//gifs.com/embed/ooh-the-zigbee-QnzE4l" frameborder="0" scrolling="no" width="640" height="360" style="backface-visibility: hidden; transform: scale(1); position: absolute; height: 100%; width: 100%;"></iframe></div>
