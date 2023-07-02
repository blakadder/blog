---
layout: post
title: "Seeed Studio Grove All-in-one Environmental Sensor with Tasmota"
author: blak
categories: [ diy, how to, tasmota  ]
tags: [ ]
image: assets/images/header_sen55.jpg
toc: true
---

The Grove all-in-one environmental sensors from Seeed Studio (SEN54 and SEN55) detect various pollution indicators and can be used to monitor air quality in enclosed spaces. They're built to easily implement into your DIY sensor project and I'll show you how to use it with Tasmota.

_**Full disclosure:** This is a review sample sent to me free of charge by [Seeed Studio](https://www.seeedstudio.com/?sensecap_affiliate=jo7uUTK&referring_service=blog). Review is not influenced by that fact and is solely my opinion. Shopping links in this article are affiliate links and I earn a small commission when you buy through them which helps support my work_

Seeed Studio Grove All-in-one Environmental Sensor (SEN5X) is available from [Seeed Studio](https://www.seeedstudio.com/Grove-All-in-one-Environmental-Sensor-SEN55-p-5373.html?sensecap_affiliate=jo7uUTK&referring_service=blog) online store.

## Overview

The SEN55 sensor measures temperature, humidity, PM1.0, PM2.5, PM4, PM10, VOC (volatile organic compounds) and nitrogen oxides (NOx). [SEN54](https://www.seeedstudio.com/Grove-All-in-one-Environmental-Sensor-SEN54-p-5374.html?sensecap_affiliate=jo7uUTK&referring_service=blog), which is is 5$ cheaper , doesn't detect nitrogen oxides.

[![The case](/assets/images/sen55/case1.jpg){: width="50%"}](/assets/images/sen55/case1.jpg)[![The case](/assets/images/sen55/case2.jpg){: width="50%"}](/assets/images/sen55/case2.jpg)
[![The case](/assets/images/sen55/case3.jpg){: width="50%"}](/assets/images/sen55/case3.jpg)[![The case](/assets/images/sen55/case4.jpg){: width="50%"}](/assets/images/sen55/case4.jpg)

Seeed Studio's Grove sensor is based on [Sensirion's SEN55](https://www.sensirion.com/products/catalog/SEN55) ([Datasheet](https://www.sensirion.com/media/documents/6791EFA0/62A1F68F/Sensirion_Datasheet_Environmental_Node_SEN5x.pdf)) which is placed into an acrylic enclosure with a [Grove](https://wiki.seeedstudio.com/Grove_System/) port. Grove is an easy to use interface to existing development platforms from [Seeed Studio](https://www.seeedstudio.com/category/Grove-c-1003.html?sensecap_affiliate=jo7uUTK&referring_service=blog) such as [XIAO](https://www.seeedstudio.com/xiao-series-page?sensecap_affiliate=jo7uUTK&referring_service=blog), [Raspberry Pi](https://www.seeedstudio.com/Grove-Base-Hat-for-Raspberry-Pi.html?sensecap_affiliate=jo7uUTK&referring_service=blog), [Arduino](https://www.seeedstudio.com/https://www.seeedstudio.com/Seeeduino-V4-2-p-2517.html?sensecap_affiliate=jo7uUTK&referring_service=blog), [Wio Terminal](https://www.seeedstudio.com/Wio-Terminal-p-4509.html?sensecap_affiliate=jo7uUTK&referring_service=blog) and others. Seeed Studio has some of those applications for SEN54 or SEN55 well documented in their [wiki](https://wiki.seeedstudio.com/Grove_SEN5X_All_in_One/).

The Grove to Grove cable is provided with the sensor. If you wish to conect it to a typical development board using the standard 2.54 pin headers you will need a [Grove Conversion cable](https://www.seeedstudio.com/Grove-4-pin-Female-Jumper-to-Grove-4-pin-Conversion-Cable-5-PCs-per-PAck.html?sensecap_affiliate=jo7uUTK&referring_service=blog). 

[![Grove Cable](/assets/images/sen55/grovecable.jpg){: width="50%"}](/assets/images/sen55/grovecable.jpg)[![Grove Cable](/assets/images/sen55/grovecable2.jpg){: width="50%"}](/assets/images/sen55/grovecable2.jpg)

Let's take a peek inside by unscrewing the 4 screws holding the acrylic case together.

[![Inside](/assets/images/sen55/inside1.jpg){: width="33%"}](/assets/images/sen55/inside1.jpg)[![Inside](/assets/images/sen55/inside2.jpg){: width="33%"}](/assets/images/sen55/inside2.jpg)[![Inside](/assets/images/sen55/inside3.jpg){: width="33%"}](/assets/images/sen55/inside3.jpg)

The modification done by Seeed Studio is the Grove port and a voltage regulator to allow a 3.3V power supply option. If you wish you can use the sensor itself in your project and connect to it via the original cable. The pinout for it is available in the [datasheet](https://www.sensirion.com/media/documents/6791EFA0/62A1F68F/Sensirion_Datasheet_Environmental_Node_SEN5x.pdf). 

[![Inside](/assets/images/sen55/inside4.jpg){: width="50%"}](/assets/images/sen55/inside4.jpg)

### VOC and NOx Index

SEN55 has a different approach towards volatile organic compound sensing. Instead of using the common TVOC representation of the data which is hard to quantify and ofter isn't exact, Sensirion implemented an index. Both indexes mimic the human nose's perception of odors with a relative intensity compared to recent history. It adapts its gain according to the events of the past 24 hours learning period, leading to different conditions being quantified on the same limited scale: an index ranging from 1 to 500. 

On the VOC Index scale, because every indoor air environment contains a certain VOC background from constantly off-gassing sources the starting value is always mapped to 100. VOC Index above 100 means that there are more VOCs compared to the average (from cooking, cleaning, breathing, etc.) while a VOC Index below 100 means that there are fewer VOCs (fresh air from an opened window, air purifier, etc.).

NOx Index is being sensitive towards oxidizing gases. A NOx Index above 1 means that there are more NOx compounds compared to the average (cooking on a gas stove), while an NOx Index close to 1 means that there are (nearly) no NOx gases present, which is the case most of the time (or induced by fresh air from an open window, using an air purifier, etc.).

## Tasmota Configuration

To use the Grove SEN5x sensors with Tasmota you need an ESP based [development board](https://templates.blakadder.com/diy.html#development%20board). Tasmota32 and tasmota-sensors build include the driver for Grove SEN5x sensors by default.

I've used the small [XIAO ESP32C3](https://www.seeedstudio.com/Seeed-XIAO-ESP32C3-p-5431.html?sensecap_affiliate=jo7uUTK&referring_service=blog) development board and its [Grove Base board](https://www.seeedstudio.com/Grove-Shield-for-Seeeduino-XIAO-p-4621.html?sensecap_affiliate=jo7uUTK&referring_service=blog) which enables me to connect other Grove peripherals.

Connect Grove SEN5x to the Grove port of your board. If you're using Grove to Dupont cable connect the wires as pictured:

![Grove pinout](/assets/images/sen55/grove_pinout.jpg)

Install Tasmota using the [Tasmota Web Installer](https://tasmota.github.io/install/) or using [other installation options](https://tasmota.github.io/docs/Getting-Started/#flashing).

![Web Intaller](/assets/images/sen55/web_installer.jpg)

Once installed, [configure Tasmota](https://tasmota.github.io/docs/Getting-Started/#initial-configuration) to connect to your Wi-Fi network and open the webUI in the browser using its new IP address.

In the webUI navigate to **Configuration -> Configure Module** and configure the I2C SDA and I2C SCL pins according to how you wired them. In my case (XIAO ESP32C3 and Grove Base) the pins are fixed to GPIO6 for SDA and GPIO7 for SCL.

![Configure module](/assets/images/sen55/module_config.jpg)

Clicking **Save** will restart Tasmota and the main page will now display all the sensor data.

![Tasmota UI](/assets/images/sen55/tasmota_ui.jpg)

Tasmota's implementation of the sensor additionally provides absolute humidity (aHumidity) and dew point sensors.

All the data is also transmitted via MQTT. Tasmota can be integrated in various [smart home solutions](https://tasmota.github.io/docs/Integrations/) and from version 13 of Tasmota you can even use [Matter](https://tasmota.github.io/docs/Matter/) over Wi-Fi (Current Matter specification supports only temperature and humidity sensors).

## Conclusion

All the readings during my testing were accurate and in a trend line with my sensor testing setup. The sensor with the XIAO ESP32C3 board uses around 0.8W.

[![PM Sensor](/assets/images/sen55/pm_sensor.jpg){: width="41.5%"}](/assets/images/sen55/pm_sensor.jpg)[![The case](/assets/images/sen55/th_sensor.jpg){: width="58.5%"}](/assets/images/sen55/th_sensor.jpg)

The VOC and NOx index is great for creating automations since it gives your values applicable in real life scenarios.

The price of 55$ (49$ for SEN54) might seem high but good and accurate environment sensors like this cannot be bought cheaply and I find this a good value for what you receive.

It would be perfect if the package also implemented CO2 detection to be a true environment sensor. For now you need to add a CO2 sensor like [SCD41](https://www.seeedstudio.com/Grove-CO2-Temperature-Humidity-Sensor-SCD41-p-5025.html?sensecap_affiliate=jo7uUTK&referring_service=blog), [SCD30](https://www.seeedstudio.com/Grove-CO2-Temperature-Humidity-Sensor-SCD30-p-2911.html?sensecap_affiliate=jo7uUTK&referring_service=link) or [MH-Z19](https://www.aliexpress.com/item/1005003727471729.html?aff_fcid=454aca992b6d4387823f0cb8c724a6e4-1688322885581-03850-_DeL9zjx&tt=CPS_NORMAL&aff_fsk=_DeL9zjx&aff_platform=shareComponent-detail&sk=_DeL9zjx&aff_trace_key=454aca992b6d4387823f0cb8c724a6e4-1688322885581-03850-_DeL9zjx&terminal_id=3f8c776975fd455ba956809c02d71a91&afSmartRedirect=y) yourself.