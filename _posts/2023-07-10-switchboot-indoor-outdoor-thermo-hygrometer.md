---
layout: post
title: "SwitchBot SwitchBot Indoor/Outdoor Thermo-Hygrometer Review"
author: blak
categories: [ review, bluetooth, home assistant, mqtt  ]
tags: [ ]
image: assets/images/header_switchbot_outdoor.jpg
toc: true
---

SwitchBot brings us a real rarity amongst sensors. A true outdoor sensor that is built to be outside and withstand the elements. 

_**Full disclosure:** This is a review sample sent to me free of charge by [SwitchBot](https://shareasale.com/r.cfm?b=1689064&u=2442238&m=104860&urllink=&afftrack=). Review is not influenced by that fact and is solely my opinion. Shopping links in this article are affiliate links and I earn a small commission when you buy through them which helps support my work_

SwitchBot SwitchBot Indoor/Outdoor Thermo-Hygrometer is available from:
- [SwithcBot Store](https://shareasale.com/r.cfm?b=1689064&u=2442238&m=104860&urllink=eu%2Eswitch%2Dbot%2Ecom%2Fproducts%2Fswitchbot%2Dindoor%2Doutdoor%2Dthermo%2Dhygrometer&afftrack=)
- [Amazon US](https://www.amazon.com/SwitchBot-Hygrometer-Thermometer-Bluetooth-Refrigerator/dp/B0BVZC9Q31?&linkCode=ll1&tag=blakadders-20&linkId=136ef4f52799773871ec1b218aa2580f&language=en_US&ref_=as_li_ss_tl)
- [Amazon CA](https://www.amazon.ca/dp/B0BVZC9Q31?&linkCode=ll1&tag=tasmotatemp03-20&linkId=87600fd500cc6a77ef876d0a318413f9&language=en_CA&ref_=as_li_ss_tl)
- [Amazon EU](https://www.amazon.de/dp/B0BVZC9Q31?&linkCode=ll1&tag=blakadders-20&linkId=ba068b630fc5655c8ea6e0bade8a52cf&language=en_GB&ref_=as_li_ss_tl)
- [Amazon UK](https://www.amazon.co.uk/dp/B0BVZC9Q31?&linkCode=ll1&tag=blakadders-20&linkId=1feb3fe1af8748399d63c08d7aee95da&language=en_GB&ref_=as_li_ss_tl)

SwitchBot will have 25% of every product on Amazon Prime day. Then an additional 5% off every product with the coupon code `XMSWITCHBOT`. That's a total discount of 30%!

You can also buy directly from [SwitchBot online store](https://shareasale.com/r.cfm?b=1689064&u=2442238&m=104860&urllink=eu%2Eswitch%2Dbot%2Ecom%2Fpages%2Fswitchbot%2Ddeals&afftrack=) and use code `30PRIME` for 30% discount and there are even some additional perks depending on the order during the Prime Day Sale. 

A very good deal is the Indoor/Outdoor sensor + SwitchBot Hub 2 [combo pack](https://shareasale.com/r.cfm?b=1689064&u=2442238&m=104860&urllink=eu%2Eswitch%2Dbot%2Ecom%2Fproducts%2Findoor%2Doutdoor%2Dthermo%2Dhygrometer%2Dcombo&afftrack=).

## Overview

I've wasn't a fan of SwitchBot in their early SwitchBot Bot days but they're turning me around with their new line of devices and the Indoor/Outdoor Thermo-Hygrometer (a fancy word for temperature and humidity sensor) has a lot of what I've been missing.

[![The box](/assets/images/switchbot_outdoor/box.jpg)](/assets/images/switchbot_outdoor/box.jpg)

First and foremost it is built for being placed outdoors. It is a splash proof, IP65 rated device that you can leave outside without fear of it corroding and breaking down. Its plastic shell is UV resistant and will withstand exposure to the sun without adverse effects.

[![What's inside the box](/assets/images/switchbot_outdoor/package.jpg)](/assets/images/switchbot_outdoor/package.jpg)

It can be placed everywhere with the provided lanyard. No more double sided tape or screws, just hang it where you need it.

Unfortunately (or fortunately?) it is Bluetooth. You can read the values using your smartphone and the SwitchBot app but for real automations you require one of SwitchBot's hubs. That's not that bad because [SwitchBot Hub 2](https://shareasale.com/r.cfm?b=1689064&u=2442238&m=104860&urllink=eu%2Eswitch%2Dbot%2Ecom%2Fpages%2Fswitchbot%2Dhub%2D2&afftrack=) is a 4 in 1 device (hub, remote button, smart remote and thermo-hygrometer) that supports Homekit and Matter which allows you to add these sensors to your smart home using a Matter hub with a completely local protocol.

But there are also other ways, SwitchBot is very open to third party implementations and we'll go over some of the options later in the article.

The sensor uses good ole' AA batteries (already included in the package) that provide a long battery life and are easy to replace. It also looks good and blends in nicely.

[![Batteries already included](/assets/images/switchbot_outdoor/batteries.jpg)](/assets/images/switchbot_outdoor/batteries.jpg)

## App

The sensor is quickly found with the SwitchBot app which is surprisingly nice to use and has an appealing layout. There is a plethora of data collected and besides the temperature and humidity, you also have: absolute humidity, dew point and vapor pressure deficit data.

[![App screen 1](/assets/images/switchbot_outdoor/app1.jpg){: width="33%"}](/assets/images/switchbot_outdoor/app1.jpg)[![App screen 2](/assets/images/switchbot_outdoor/app2.jpg){: width="33%"}](/assets/images/switchbot_outdoor/app2.jpg)[![App screen 3](/assets/images/switchbot_outdoor/app3.jpg){: width="33%"}](/assets/images/switchbot_outdoor/app3.jpg)

![Data update](/assets/images/switchbot_outdoor/data_update.jpg)
<figcaption class="figure-caption text-center">Sensor readings are made every 4 seconds so you won't miss any changes.</figcaption>

In the settings menu there's an option to calibrate the sensor further ensuring its accuracy and alert settings.

[![App screen 4](/assets/images/switchbot_outdoor/app4.jpg){: width="33%"}](/assets/images/switchbot_outdoor/app4.jpg)[![App screen 5](/assets/images/switchbot_outdoor/app5.jpg){: width="33%"}](/assets/images/switchbot_outdoor/app5.jpg)[![App screen 6](/assets/images/switchbot_outdoor/app6.jpg){: width="33%"}](/assets/images/switchbot_outdoor/app6.jpg)

## Teardown

When you open the back cover you can see the gasket around its lip that seals the device to make it dust and water proof. After taking out the PCB you can see that they sealed the hole for the temperature as well. 

[![Gasket](/assets/images/switchbot_outdoor/gasket.jpg){: width="50%"}](/assets/images/switchbot_outdoor/gasket.jpg)[![SensorGasket](/assets/images/switchbot_outdoor/sensor_gasket.jpg){: width="50%"}](/assets/images/switchbot_outdoor/sensor_gasket.jpg)

After I took out the PCB I also noticed that the hole for the temperature sensor is likewise sealed. With all that there is no problem leaving it anywhere, potentially you can bring it in the shower if you want or need to.  

The temperature and humidity sensor of choice is the [Sensirion SHT40](https://sensirion.com/products/catalog/SHT40). This sensor an operating range of −40-125 °C and 0-100 %RH. On top of that it boasts a ±1.5% humidity and ±0.1°C temperature sensor accuracy delta which is excellent for a domestic sensor.

[![PCB](/assets/images/switchbot_outdoor/pcb.jpg){: width="50%"}](/assets/images/switchbot_outdoor/pcb.jpg)[![Sensor](/assets/images/switchbot_outdoor/sensor.jpg){: width="50%"}](/assets/images/switchbot_outdoor/sensor.jpg)

## Integration

It is well known I'm not that keen on using vendor apps or their hubs but there are open source ways to integrate the SwitchBot Indoor/Outdoor sensor. The benefit of Bluetooth is that it only broadcasts data, therefore you can even use the SwitchBot Hub while using one or all of the integrations.

### OpenMQTTGateway

[OpenMQTTGateway](https://docs.openmqttgateway.com/) turns an ESP32 (or ESP8266) device into a multiprotocol to MQTT bridge, BLE being one of them. It's a mature project, developed for many years and works on [many platforms](https://docs.openmqttgateway.com/prerequisites/board.html) and, more importantly, supports a [huge number of devices](https://compatible.openmqttgateway.com/). Being MQTT based it integrates easily with other smart home solutions such as [openHAB](https://docs.openmqttgateway.com/integrate/openhab3.html) or [Home Assistant](https://docs.openmqttgateway.com/integrate/home_assistant.html).

For a quick and easy Bluetooth2MQTT gateway install it to one of the supported ESP32 boards using their web installer and configure it using the web UI.

After configuring Wi-Fi and MQTT, many devices started popping up in my Home Assistant MQTT integration and amongst them was the SwitchBot Indoor/Outdoor.

[![OpenMQTTGateway in Home Assistant](/assets/images/switchbot_outdoor/omg_device_card.jpg)](/assets/images/switchbot_outdoor/omg_device_card.jpg)

This was one of the first solutions that supported the Indoor/Outdoor sensor and it is what I used during testing for the past 45 days.

BLE to MQTT gateway can also run on Raspberry Pi, Windows or Unix using [Theengs Gateway](https://gateway.theengs.io/).

### Home Assistant

Home Assistant 2023.7 finally brought support for this sensor in the [SwitchBot integration](https://www.home-assistant.io/integrations/switchbot/). You can use a [Bluetooth dongle](https://www.home-assistant.io/integrations/bluetooth/#installing-a-usb-bluetooth-adapter) plugged into your Home Assistant server or via a remote [ESPHome Bluetooth proxy](https://esphome.github.io/bluetooth-proxies/). I used my trusty [GL-S10](gl-s10) as a proxy device due to its superior range.

[![SwitchBot integration in Home Assistant](/assets/images/switchbot_outdoor/bt_device_card.jpg)](/assets/images/switchbot_outdoor/bt_device_card.jpg)

## Performance

As soon as I got my pair of sensors I stuck one outside on the front doors. Its a spot slightly protected from rain but the sun is hitting it for most of the day. The sensor had the opportunity to test high humidity and rain since we had a week and a half straight with daily rain and even rainstorms.

[![Hanging on front door](/assets/images/switchbot_outdoor/hanging_door.jpg)](/assets/images/switchbot_outdoor/hanging_door.jpg)
<figcaption class="figure-caption text-center">Disclaimer: I wasn't responsible for the extra decoration.</figcaption>

The image above is taken on the day of writing this review, 45ish days after. As you can see it still looks crisp white, it functions the same as the first day and the battery did not budge from 100%.

The other sensor had some fun in the freezer at -18°C for a day. It survived the outing and read the freezer temps just fine apart from the signal being spotty because the fridge is a metal box and the OpenMQTTGateway was 8m away. Battery still at 100% and temperature readings are fine.

[![In freezer](/assets/images/switchbot_outdoor/in_freezer.jpg)](/assets/images/switchbot_outdoor/in_freezer.jpg)

All the changes are read instantly and there's a report every 5 seconds when there are rapid changes (like putting it in a freezer)

When put in my sensor testing setup, affectionately called "Sensmörgåsbord" it kept in line with other known sensors, no outliers or drops at all but with 0,5°C higher readings.

[![Temperature Testing](/assets/images/switchbot_outdoor/temptest.jpg)](/assets/images/switchbot_outdoor/temptest.jpg)

The SHT4x line of sensors has already been performing exceptionally in my testing and are my choice for DIY projects.

## Conclusion

[![Cherries](/assets/images/switchbot_outdoor/cherries.jpg)](/assets/images/switchbot_outdoor/cherries.jpg)
<figcaption class="figure-caption text-center">Hanging the sensor in random trees for those Insta shots!</figcaption>

A very versatile temperature and humidity sensor that can go in many places other sensors shouldn't. Using standard batteries is always a big plus and due to how easy it is to integrate Bluetooth devices it being Bluetooth only doesn't matter as much.

The standard price tag of 14.99$ (19.99€ in EU! VAT hurts!) for one looks a bit high (or very high for EU) at first glance but its still a fair price for the quality, accuracy and versatility of the sensor. There aren't many sensors that you can safely stick in a freezer, shower or outside in the sun long term.

Cost per sensor gets cheaper if you buy them in packs and with the upcoming Prime Day sales the price drops down significantly. If you need sensors for those special places this is the ticket.

SwitchBot SwitchBot Indoor/Outdoor Thermo-Hygrometer is available from:
- [SwithcBot Store](https://shareasale.com/r.cfm?b=1689064&u=2442238&m=104860&urllink=eu%2Eswitch%2Dbot%2Ecom%2Fproducts%2Fswitchbot%2Dindoor%2Doutdoor%2Dthermo%2Dhygrometer&afftrack=)
- [Amazon US](https://www.amazon.com/SwitchBot-Hygrometer-Thermometer-Bluetooth-Refrigerator/dp/B0BVZC9Q31?&linkCode=ll1&tag=blakadders-20&linkId=136ef4f52799773871ec1b218aa2580f&language=en_US&ref_=as_li_ss_tl)
- [Amazon CA](https://www.amazon.ca/dp/B0BVZC9Q31?&linkCode=ll1&tag=tasmotatemp03-20&linkId=87600fd500cc6a77ef876d0a318413f9&language=en_CA&ref_=as_li_ss_tl)
- [Amazon EU](https://www.amazon.de/dp/B0BVZC9Q31?&linkCode=ll1&tag=blakadders-20&linkId=ba068b630fc5655c8ea6e0bade8a52cf&language=en_GB&ref_=as_li_ss_tl)
- [Amazon UK](https://www.amazon.co.uk/dp/B0BVZC9Q31?&linkCode=ll1&tag=blakadders-20&linkId=1feb3fe1af8748399d63c08d7aee95da&language=en_GB&ref_=as_li_ss_tl)

SwitchBot will have 25% of every product on Amazon Prime day. Then an additional 5% off every product with the coupon code `XMSWITCHBOT`. That's a total discount of 30%!

You can also buy directly from [SwitchBot online store](https://shareasale.com/r.cfm?b=1689064&u=2442238&m=104860&urllink=eu%2Eswitch%2Dbot%2Ecom%2Fpages%2Fswitchbot%2Ddeals&afftrack=) and use code `30PRIME` for 30% discount and there are even some additional perks depending on the order during the Prime Day Sale. 

A very good deal is the Indoor/Outdoor sensor + SwitchBot Hub 2 [combo pack](https://shareasale.com/r.cfm?b=1689064&u=2442238&m=104860&urllink=eu%2Eswitch%2Dbot%2Ecom%2Fproducts%2Findoor%2Doutdoor%2Dthermo%2Dhygrometer%2Dcombo&afftrack=).

I just wish it has a better name or model number, writing Indoor/Outdoor sensor throughout this review was weird!

[![Mulberry](/assets/images/switchbot_outdoor/dud.jpg)](/assets/images/switchbot_outdoor/dud.jpg)
<figcaption class="figure-caption text-center">And now.... a mulberry!</figcaption>
