---
layout: post
title: "IKEA Vindriktning air quality sensor running Tasmota"
author: blak
categories: [ tasmota, how to ]
tags: [ ]
image: assets/images/header_vindriktning.jpg
toc: true
---

IKEA Vindriktning is an affordable air quality sensor with a well build enclosure and you can connect it to your smart home using Tasmota and some soldering

_Links in this article marked with a * are affiliate links. By purchasing through them I get a commission that helps in future projects!_

Thanks to the [Hypfer/esp8266-vindriktning-particle-sensor](https://github.com/Hypfer/esp8266-vindriktning-particle-sensor) project I found out that IKEA's new Vindriktning sensor can be connected to an ESP module. Considering it costs only 10€ I decided to get one to play around with and in the meantime Tasmota added a driver for it, perfect!

## Disassembly
Vindriktning has a nice, unobtrusive design and its powered by USB Type C. Its very easy to open by unscrewing only 4 screws which gets you access to the PCB and the spacious internals. While the top part is very spacious I would recommend fitting the ESP8266 board in the bottom, below the fan. This will keep the airflow to the PM2.5 sensor unobstructed.

![Opened](/assets/images/vindriktning/opened.jpg)

<figcaption class="figure-caption text-center">Image from https://www.pokipsie.ch/ because I forgot to take one before the modifications.</figcaption>

To make it smarter you will need:

- a soldering iron - a basic soldering iron is sufficient for small projects like this. I use a [Mustool MT883 from Banggood](https://www.banggood.com/custlink/Dv3YBFghm0)* for a couple of years now.
- good solder and flux - don't get the super cheap stuff and definitely don't use unleaded solder. I typically get [Mechanic solder](https://www.aliexpress.com/item/4001063085857.html?aff_fcid=734adb3158bd494784ddfe073f81db76-1630272706741-07641-_9G5FHa&tt=CPS_NORMAL&aff_fsk=_9G5FHa&aff_platform=shareComponent-detail&sk=_9G5FHa&aff_trace_key=734adb3158bd494784ddfe073f81db76-1630272706741-07641-_9G5FHa&terminal_id=c60aa1c2bd3d4f80907b0cc2716fb935)* and [RMA223 flux](https://www.aliexpress.com/item/32890310514.html?aff_fcid=17e4e67946eb482c9f678cc0aa234d5b-1630272650435-00951-_AgnWPw&tt=CPS_NORMAL&aff_fsk=_AgnWPw&aff_platform=shareComponent-detail&sk=_AgnWPw&aff_trace_key=17e4e67946eb482c9f678cc0aa234d5b-1630272650435-00951-_AgnWPw&terminal_id=c60aa1c2bd3d4f80907b0cc2716fb935)*.
- wires - I reuse scrap wire 
- an ESP board or module - a Wemos D1 mini without the header pins soldered fits nicely below and can be powered by 5V directly. I opted for an [ESP-M3](https://templates.blakadder.com/ESP-M3) module with an [AMS1117 breakout board](https://www.aliexpress.com/item/1005003051751791.html?aff_fcid=5d85150be83048d7a75c9925a9468a89-1630265576181-03303-_997Dpw&tt=CPS_NORMAL&aff_fsk=_997Dpw&aff_platform=shareComponent-detail&sk=_997Dpw&aff_trace_key=5d85150be83048d7a75c9925a9468a89-1630265576181-03303-_997Dpw&terminal_id=c60aa1c2bd3d4f80907b0cc2716fb935)* voltage regulator to convert 5V to 3.3V needed by the ESP. Goes without saying, any other ESP module would work, you can even use an ESP32 if you find a small enough board to fit inside. Make sure to flash Tasmota to the module before soldering everything.

## Soldering
Unplug the two connectors from the PCB and remove it from the enclosure so you can work with it easily.

![PCB](/assets/images/vindriktning/pcb.jpg)

The PCB has all the pads labelled and easily accessible. You will use the top row, ones labelled: 5V, GND and REST. 

Pre-tin the pads and wires for easier soldering. Always use plenty of flux, don't skimp on it. Don't keep the soldering iron too long on the pads to avoid burning them off with excessive heat.

![Pre-tin](/assets/images/vindriktning/pretin.jpg)

Because everything is pre-tinned, soldering the three wires requires just brief contact with the soldering iron set to around 350°C until the solder melts on the pad and the wire and fuses together. Make sure the wires will have enough length to reach the ESP module. Make sure the wires are held strong enough and that there are no shorts between them.

![Soldered](/assets/images/vindriktning/soldered.jpg)

Connect the wires as follows:

- REST to any [safe](https://github.com/thehookup/Wireless_MQTT_Doorbell/blob/master/GPIO_Limitations_ESP8266_NodeMCU.jpg?raw=true) free pin, I used GPIO5 aka D1. This is the pin used to receive data from the air quality sensor.
- 5V to the Vin of the voltage regulator or 5V pin on the D1 mini
- GND to GND

Here is the finished product with a little extra: 4 extra wires with a [JST connector](https://www.aliexpress.com/item/32992681983.html?aff_fcid=de07842fc9f04052867660a05f746553-1630267556133-09478-_9jguMG&tt=CPS_NORMAL&aff_fsk=_9jguMG&aff_platform=shareComponent-detail&sk=_9jguMG&aff_trace_key=de07842fc9f04052867660a05f746553-1630267556133-09478-_9jguMG&terminal_id=c60aa1c2bd3d4f80907b0cc2716fb935)* are soldered for connecting additional I2C sensors. More on that at the end...

![Wired](/assets/images/vindriktning/wiring.jpg)

To fit the new wires seamlessly I made a hole on the other side.

![Dremel](/assets/images/vindriktning/dremel.jpg)

## Tasmota Configuration

To have support for Vindriktning's sensor you need at least Tasmota version 9.5.0.7. Since it is not included in standard builds, you need to enable the sensor by adding it to `user_config_override.h`:

```c+
#define USE_VINDRIKTNING    // Add support for IKEA VINDRIKTNING particle concentration sensor (+1k code)¸
```

Or, download an unofficial build [`tasmota-allsensors`](https://github.com/tasmota/install/raw/main/firmware/unofficial/tasmota-allsensors.bin) which supports almost all sensors, Vindriktning included. 

Once the firmware is [installed](https://tasmota.github.io/docs/Getting-Started/#hardware-preparation) on the ESP module go to **Configuration - Configure Module** and set the pin you connected the REST wire to as `VINDRIKTNING`. In my case it was GPIO5.

![Module Config](/assets/images/vindriktning/moduleconfig.jpg)

After a reset, if everything went according to plan you will see the values in the webUI

![webUI](/assets/images/vindriktning/webui.jpg)

## Adding more sensors

Because there's a lot of room in the enclosure and the power supply is decent it would be a shame not to use that and add some more sensors in. Using I2C you can daisy chain a few of them using only two wires for communication with the ESP plus the wires needed to power them.

Since Vindriktning is an air quality sensor measuring particulate matter in the air, I decided to add moar air quality.

Sensors I had on hand were an [iAQ Core](https://www.aliexpress.com/item/33044332335.html?aff_fcid=2ba8e77d415d4fe8a4bc2f88379398bc-1630270811682-02300-_9zdXO4&tt=CPS_NORMAL&aff_fsk=_9zdXO4&aff_platform=shareComponent-detail&sk=_9zdXO4&aff_trace_key=2ba8e77d415d4fe8a4bc2f88379398bc-1630270811682-02300-_9zdXO4&terminal_id=c60aa1c2bd3d4f80907b0cc2716fb935)* for eCO2 and eTVOC and a [BME680](https://www.aliexpress.com/item/1005002831174240.html?aff_fcid=fbce2215ed7e414b84988631b8e2a0bc-1630270871559-09849-_9G2UTa&tt=CPS_NORMAL&aff_fsk=_9G2UTa&aff_platform=shareComponent-detail&sk=_9G2UTa&aff_trace_key=fbce2215ed7e414b84988631b8e2a0bc-1630270871559-09849-_9G2UTa&terminal_id=c60aa1c2bd3d4f80907b0cc2716fb935)* for temperature, humidity and pressure. I didn't have any BME280 on hand which would be a more sensible choice. You can add any combination of different sensors and simply plug them in the JST connector.

As you can see, it all fits nicely and there's room for more. I'm thinking about drilling a hole in the top for a [TSL2561](https://www.aliexpress.com/item/1005003035715609.html?aff_fcid=00a65af26cb241a0a57d3ffe8919116e-1630271156169-09125-_9iqQtW&tt=CPS_NORMAL&aff_fsk=_9iqQtW&aff_platform=shareComponent-detail&sk=_9iqQtW&aff_trace_key=00a65af26cb241a0a57d3ffe8919116e-1630271156169-09125-_9iqQtW&terminal_id=c60aa1c2bd3d4f80907b0cc2716fb935)* illumination sensor.
![Vindriktning Plus](/assets/images/vindriktning/plus.jpg)

![Vindriktning Plus UI](/assets/images/vindriktning/plusui.jpg)

<figcaption class="figure-caption text-center">Must...Have...All...Air quality readings!!!</figcaption>

Using Tasmota integration in Home Assistant gets you loads of sensors and data.

![Vindriktning Plus in Home Assistant](/assets/images/vindriktning/homeassistant.jpg)

What sensors would you add?
