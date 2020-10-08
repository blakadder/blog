---
layout: post
title: "Tasmota Motion Sensor in Home Assistant Using Discovery"
author: blak
categories: [ tasmota, home assistant, how to ]
tags: [ ]
image: assets/images/header_pir.jpg
toc: true
---

The definitive (?) guide to setting up a PIR in Tasmota and adding it to Home Assistant using discovery. No configuration.yaml editing required!

***All information applies only to Home Assistant 0.114+ and Tasmota 8.4.0.2+***

You've connected a PIR sensor to your device and gone through [instructions](https://tasmota.github.io/docs/PIR-Motion-Sensors/) to set it up. Now you're left with custom topics and grappling with Home Assistant's configuration.yaml and even after all that it doesn't show up under the Device card in Home Assistant and you want the nifty Device triggers to use in automations. Let's fix all that! 

First, delete everything you did and start again! 


## Initial Setup
Example device is setup as follows: MQTT topic is set to `motion_test`, DeviceName is "Motion Test" and FriendlyName1 is "Relay". Relay1 and Switch1 are on the device, newly connected PIR sensor is configured as Switch2. 

![Module configuration](/assets/images/pir/module.jpg)

## Decouple PIR and Relay

By default the PIR sensor will trigger Relay1. Yes, even though it is `Switch2` or if it were higher number. To prevent it from controlling the relay, or decouple them we will use a rule.

```
rule1 on switch2#state do publish stat/%topic%/MOTION %value% endon
```

This will publish switch state (`0` = off or `1` = on) on each trigger to the topic defined (%topic% will be filled with the actual device topic)

By default switches sends only `TOGGLE` state (`2`). We will change that to on/off with `SwitchMode2 1`.

After all that you will have output like this in the console:
```haskell
20:37:42 RUL: SWITCH2#STATE performs "publish stat/motion_test/MOTION 1"
20:37:42 MQT: stat/motion_test/MOTION = 1
20:37:47 RUL: SWITCH2#STATE performs "publish stat/motion_test/MOTION 0"
20:37:47 MQT: stat/motion_test/MOTION = 0
```

## Add Device to Home Assistant

Now we will activate Home Assistant discovery with `SetOption19 1`. If everything is configured correctly, Home Assistant's **Configuration - Devices** list should have a new device: "Motion Test"

![HA Device Card](/assets/images/pir/device_card.jpg)

## Backlog Shortcut
You can do all of the abvove with just a single Backlog line:
```haskell
Backlog rule1 on switch2#state do publish stat/%topic%/MOTION %value% endon; rule1 1; switchmode2 1, so19 1
```

## Setup Manual Discovery
To add the PIR sensor to Home Assistant as a binary sensor we have to create a discovery configuration message. 

This is the configuration in yaml so it is easier to edit. Most of the configuration does not need to be changed and will be automatically populated when send from a rule in Tasmota.

```yaml
name: Motion Sensor
state_topic: stat/%topic%/MOTION
payload_on: 1
availability_topic: tele/%topic%/LWT
payload_available: Online
payload_not_available: Offline
device_class: motion
force_update: true
off_delay: 30
unique_id: '%deviceid%_motion'
device:
  identifiers:
    - '%deviceid%'
```

## Discovery Configuration Analysis
```yaml
name: Motion Sensor
```
Change it to a name you'd like displayed in Home Assistant. It will also determine the initial entity_id combined with device name, in this case `binary_sensor.motion_test_motion_sensor`.

```yaml
device_class: motion
```
This determines how Home Assistant displays the device in Lovelace. 

```yaml
state_topic: stat/%topic%/MOTION
```
this is the same as the topic defined in the PIR decoupling rule. Change only if you've changed the rule topic.

```yaml
payload_on: 1
```
Has to be identical to the payload shown in console output. Do not change unless you completely changed the rule. There is no payload off because we're using `off_delay` option.
```yaml
force_update: true
```
When true it will retrigger the binary sensor in HA even if the state is same. Very useful for a motion sensor.
```yaml
off_delay: 30
```
**This is the magic ingredient!** Here you can define a delay period after which Home Assistant will set the motion sensor state to off. Specially useful if you're using PIR sensors that switch to off after only a few seconds. 
```yaml
availability_topic: tele/%topic%/LWT
payload_available: Online
payload_not_available: Offline
```
Standard sensor availability configuration. Do not change.
```yaml
unique_id: '%deviceid%_motion'
device:
  identifiers:
    - '%deviceid%'
```
These are the important discovery bits. Thanks to the new `%deviceid%` rule variable these will be populated automatically. 

`device:` value will link the new PIR sensor to the existing device in HA Devices list. Do not change!

You can change unique_id but there's no reason to do so.

## YAML to JSON
Take the yaml block and convert it to JSON. I recommend using [onlineyamltools](https://onlineyamltools.com/convert-yaml-to-json). Make sure to set _Output indent size_ to **0**.

![YAML to JSON](/assets/images/pir/yaml-to-json.jpg)

Copy the json output to clipboard.

## Rule to Trigger Discovery
In Tasmota we will add a new rule that will send a discovery message on Tasmota startup.

```haskell
rule2 on system#boot do publish2 homeassistant/binary_sensor/%deviceid%_motion/config <paste json output here> endon
```

Publish2 will send a retained message to topic `homeassistant/binary_sensor/%deviceid%_motion/config` with the payload that was created and converted to JSON:

```json
{"name":"Motion Sensor","state_topic":"stat/motion_test/MOTION","payload_on":1,"availability_topic":"tele/motion_test/LWT","payload_available":"Online","payload_not_available":"Offline","device_class":"motion","force_update":true,"off_delay":30,"unique_id":"FB0DF2_motion","device":{"identifiers":["FB0DF2"]},"platform":"mqtt"}
```

Enable the rule with `Rule2 1`.

## Enjoy
Once Tasmota is restarted (`Restart 1`), the discovery configuration message will be sent.

```haskell
20:56:32 RUL: SYSTEM#BOOT performs "publish2 homeassistant/binary_sensor/FB0DF2_motion/config {"name":"Motion Sensor","state_topic":"stat/motion_test/MOTION","payload_on":1,"availability_topic":"tele/motion_test/LWT","payload_available":"Online","payload_not_available":"Offline","device_class":"motion","force_update":true,"off_delay":30,"unique_id":"FB0DF2_motion","device":{"identifiers":["FB0DF2"]}}"
20:56:32 MQT: homeassistant/binary_sensor/FB0DF2_motion/config = {"name":"Motion Sensor","state_topic":"stat/motion_test/MOTION","payload_on":1,"availability_topic":"tele/motion_test/LWT","payload_available":"Online","payload_not_available":"Offline","device_class":"motion","force_update":true,"off_delay":30,"unique_id":"FB0DF2_motion","device":{"identifiers":["FB0DF2"]}} (retained)
```

A new entity will pop up in Home Assistant under the same device.

![HA Device Card with PIR](/assets/images/pir/device_card_pir.jpg)

You can use the new Device Triggers in automations instead of entity_id.

![Automation Possibilities](/assets/images/pir/automation.jpg)

If you open **MQTT INFO** window you will find the discovery configuration payload you've created.

![MQTT INFO](/assets/images/pir/mqtt_info.jpg)

## Remove the Device
Simply send an empty retained MQTT message to the configuration topic shown in **MQTT INFO**.

Easiest way is from Tasmota using `publish2` command. For this example it would be `publish2 homeassistant/binary_sensor/FB0DF2_motion/config`.

And don't forget to delete or disable the rule publishing the discovery message. 