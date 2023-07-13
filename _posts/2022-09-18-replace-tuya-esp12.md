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

All the listed modules have a similar footprint and same pinout for pins labelled in the image. 

![ESP-12 Shape](/assets/images/replace-tuya-esp12/esp12footprint.webp)

For this you will need the following:

* soldering heat gun aka rework station - I use the JCD from [AliExpress*](https://s.click.aliexpress.com/e/_DneMSyr). You can try [without one](https://youtu.be/CVsmwFAkf7I?t=254) but it's harder than it looks in the video.
* a soldering iron - I recommend a modern pen style iron with TS or T12 tips such as Sequre Si012 ([Banggood](https://www.banggood.com/SEQURE-SI012-Intelligent-OLED-Electric-Soldering-Iron-Sensitivity-Adjustable-Built-In-Buzzer-Suitable-For-T12-p-1980737.html?p=CM27171011078201412U&custlinkid=3292815)\*, [AliExpress](https://www.aliexpress.com/item/1005004599576129.html?aff_fcid=fd558e32dbc64909a6ec9d005411095e-1689239662245-01569-_DdXZFwV&tt=CPS_NORMAL&aff_fsk=_DdXZFwV&aff_platform=shareComponent-detail&sk=_DdXZFwV&aff_trace_key=fd558e32dbc64909a6ec9d005411095e-1689239662245-01569-_DdXZFwV&terminal_id=6db88f7b3fff4670be83ec2d245af448&afSmartRedirect=y)\*, [Amazon](https://www.amazon.com/SEQURE-Soldering-Adjustable-Temperature-212%C2%B0F-842%C2%B0F/dp/B0BZ83D4D1?&linkCode=ll1&tag=blakadders-20&linkId=52fedd3ac4156a7ef0907b29e9175f4c&language=en_US&ref_=as_li_ss_tl)\*, [Sequre](https://sequremall.com/collections/soldering-irons/products/sequre-si012-pro-intelligent-oled-electric-soldering-iron-with-adjustable-sensitivity-and-built-in-buzzer-for-t12-ts-soldering-iron-tips-supports-pd3-0-qc2-0-dc5525-power-supply?ref=blakadder&variant=42003727614140)\*), PTS100 ([Banggood](https://www.banggood.com/PTS100-T12-PD-5-20V-65W-Portable-Electric-Soldering-Iron-CNC-Metal-Body-Temperature-Adjustable-Solder-Welding-Station-p-1981806.html?p=CM27171011078201412U&custlinkid=3292819)\*, [AliExpress](https://www.aliexpress.com/item/1005005743929099.html?aff_fcid=3f45fd61aae9498a899a2414a12eef09-1689238471353-00576-_DljjfGv&tt=CPS_NORMAL&aff_fsk=_DljjfGv&aff_platform=shareComponent-detail&sk=_DljjfGv&aff_trace_key=3f45fd61aae9498a899a2414a12eef09-1689238471353-00576-_DljjfGv&terminal_id=6db88f7b3fff4670be83ec2d245af448&afSmartRedirect=y)\*) or Pinecil ([Amazon](https://www.amazon.com/PINECIL-Smart-Mini-Portable-Soldering/dp/B096X6SG13?crid=UB2NV74HN24H&keywords=pinecil&qid=1689238561&sprefix=pinecil%2Caps%2C221&sr=8-1&linkCode=ll1&tag=blakadders-20&linkId=3497e8cb37c6ca323381f0009877ae47&language=en_US&ref_=as_li_ss_tl)\*)
* good solder and flux - don't get the super cheap stuff and definitely don't use unleaded solder. I typically get Mechanic brand from [AliExpress](https://s.click.aliexpress.com/e/_DDoZ8Ej)* and also their [RMA223 flux](https://s.click.aliexpress.com/e/_DFchJSr)*.
* desoldering braid - not a must have but helps in cleaning up old solder. Mechanic brand from [AliExpress](https://s.click.aliexpress.com/e/_DEdSR9d)* of course.
* serial to USB flasher - to flash the new ESP-12 module with firmware. I recommend using the ESP8266 test board ([AliExpress](https://s.click.aliexpress.com/e/_DBh3nPp)\*, [Banggood](https://www.banggood.com/ESP8266-Test-Board-Burner-Development-Board-WIFI-Module-For-ESP-01-ESP-01S-ESP-12E-ESP-12F-ESP-12S-ESP-18T-p-1684992.html?p=CM27171011078201412U&custlinkid=1640377)\* or [Amazon](https://amzn.to/3xVvrm7)*).
* IPA for cleaning
* an ESP-12 type module - pick between [ESP8266](https://templates.blakadder.com/ESP-12), [ESP32-C3](https://templates.blakadder.com/ESP-C3-12F.html) or [ESP32-S2](https://templates.blakadder.com/ESP-12H.html) versions

Work on a heat resistant surface. I use a cooking silicone mat [(AliExpress)](https://s.click.aliexpress.com/e/_DdGgqaj).

Disassemble the device and take the PCB out. 

[![Prepare the PCB](/assets/images/replace-tuya-esp12/1.jpg)](/assets/images/replace-tuya-esp12/1.jpg)

Put flux on the left and right row of pads and solder them liberally. The leaded solder you use will mix with the factory non-leaded solder and that will bring down the melting point and make the process faster and smoother.

[![Solder the pads liberally](/assets/images/replace-tuya-esp12/2.jpg)](/assets/images/replace-tuya-esp12/2.jpg)

Set the heat gun to 350-400°C and start moving it up and down alternating between left and right side of the module. Once you see the solder liquified, nudge the module up with tweezers or whatever. 

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
