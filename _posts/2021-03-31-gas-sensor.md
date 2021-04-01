---
layout: post
title: "Tasmota Gas and Heat Alarm in Home Assistant"
author: blak
categories: [ tasmota, home assistant, how to ]
tags: [ ]
image: assets/images/header_gas_alarm.jpg
toc: true
---

Guide to adding a Tuya gas and heat alarm running Tasmota to Home Assistant as device triggers or binary sensors. No configuration.yaml editing required! 

***All information applies only to Home Assistant 2021.3.4+ and Tasmota 9.3.1.2+***

This guide will delve deep into correctly setting up the [Digoo DG-ZXGS21](https://templates.blakadder.com/digoo_DG-ZXGS21.html) gas and heat alarm in Home Assistant. Alongside the entities added through the [official Tasmota integration](https://www.home-assistant.io/integrations/tasmota/) you will see how to create Device triggers and various binary sensors for the device.

## Rules Setup in Tasmota
After you've [flashed the device](https://templates.blakadder.com/digoo_DG-ZXGS21.html) you need to configure all TuyaMCU functions and create a set of rules that are needed to create device triggers or binary sensors in Home Assistant. 

After switching to `Module 54` run these commands to configure TuyaMCU:

```console
Backlog TuyaMCU 79,2; TuyaMCU 71,19; TuyaMCU 72,101; TuyaMCU 11,16; TuyaMCU 12,8; TempRes 0
```

Set Device Name, FriendlyNames and activate all rulesets for future use:

```console
Backlog DeviceName Gas Sensor; FriendlyName1 Mute Alarm; FriendlyName2 Run Test; Rule1 1; Rule2 1; Rule3 1
```

If you've configured the [Tasmota integration](https://www.home-assistant.io/integrations/tasmota/) in Home assistant, the following device will be discovered:

![Initial Device Card](/assets/images/gas-sensor/initialdevicecard.jpg)

So far you have:
  - "Alarm" entity to mute the alarm 
  - "Run Test" entity to trigger the self test
  - Gas sensor showing gas concentration values in %LEL
  - Temperature sensor showing current temperature
  - Set Temperature sensor showing temperature threshold for triggering the heat alarm

Gas and heat alarm triggers are missing as well as preheat mode and lifecycle sensors. You can add alarm triggers as [device triggers](https://www.home-assistant.io/integrations/device_trigger.mqtt/) or [binary sensors](https://www.home-assistant.io/integrations/binary_sensor.mqtt/) or both if you want to.

## Device Triggers

Set a rule to send gas alarm and heat alarm states to their respective custom topics. (`+` will append the rule line to already existing rules)

```console
rule1 + ON tuyareceived#dptype4id1 DO publish stat/%topic%/gas_alarm %value% ENDON ON tuyareceived#dptype4id18 DO publish stat/%topic%/heat_alarm %value% ENDON
```

This will publish a message of `0` on a triggered alarm or `1` as default to custom topics.

To create a device trigger in HA using MQTT discovery use a rule to send the discovery message on system boot:

```console
rule2 + ON system#boot DO publish2 homeassistant/device_automation/%macaddr%/gas_alarm/config {"automation_type":"trigger","payload":0,"subtype":"Gas Alarm","type":"triggered","topic":"stat/%topic%/gas_alarm","device":{"connections":[["mac","%macaddr%"]]}} ENDON ON system#boot DO publish2 homeassistant/device_automation/%macaddr%/heat_alarm/config {"automation_type":"trigger","payload":0,"subtype":"Heat Alarm","type":"triggered","topic":"stat/%topic%/heat_alarm","device":{"connections":[["mac","%macaddr%"]]}} ENDON 
```

There is no need to change anything, this string will work on every system. After a reboot of Tasmota you can use the triggers in your automations.

![Device triggers](/assets/images/gas-sensor/triggers.jpg)


The gas alarm trigger is created in YAML so its more readable. Use a [converter](https://www.convertjson.com/yaml-to-json.htm) to get a JSON string (make sure to check "Minimize JSON")) used as the payload in the MQTT message:

```yaml
automation_type: trigger
payload: 0
subtype: Gas Alarm
type: triggered
topic: 'stat/%topic%/gas_alarm'
device:
  cns: [[mac,'%macaddr%']]
```

_In the actual JSON string shortened names are used to save on rules space._

It needs to be sent to topic `homeassistant/device_automation/%macaddr%/gas_alarm/config` with a retain flag.

## Binary Sensor for Gas Alarm

Use the same Rule1 as for Device triggers to send values to topics used by HA binary sensors.

Create a discovery configuration message for the binary sensor by converting the following YAML to a JSON string. You can change the `name` which will change the entity name in HA.

```yaml
name: Gas Alarm
stat_t: stat/%topic%/gas_alarm
avty_t: tele/%topic%/LWT
pl_avail: Online
pl_not_avail: Offline
dev_cla: gas
pl_off: 0
pl_on: 1
uniq_id: '%macaddr%_gas_alarm'
device:
  cns: [[mac,'%macaddr%']]
```

Send it to topic `homeassistant/binary_sensor/%macaddr%/gas_alarm/config` on Tasmota boot with the rule:

```json
rule2 + ON system#boot DO publish2 homeassistant/binary_sensor/%macaddr%/gas_alarm/config {"name":"Gas Alarm","stat_t":"stat/%topic%/gas_alarm","avty_t":"tele/%topic%/LWT","pl_avail":"Online","pl_not_avail":"Offline","dev_cla":"gas","pl_off":1,"pl_on":0,"uniq_id":"%macaddr%_gas_alarm","device":{"cns":[["mac","%macaddr%"]]}} ENDON
```

## Binary Sensor for Heat Alarm

Same goes for the heat alarm with some fields changed:

```yaml
name: Heat Alarm
stat_t: stat/%topic%/heat_alarm
avty_t: tele/%topic%/LWT
pl_avail: Online
pl_not_avail: Offline
dev_cla: heat
pl_off: 1
pl_on: 0
uniq_id: '%macaddr%_heat_alarm'
device:
  cns: [[mac,'%macaddr%']]
```

Send it to MQTT topic `homeassistant/binary_sensor/%macaddr%/heat_alarm/config` on Tasmota boot with the rule:

```json
rule2 + ON system#boot DO publish2 homeassistant/binary_sensor/%macaddr%/heat_alarm/config {"name":"Heat Alarm","stat_t":"stat/%topic%/heat_alarm","avty_t":"tele/%topic%/LWT","pl_avail":"Online","pl_not_avail":"Offline","dev_cla":"heat","pl_off":1,"pl_on":0,"uniq_id":"%macaddr%_heat_alarm","device":{"cns":[["mac","%macaddr%"]]}} ENDON
```

## Binary Sensor for Preheat Mode

Add the rule
```console
rule1 + ON tuyareceived#dptype1id10 DO publish stat/%topic%/preheat %value% ENDON
```

Initial YAML:

```yaml
name: Preheat Mode
stat_t: stat/%topic%/preheat
avty_t: tele/%topic%/LWT
pl_avail: Online
pl_not_avail: Offline
pl_off: 0
pl_on: 1
uniq_id: '%macaddr%_preheat'
device:
  cns: [[mac,'%macaddr%']]

```

Send it to topic `homeassistant/binary_sensor/%macaddr%/preheat/config` on Tasmota boot with the rule:

```json
rule2 + ON system#boot do publish2 homeassistant/binary_sensor/%macaddr%/preheat/config {"name":"Preheat Mode","stat_t":"stat/%topic%/preheat","avty_t":"tele/%topic%/LWT","pl_avail":"Online","pl_not_avail":"Offline","pl_off":0,"pl_on":1,"uniq_id":"%macaddr%_preheat","device":{"cns":[["mac","%macaddr%"]]}} ENDON
```

## Binary Sensor for Lifecycle 

Add the rule
```console
rule1 + ON tuyareceived#dptype1id12 DO publish stat/%topic%/lifecycle %value% ENDON
```

Initial YAML:

```yaml
name: Lifecycle
stat_t: stat/%topic%/lifecycle
avty_t: tele/%topic%/LWT
pl_avail: Online
pl_not_avail: Offline
pl_off: 1
pl_on: 0
dev_cla: problem
uniq_id: '%macaddr%_lifecycle'
device:
  cns: [[mac,'%macaddr%']]

```

Send it to topic `homeassistant/binary_sensor/%macaddr%/lifecycle/config` on Tasmota boot with the rule:

```json
rule2 + ON system#boot DO publish2 homeassistant/binary_sensor/%macaddr%/lifecycle/config {"name":"Lifecycle","stat_t":"stat/%topic%/lifecycle","avty_t":"tele/%topic%/LWT","pl_avail":"Online","pl_not_avail":"Offline","pl_off":1,"pl_on":0,"dev_cla":"problem","uniq_id":"%macaddr%_lifecycle","device":{"cns":[["mac","%macaddr%"]]}} ENDON
```

## Device Card
Device card with all added and discovered entities.

![Final device card](/assets/images/gas-sensor/finaldevicecard.jpg)

## All in One Setup
If you just want to set everything up without changing anything: 

```console
Backlog TuyaMCU 79,2; TuyaMCU 71,19; TuyaMCU 72,101; TuyaMCU 11,16; TuyaMCU 12,8; TempRes 0
```

Set Device Name, FriendlyNames and activate all rulesets for future use:

```console
Backlog DeviceName Gas Sensor; FriendlyName1 Mute Alarm; FriendlyName2 Run Test; Rule1 1; Rule2 1; Rule3 1
```

```console
rule1 ON tuyareceived#dptype4id1 DO publish stat/%topic%/gas_alarm %value% ENDON ON tuyareceived#dptype4id18 DO publish stat/%topic%/heat_alarm %value% ENDON ON tuyareceived#dptype1id10 DO publish stat/%topic%/preheat %value% ENDON ON tuyareceived#dptype1id12 DO publish stat/%topic%/lifecycle %value% ENDON ON tuyareceived#dptype4id102=0 DO tuyasend4 102,1 ENDON
```

```console
rule2 ON system#boot DO publish2 homeassistant/device_automation/%macaddr%/gas_alarm/config {"automation_type":"trigger","payload":0,"subtype":"Gas Alarm","type":"triggered","topic":"stat/%topic%/gas_alarm","device":{"connections":[["mac","%macaddr%"]]}} ENDON ON system#boot DO publish2 homeassistant/device_automation/%macaddr%/heat_alarm/config {"automation_type":"trigger","payload":0,"subtype":"Heat Alarm","type":"triggered","topic":"stat/%topic%/heat_alarm","device":{"connections":[["mac","%macaddr%"]]}} ENDON ON system#boot DO publish2 homeassistant/binary_sensor/%macaddr%/gas_alarm/config {"name":"Gas Alarm","stat_t":"stat/%topic%/gas_alarm","avty_t":"tele/%topic%/LWT","pl_avail":"Online","pl_not_avail":"Offline","dev_cla":"gas","pl_off":1,"pl_on":0,"uniq_id":"%macaddr%_gas_alarm","device":{"cns":[["mac","%macaddr%"]]}} ENDON ON system#boot DO publish2 homeassistant/binary_sensor/%macaddr%/heat_alarm/config {"name":"Heat Alarm","stat_t":"stat/%topic%/heat_alarm","avty_t":"tele/%topic%/LWT","pl_avail":"Online","pl_not_avail":"Offline","dev_cla":"heat","pl_off":1,"pl_on":0,"uniq_id":"%macaddr%_heat_alarm","device":{"cns":[["mac","%macaddr%"]]}} ENDON 
```
```console
rule2 + ON system#boot DO publish2 homeassistant/binary_sensor/%macaddr%/lifecycle/config {"name":"Lifecycle","stat_t":"stat/%topic%/lifecycle","avty_t":"tele/%topic%/LWT","pl_avail":"Online","pl_not_avail":"Offline","pl_off":1,"pl_on":0,"dev_cla":"problem","uniq_id":"%macaddr%_lifecycle","device":{"cns":[["mac","%macaddr%"]]}} ENDON ON system#boot do publish2 homeassistant/binary_sensor/%macaddr%/preheat/config {"name":"Preheat Mode","stat_t":"stat/%topic%/preheat","avty_t":"tele/%topic%/LWT","pl_avail":"Online","pl_not_avail":"Offline","pl_off":0,"pl_on":1,"uniq_id":"%macaddr%_preheat","device":{"cns":[["mac","%macaddr%"]]}} ENDON
```