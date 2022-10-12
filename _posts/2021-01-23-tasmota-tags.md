---
layout: post
title: "Tags in Home Assistant Using Tasmota RFID Reader"
author: blak
categories: [ tasmota, home assistant, how to ]
tags: [ ]
image: assets/images/header_tags.jpg
toc: true
---

Set up Home Assistant tags using Tasmota RFID/NFC reader and custom discovery messages. No configuration.yaml editing required!

***All information applies to Home Assistant 2020.12.0+ and Tasmota 9.2+***

This guide assumes you've correctly set up an RFID/NFC reader in Tasmota and your tags are scanned and identified in the web UI and/or console.

Tasmota device used in this guide is configured as follows:

```console
Backlog DeviceName RFID Reader; Topic rfid
```

## Add Device to Home Assistant

Make sure the device is discovered in Home Assistant under Tasmota integration. If everything is configured correctly, Home Assistant's **Configuration - Devices** list should have a device "RFID Reader".

![Device Card](/assets/images/tags/device_card_tags.jpg)

## Setup Tags Discovery

To add the scanned tags to Home Assistant tags we have to create a discovery configuration message.

This is that message written in yaml so it is easier to view and edit. Most of the configuration does not need to be changed and will be automatically populated when sent from a rule in Tasmota.

```yaml
topic: tele/%topic%/SENSOR
value_template: {% raw %}"{{value_json['RC522']['UID']}}"{% endraw %}
device:
  connections: [[mac,'%macaddr%']]
```

## Discovery Configuration Analysis
```yaml
topic: tele/%topic%/SENSOR
```
This is the topic to which scanned tags are published. There is no need to edit this since the `%topic%` rule variable will be replaced with your device topic when the rule is executed.

### Value Template Options

```yaml
value_template: {% raw %}"{{value_json['RC522']['UID']}}"{% endraw %}
```

Filter part of the JSON payload to identify the tag. It consists of two parts, first defines the RFID reader used and second the field used for identification.

#### Choose RFID Reader 

Every RFID reader in Tasmota publishes to JSON under its own key. To receive those messages Home Assistant you need to change `RC522` to the type of reader you have:

* [PN532](https://tasmota.github.io/docs/PN532/) (HSU) NFC Tag Reader to `PN532`
* [RDM6300](https://tasmota.github.io/docs/RDM6300/) 125kHz NFC Tag Reader to `RDM6300`
* [MFRC522](https://tasmota.github.io/docs/MFRC522/) (RFID-RC522) NFC Tag Reader to `RC522` (used in this tutorial)
* Wiegand Interface 125kHz NFC Tag Reader to `Wiegand` 

#### Choose Identifying Field

Depending on the used RFID reader there will be extra data published when a tag is scanned. `UID` will be scanned with every reader type and is the preferred filed to use and is used in this tutorial.

`Data` is another useful field since it can be edited to a custom message (again, depending on the reader used!). PN532 has an option to write to a tag from Tasmota console.

#### Be Specific

While you can use or `value_json.<reader>` _(1)_ or only `value_json` _(2)_  the result in Home Assistant might not be what you need and might get triggered on different sensor messages:

![Discovered tag options](/assets/images/tags/ha_discovered_tags.jpg)

Best result is achieved when using the suggested value_template line _(3)_.

### Device Identifier 

```yaml
device:
  connections: [[mac,'%macaddr%']]
```

`device:` value will link the new tags sensor to the existing device in Tasmota integration using the mac address attribute. The `%macaddr%` rule variable will be replaced with the device mac address when the rule is executed.

This is currently not required for tags but it doesn't hurt to leave it in.

You can change unique_id but there's no reason to do so unless you have multiple motion sensors on the same device.

## YAML to JSON

Take the yaml block and convert it to JSON. I recommend using [onlineyamltools](https://onlineyamltools.com/convert-yaml-to-json). Make sure to set _Output indent size_ to **0**.

![YAML to JSON](/assets/images/tags/yaml_to_json_tags.jpg)

Copy the json output to clipboard.

## Rule to Trigger Discovery

In Tasmota we will add a new rule that will send a discovery message on Tasmota startup.

```console
rule1 on system#boot do publish2 homeassistant/tag/%macaddr%_tag/config <paste json output here> endon
```

Publish2 will send a retained message to topic `publish2 homeassistant/tag/%macaddr%_tag/config` with the payload that was created and converted to JSON:

```json
{% raw %}{"topic":"tele/%topic%/SENSOR","value_template":"{{value_json['RC522']['UID']}}","device":{"connections":[["mac","%macaddr%"]]}}{% endraw %}
```

Enable the rule with `Rule1 1`.

## Use Tags 
Once Tasmota is restarted (`Restart 1`), the discovery configuration message will be sent.

```json
RUL: SYSTEM#BOOT performs "publish2 homeassistant/tag/D198079204F3_tag/config {"topic":"tele/rfid/SENSOR","value_template":...
```
There will be no new entities showing in the device card for the "RFID Reader". 

To show the newly discovered tags navigate to **Configuration - Tags** and any newly scanned tag will appear there.

![Tag Scanned](/assets/images/tags/tag_scanned.jpg)

## Remove Tag Discovery

Simply send an empty retained MQTT message to the configuration topic used.

Easiest way is from Tasmota using `publish2` command. For this example it would be `publish2 homeassistant/tag/D198079204F3_tag/config`.

And don't forget to delete or disable the rule publishing the discovery message on boot. 