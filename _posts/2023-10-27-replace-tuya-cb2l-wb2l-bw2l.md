---
layout: post
title: "Replace Tuya CB2L, WB2L, BW2L Modules with ESP"
author: blak
categories: [ diy, frankenstein, how to ]
tags: [ ]
image: assets/images/header_replace-tuya-cb2l-wb2l-bw2l.jpg
toc: true
---

Replace Tuya TYWE2L, CB2L, WB2L, WR2L, WBR2L, BW2L and similar Wi-Fi modules with an Espressif ESP32 or ESP8266 light module.

_**Full disclosure:** Links to Amazon, AliExpress and Banggood are affiliate links and I earn a small commission when you buy through them which helps fund future projects and reviews_

All the listed modules have a similar footprint and same pinout for pins labelled in the image.

![Module Shape](/assets/images/replace-tuya-cb2l-wb2l-bw2l/footprint.webp)

For this you will need the following:

{% include replacetools.html %}
* serial to USB flasher to flash the new module. I use [Soldered Connect Programmer](https://soldered.com/product/connect-programmer/) with a good voltage regulator but a cheap CH340G one from [AliExpress](https://www.aliexpress.com/item/1005005591052286.html?aff_fcid=e1ae94290a6142a3b6819ff94c64d530-1698592872921-05224-_DdFIRFp) or [Amazon](https://amzn.to/46OrOh7) or any other flasher board will be enough.
* ESP module:
  * [ESP8685-WROOM-05](https://templates.blakadder.com/ESP8685-WROOM-05.html) - ESP32-C3
  * [DT-ESP-C05](https://templates.blakadder.com/DT-ESP-C05.html) - ESP32-C3
  * [DMP-L1](https://templates.blakadder.com/DMP-L1.html) - ESP8266
  * [ESP8684-WROOM-05](https://templates.blakadder.com/ESP8684-WROOM-05.html) - ESP32-C2

Work on a heat resistant surface. I use a cheap cooking silicone mat [(AliExpress)](https://www.aliexpress.com/item/4000219480501.html?aff_fcid=8e01861330754a3dadbc5371915e3366-1698590891606-04210-_DdGgqaj).

Disassemble the device and take the PCB out if it makes it easier to work with. This example features the eWeLink E14 Candle bulb.

[![Prepare the PCB](/assets/images/replace-tuya-cb2l-wb2l-bw2l/1.jpg)](/assets/images/replace-tuya-cb2l-wb2l-bw2l/1.jpg)

Put flux on the bottom row of pads and solder them liberally. The leaded solder you use will mix with the factory non-leaded solder and that will bring down the melting point and make the process faster and smoother.

[![Heat the module](/assets/images/replace-tuya-cb2l-wb2l-bw2l/2.jpg)](/assets/images/replace-tuya-cb2l-wb2l-bw2l/2.jpg)

Set the heat gun to 350-400°C and start moving it left to right on the bottom of the module. Once you see the solder liquifying, nudge the module up with tweezers or whatever you have handy. 

[![Module removed](/assets/images/replace-tuya-cb2l-wb2l-bw2l/3.jpg)](/assets/images/replace-tuya-cb2l-wb2l-bw2l/3.jpg)

**DO NOT** apply a lot of pressure and force it, that will only result in torn pads. If everything has heated up to the right temperature the module will slide off.

[![Clean up old solder](/assets/images/replace-tuya-cb2l-wb2l-bw2l/4.jpg)](/assets/images/replace-tuya-cb2l-wb2l-bw2l/4.jpg)

Clean up left over solder with the desoldering braid.

[![Clean up flux](/assets/images/replace-tuya-cb2l-wb2l-bw2l/5.jpg)](/assets/images/replace-tuya-cb2l-wb2l-bw2l/5.jpg)

Clean the surface with IPA or other alcohol.

[![Pads need to be clean](/assets/images/replace-tuya-cb2l-wb2l-bw2l/6.jpg)](/assets/images/replace-tuya-cb2l-wb2l-bw2l/6.jpg)

Now is the time to flash the replacement module with firmware you'll be using.

[![Module comparison](/assets/images/replace-tuya-cb2l-wb2l-bw2l/7.jpg)](/assets/images/replace-tuya-cb2l-wb2l-bw2l/7.jpg)

Put blobs of solder on the pads on the back of the module.

[![Put blobs of solder on the pads on the back of the module](/assets/images/replace-tuya-cb2l-wb2l-bw2l/8.jpg)](/assets/images/replace-tuya-cb2l-wb2l-bw2l/8.jpg)

Solder wires that will connect to your USB adapter. You need to solder 3V3, GND, TX, RX, IO9 is optional since it only needs to be grounded during power up. I still solder the wire and keep it connected to ground during the flashing process.

[![Solder wires that will connect to your USB adapter](/assets/images/replace-tuya-cb2l-wb2l-bw2l/9.jpg)](/assets/images/replace-tuya-cb2l-wb2l-bw2l/9.jpg)

Flash the module with your firmware and make sure it's working correctly. Don't forget to unground IO9 to boot the module in normal working mode.

[![Flashing in progress](/assets/images/replace-tuya-cb2l-wb2l-bw2l/10.jpg)](/assets/images/replace-tuya-cb2l-wb2l-bw2l/10.jpg)

Clean up the solder from the back pads with the solder wick. This is required if the module sits flush with the PCB in the device you're replacing it. 

[![Clean pads on the module](/assets/images/replace-tuya-cb2l-wb2l-bw2l/11.jpg)](/assets/images/replace-tuya-cb2l-wb2l-bw2l/11.jpg)

Align the module with the pads and secure it with tape so it doesn't move while soldering.

[![Line up the pads](/assets/images/replace-tuya-cb2l-wb2l-bw2l/12.jpg)](/assets/images/replace-tuya-cb2l-wb2l-bw2l/12.jpg)

Fill each pad with solder. Make sure your soldering iron is hot enough and use plenty of flux to help the solder flow. You don't need to make it look pretty, you just need good connections between the module and the PCB pads.

[![Solder new module](/assets/images/replace-tuya-cb2l-wb2l-bw2l/13.jpg)](/assets/images/replace-tuya-cb2l-wb2l-bw2l/13.jpg)

Clean up your work with more IPA and its ready to be reassembled.

[![Job finished](/assets/images/replace-tuya-cb2l-wb2l-bw2l/14.jpg)](/assets/images/replace-tuya-cb2l-wb2l-bw2l/14.jpg)

Congratulate yourself on a job well done!