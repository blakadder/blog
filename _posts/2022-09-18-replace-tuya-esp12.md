---
layout: post
title: "Replace Tuya Module with ESP-12"
author: blak
categories: [ diy, frankenstein, how to ]
tags: [ ]
image: assets/images/header_replace-tuya-esp12.jpg
toc: true
---

Replace Tuya (TYWE3S), Beken (CB3S, CB3L, WB3L, WB3S), Belon Solutions (FL_M93_V1), BouffaloLab (BL-62B), Realtek (WR3) and similar Wi-Fi modules with an Espressif ESP-12.

_**Full disclosure:** Links to Amazon, AliExpress and Banggood are affiliate links and I earn a small commission when you buy through them which helps fund future projects and reviews_

All the listed modules have a similar footprint and same pinout for pins labelled in the image. 

![ESP-12 Shape](/assets/images/replace-tuya-esp12/esp12footprint.webp)

For this you will need the following:

{% include replacetools.html %}
* serial to USB flasher - to flash the new ESP-12 module with firmware. I recommend using the ESP8266 test board ([AliExpress](https://s.click.aliexpress.com/e/_DBh3nPp)\*, [Banggood](https://www.banggood.com/ESP8266-Test-Board-Burner-Development-Board-WIFI-Module-For-ESP-01-ESP-01S-ESP-12E-ESP-12F-ESP-12S-ESP-18T-p-1684992.html?p=CM27171011078201412U&custlinkid=1640377)\* or [Amazon](https://amzn.to/3xVvrm7)*).
* an ESP-12 type module - pick between [ESP8266](https://templates.blakadder.com/ESP-12), [ESP32-C3](https://templates.blakadder.com/ESP-C3-12F.html) or [ESP32-S2](https://templates.blakadder.com/ESP-12H.html) versions
* ESP module:
  * [ESP8685-WROOM-01](https://templates.blakadder.com/ESP8685-WROOM-01.html) - ESP32-C3
  * [ESP8685-WROOM-04](https://templates.blakadder.com/ESP8685-WROOM-04.html) - ESP32-C3
  * [ESP-C3-12F](https://templates.blakadder.com/ESP-C3-12F.html) - ESP32-C3
  * [ESP-12](https://templates.blakadder.com/ESP-12) - ESP8266
  * [ESP8684-WROOM-05](https://templates.blakadder.com/ESP8684-WROOM-05.html) - ESP32-C2
  * [ESPC2-12](https://templates.blakadder.com/ESPC2-12.html) - ESP32-C2
  * [ESP8684-WROOM-01C](https://templates.blakadder.com/ESP8684-WROOM-01C.html) - ESP32-C3
  * [ESP-12H](https://templates.blakadder.com/ESP-12H.html) - ESP32-S2

Work on a heat resistant surface. I use a cooking silicone mat [(AliExpress)](https://s.click.aliexpress.com/e/_DdGgqaj).

Disassemble the device and take the PCB out. 

[![Prepare the PCB](/assets/images/replace-tuya-esp12/1.jpg)](/assets/images/replace-tuya-esp12/1.jpg)

Put flux on the left and right row of pads and solder them liberally. The leaded solder you use will mix with the factory non-leaded solder and that will bring down the melting point and make the process faster and smoother.

[![Solder the pads liberally](/assets/images/replace-tuya-esp12/2.jpg)](/assets/images/replace-tuya-esp12/2.jpg)

Set the heat gun to 350-400°C and start moving it up and down alternating between left and right side of the module. Once you see the solder liquifying, nudge the module up with tweezers or whatever. 

[![Heat the module](/assets/images/replace-tuya-esp12/3.jpg)](/assets/images/replace-tuya-esp12/3.jpg)

**DO NOT** apply a lot of pressure and force it, that will only result in torn pads. If everything has heated up to the right temperature the module will slide off.

[![Module removed](/assets/images/replace-tuya-esp12/4.jpg)](/assets/images/replace-tuya-esp12/4.jpg)

Clean up left over solder with the desoldering braid.

[![Clean up old solder](/assets/images/replace-tuya-esp12/5.jpg)](/assets/images/replace-tuya-esp12/5.jpg)

Clean the surface with IPA or other alcohol.

[![Clean up flux](/assets/images/replace-tuya-esp12/6.jpg)](/assets/images/replace-tuya-esp12/6.jpg)

Now is the time to flash the replacement module with firmware you'll be using.

[![Flashing in progress](/assets/images/replace-tuya-esp12/7.jpg)](/assets/images/replace-tuya-esp12/7.jpg)

Align the module with the pads and secure it with a small clamp or clothespin.

[![Line up the pads](/assets/images/replace-tuya-esp12/8.jpg)](/assets/images/replace-tuya-esp12/8.jpg)

Fill each pad with solder. Make sure your soldering iron is hot enough and use flux to help the solder flow. You don't need to make it look pretty, you just need good connections between the module and the PCB pads.

[![Solder new module](/assets/images/replace-tuya-esp12/9.jpg)](/assets/images/replace-tuya-esp12/9.jpg)

Clean up your work with more IPA, put it all back together. 

Congratulate yourself on a job well done!
