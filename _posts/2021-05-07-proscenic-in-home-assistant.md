---
layout: post
title: "Proscenic T21 Air Fryer in Home Assistant Using Tasmota"
author: blak
categories: [ tasmota, home assistant, how to ]
tags: [ ]
image: assets/images/header_climate.jpg
toc: true
---

Guide to seting up [Proscenic T21](https://templates.blakadder.com/proscenic_T21.html) air fryer running Tasmota in Home Assistant with all automations and UI configuration

***All information applies only to Home Assistant 2021.5.0+ and Tasmota 9.4+***

This guide assumes you've correctly set up Tasmota on the [Proscenic T21](https://templates.blakadder.com/proscenic_T21.html) and the Tasmota integration in Home Assistant.

Tasmota was configured like this:

```console
Backlog DeviceName Proscenic; FriendlyName1 Activate Fryer; FriendlyName2 Start/Pause Cooking; FriendlyName3 Keep Warm; FriendlyName4 Delayed Cook; SetOption8 1; Rule0 1
```

Even though the temperature is displayed in °F, Home Assistant will convert the discovered temperature sensor to your unit of measurement.

## Add Device to Home Assistant
Make sure the Tasmota device is discovered in Home Assistant under Tasmota integration. If everything is configured correctly, Home Assistant's **Configuration - Devices** list should have a device "Proscenic" and will show the following entities.

![Device Card](/assets/images/proscenic/device_card_before.jpg)

If you want to customize any of the entities click their name!

## Tasmota Rule
Create a rule set that will dispatch received data from the Tuya MCU to separate topics. Some rules use variables to receive data from Home Assistant and forward them to the Tuya MCU.

```console
rule3 
on tuyareceived#dptype4id3 do publish %topic%/cookbook %value% endon 
on tuyareceived#dptype4id5 do publish %topic%/mode %value% endon 
on tuyareceived#dptype2id103 do publish %topic%/cookingtemp %value% endon 
on var3#state do tuyasend2 103,%value% endon 
on tuyareceived#dptype2id7 do publish %topic%/cooktime %value% endon 
on var7#state do tuyasend2 7,%value% endon 
on tuyareceived#dptype2id105 do publish %topic%/warmtime %value% endon 
on var5#state do tuyasend2 105,%value% endon 
on tuyareceived#dptype2id6 do publish %topic%/delayedtime %value% endon 
on var6#state do tuyasend2 6,%value% endon 
```

## Autodiscover Entities Rule

Next, import a blueprint for an automation that will generate and publish all necessary MQTT discovery messages so all the sensors will be in the device card for the air fryer.

<a href="https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://github.com/tasmota/blueprints/blob/main/discovery-proscenic-t21.yaml" title="Import Proscenic T21 Discovery "><img loading="lazy" src="/assets/blueprint_import.svg"></a>

Automation will run on every Home Assistant restart or when Proscenic T21 reports its online to the MQTT broker.

Resulting Device Card will look like this:

![Device Card After](/assets/images/proscenic/device_card_after.jpg)

Once all the sensors are discovered, set up [Helpers](https://my.home-assistant.io/redirect/helpers)

## Dropdown Helper

[Create](https://my.home-assistant.io/redirect/helpers) a dropdown aka input select helper that will let you select the cooking mode from the UI. 

The helper should be like this:

![Dropdown Helper](/assets/images/proscenic/input_select.jpg)

Values in the helper are used in the related automation, so don't change anything unless you're changing the blueprint too.

After creating the helper, import blueprint and create the automation.

<a href="https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://github.com/tasmota/blueprints/blob/main/proscenic_t21_cookbook.yaml" title="Import Proscenic T21 Cookbook"><img loading="lazy" src="/assets/blueprint_import.svg"></a>

## Number Helper

[Create](https://my.home-assistant.io/redirect/helpers) a number helper that will be used to set the cooking temperature. 

### Temperature in Celsius
If you use Celsius in Home Assistant set the helper like this: 

![Number Helper for °C](/assets/images/proscenic/input_number.jpg)

Automation will do the necessary conversion from Celsius to Fahrenheit, which is the only unit Proscenic accepts.

<a href="https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://github.com/tasmota/blueprints/blob/main/proscenic_t21_cooking_temperature_celsius.yaml" title="Import Proscenic T21 Cookbook in °C"><img loading="lazy" src="/assets/blueprint_import.svg"></a>

### Temperature in Fahrenheit

If you use Fahrenheit in Home Assistant set the helper like this: 

![Number Helper for °F](/assets/images/proscenic/input_number_fahrenheit.jpg)

<a href="https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://github.com/tasmota/blueprints/blob/main/proscenic_t21_cooking_temperature.yaml" title="Import Proscenic T21 Cookbook in °F"><img loading="lazy" src="/assets/blueprint_import.svg"></a>

### Lovelace Card

To bring it all together there's a Lovelace UI card utilising conditional cards because not all fryer options are available at all time.

![Lovelace ](/assets/images/proscenic/lovelace0.jpg)
![Lovelace 1](/assets/images/proscenic/lovelace1.jpg)
![Lovelace 2](/assets/images/proscenic/lovelace2.jpg)

If you followed the guide using the same names you can paste the entire configuration in the UI.

```yaml
type: vertical-stack
cards:
  - type: button
    tap_action:
      action: toggle
    entity: switch.activate_fryer
    icon_height: 80px
    icon: 'mdi:power'
    show_name: false
  - type: conditional
    conditions:
      - entity: switch.activate_fryer
        state: 'on'
      - entity: switch.start_pause_cooking
        state: 'off'
    card:
      type: entities
      entities:
        - entity: input_select.proscenic_cookbook
  - type: conditional
    conditions:
      - entity: sensor.cookbook
        state: Custom
      - entity: switch.activate_fryer
        state: 'on'
      - entity: switch.start_pause_cooking
        state: 'off'
    card:
      type: entities
      entities:
        - entity: input_number.proscenic_cooking_temp
        - entity: number.cooking_time_set
        - entity: switch.keep_warm
        - entity: number.keep_warm_time_set
        - entity: switch.delayed_cook
        - entity: number.delayed_cook_time_set
      state_color: true
  - type: conditional
    conditions:
      - entity: switch.activate_fryer
        state: 'on'
    card:
      type: button
      tap_action:
        action: toggle
      entity: switch.start_pause_cooking
      icon: 'mdi:play'
      show_name: false
      icon_height: 80px
      show_state: false
      name: Start Cooking
  - type: conditional
    conditions:
      - entity: switch.start_pause_cooking
        state: 'on'
      - entity: switch.activate_fryer
        state: 'on'
    card:
      type: horizontal-stack
      cards:
        - type: entity
          entity: sensor.cooking_mode
          name: Status
        - type: entity
          entity: sensor.proscenic_tuyasns_timer1
          name: Time Remaining
          icon: 'mdi:timer-sand'
          unit: minutes
        - type: entity
          entity: sensor.proscenic_tuyasns_temperature
          name: Cooking Temperature
```