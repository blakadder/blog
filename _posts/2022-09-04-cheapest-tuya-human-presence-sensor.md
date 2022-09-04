---
layout: post
title: "Cheapest Tuya Human Presence Sensor Review"
author: blak
categories: [ tasmota, home assistant, review ]
image: assets/images/header_tuya_presence.jpg
toc: true
---

You get what you pay for but sometimes not even that.

_Links in this article marked with a * are affiliate links. By purchasing through them I get a small commission that funds future projects!_

Since the release of Aqara FP1 and its immediate shortage online various mmWave human presence sensors have started appearing in [AliExpress](https://best.aliexpress.com/?lan=en&aff_fcid=9872e151f8664fad9910fe05d1ed73e4-1662142118003-01336-_DC4TnYr&tt=CPS_NORMAL&aff_fsk=_DC4TnYr&aff_platform=portals-tool&sk=_DC4TnYr&aff_trace_key=9872e151f8664fad9910fe05d1ed73e4-1662142118003-01336-_DC4TnYr&terminal_id=3f8c776975fd455ba956809c02d71a91)* listings, mostly of them working with Tuya Smart. I've decided to buy the cheapest one...

It arrived well packed in a larger box than I expected. The device is officially named (read: auto translated from Chinese) as "Intelligent Human Presence Sensor" and that's about it. No model number, no manufacturer, no discerning markings whatsoever.

[![Box](/assets/images/tuya_presence/box.jpg){: width="50%"}](/assets/images/tuya_presence/box.jpg)[![Box back](/assets/images/tuya_presence/box_back.jpg){: width="50%"}](/assets/images/tuya_presence/box_back.jpg)

Besides the presence sensor, the box contains a USB cable and a run of the mill manual sheet with instructions for the WiFi model on one side and Zigbee on the other.

The presence sensor case is very big, too big. As the measurements on the box state it's 100x100mm and 40 mm tall. The top is transparent in the middle to show the LED light.

[![Shell](/assets/images/tuya_presence/shell.jpg)](/assets/images/tuya_presence/shell.jpg)

The side with the USB plug has one more cutout and too many ventilation holes. This suggests the case is a mass produced generic shell, repurposed or modified from a gateway or some other environment sensor design.

[![Cutouts](/assets/images/tuya_presence/cutouts.jpg)](/assets/images/tuya_presence/cutouts.jpg)

The back has more ventilation cutouts, a mounting hole in the middle and the reset button used to put the device in pairing mode.

[![Back](/assets/images/tuya_presence/back.jpg)](/assets/images/tuya_presence/back.jpg)

## Smart Life App

Holding the reset button for 5 seconds puts the device in pairing mode indicated by the flashing LED.

It is added to the Smart Life app as "Human Presence Sensor WiFi Version".

[![Smart Life app](/assets/images/tuya_presence/app.jpg)](/assets/images/tuya_presence/app.jpg)

It also shows durations of presence and absence "creatively" named "Existence time" and "Departure time" but this timer caps out at 60 minutes so I'm not sure how useful it actually is.

Thankfully there's also an option to disable the LED because it's really eerie to have a red LED watching you like HAL-9000 while you're sitting in the living room.

[![HAL 9000](/assets/images/tuya_presence/hal9000.jpg)](/assets/images/tuya_presence/hal9000.jpg)

## How it works?

This presence sensor uses a 24Ghz millimeter wave radar or mmWave. Unlike traditional PIR motion sensors, it is supposed to detect presence even if there is not a lot of movement meaning it should work while you're sitting or even lying down.

It can be placed on a flat surface or mounted on a wall or ceiling. It covers a 90° radius with declared distance of 3 meters max. In my testing the detection range was up to 5 meters.

Detection is not instant and it takes from 1-4 seconds for the LED to change color when I walk into the room. Quite a disadvantage compared to PIR sensors which detect motion almost instantly.

While the mmWave radar did not give false positives when there was no one in the room it also failed to detect presence of TV watchers sitting still. The light occasionally turns green even with multiple people sitting in front of the TV.

![Presence History](/assets/images/tuya_presence/presence_history.jpg)

That first red line should be solid but it's full of short no presence events. 

## Integrations

Tuya Integration in Home Assistant added a new device automatically:

![Device Card](/assets/images/tuya_presence/ha_device_card.jpg)

No issues with Google or Alexa, they both discovered a new motion sensor from Smart Life.

## Teardown

4 screws later we're inside the case.

[![Empty case](/assets/images/tuya_presence/empty_case.jpg)](/assets/images/tuya_presence/empty_case.jpg)

That's a lot of empty real estate, both in the case and on the PCB. There are no model marking on the PCB either except the V1.0.1 marking.

[![PCB](/assets/images/tuya_presence/pcb.jpg)](/assets/images/tuya_presence/pcb.jpg)

Wi-Fi module is a standard [Tuya CB3S](https://developer.tuya.com/en/docs/iot/cb3s?id=Kai94mec0s076), based on Beken BK7231N.

This type of design allows them to just change the communication module and switch the device to ZigBee.

Being a Beken chip, means no Tasmota unless you replace the module with and [ESP-12](https://templates.blakadder.com/ESP-12) (which I will definitely do).

There is the option of flashing [OpenBeken](https://github.com/openshwprojects/OpenBK7231T_App) to it if you're willing to go down that rabbit hole.

In the middle is the very small [HLK-LD2410](https://github.com/ESPresense/ESPresense/files/9189632/HLK-LD2410.user.manual.V1.02.pdf) presence sensor from Hi-Link Electronic.

You can find the exact same one on [AliExpress](https://s.click.aliexpress.com/e/_DB0VZ7V)* for around 5$. This poses the question of why is this sensor almost 30$?

Above it is the onboard RGB LED serving as a presence indicator, changing from green to red when presence is detected.

Left side holds the power supply and the [STC8G1K17 MCU](http://www.stcmicro.com/stc/stc8g1k08.html) that controls the LED and interfaces between the LD2410 and CB3S. Note the terrible soldering work on the USB connector.

## Is it worth it?

First of all, it's too damn big for no reason. The mmWave radar used has sensitivity options than Tuya doesn't give access to.

The polling delay of 3 seconds between events is the final nail in the coffin.

You're better off paying more for a mmWave radar in a smaller case like [this one](https://s.click.aliexpress.com/e/_DDtdmKR)* with better range and detection radius that also has extra sensitivity settings and gives you an illumination sensor on top of all that.

If you want to go cheapest possible route build your own with an ESP [development board](https://templates.blakadder.com/diy#development_board) and the [HLK-LD2410](https://s.click.aliexpress.com/e/_DB0VZ7V)*. ESPHome supports it with [code from crlogic](https://community.home-assistant.io/t/mmwave-wars-one-sensor-module-to-rule-them-all/453260/2).
