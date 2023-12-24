---
layout: post
title: "Convert Xiaomi MMC MHO-C401 E-Ink Bluetooth Sensor to Zigbee"
author: blak
categories: [ zigbee, diy ]
tags: [ ]
image: assets/images/header_mho-c401.jpg
toc: true
---

Guide on installing Zigbee firmware on the Bluetooth temperature and humidity sensor with e-ink display. The best Zigbee temperature and humidity sensor with an e-ink display is a Bluetooth one.

_**Full disclosure:** This device is purchased with the help of your donations. Links in this article are affiliate links. By purchasing through them I also get a small commission that funds future projects!_

Xiaomi MiaoMiaoCe MHO-C401 is available from the following stores:
- [Aliexpress](https://www.aliexpress.com/item/1005005933945983.html?aff_fcid=838cd87b9730419a949bb351a9225daf-1635037251054-04238-_9gTCE1&tt=CPS_NORMAL&aff_fsk=_9gTCE1&aff_platform=shareComponent-detail&sk=_9gTCE1&aff_trace_key=838cd87b9730419a949bb351a9225daf-1635037251054-04238-_9gTCE1&terminal_id=c60aa1c2bd3d4f80907b0cc2716fb935)
- [Banggood](https://www.banggood.com/Upgrade-Version-MMC-E-Ink-Screen-BT2_0-Smart-Bluetooth-Thermometer-Hygrometer-Works-with-App-Home-Gadget-Tools-p-1696154.html?ID=0&warehouse=CN&p=CM27171011078201412U&custlinkid=3691172)

Make sure you have a MHO-C401 sensor. The model name is displayed on the back of the box.

**WARNING** This process is not fool proof and something can go wrong at an inopportune time. This can leave the device inoperable and unable to flash with the web flasher but it will **NOT** be permanently bricked. It can always be flashed to its original firmware or Zigbee firmware using wires and a serial to USB adapter.

## OTA Firmware Update

Open the [web flasher by pvvx](https://pvvx.github.io/ATC_MiThermometer/TelinkMiFlasher.html) in a Chrome based browser (or Opera). Make sure you have the `#enable-experimental-web-platform-features` enabled by checking using the link for your web browser.

Click on <b>Connect</b>.

[![Click Connect](/assets/images/mho-c401/1.jpg)](/assets/images/mho-c401/1.jpg)

Select MHO-C401 from the list of discovered devices.

![Select MHO-C401 from the list](/assets/images/mho-c401/2.jpg)

Once connected click on **Do Activation** and wait for the activation to complete

[![Click on Do Activation](/assets/images/mho-c401/3.jpg)](/assets/images/mho-c401/3.jpg)

When activation completes the page will display options for firmware upgrade. Due to the firmware size you cannot go straight to Zigbee firmware and instead you first need to install custom firmware which was on version 4.6 at the time of writing this guide so click that option first.

[![Upgrade to custom firmware](/assets/images/mho-c401/4.jpg)](/assets/images/mho-c401/4.jpg)

Now click **Start Flashing**

[![Now click **Start Flashing**](/assets/images/mho-c401/5.jpg)](/assets/images/mho-c401/5.jpg)

Watch the firmware update progress in this section of the page.

[![Watch the flashing progress](/assets/images/mho-c401/6.jpg)](/assets/images/mho-c401/6.jpg)

The log window will show "Update done after xx seconds" once complete.

[![The log window will show "Update done after xx seconds" once complete.](/assets/images/mho-c401/7.jpg)](/assets/images/mho-c401/7.jpg)

Now you need to reconnect to the sensor with the new firmware by again clicking **Connect**. It will no longer show up as "MHO-C401" and will now be "MHO_XXXXXX" where XXXXXX is derived from its MAC address.

![Reconnect to the sensor with the new firmware.](/assets/images/mho-c401/8.jpg)

When connected the screen will show different options than with the original Xiaomi firmware. Now you can click on the **Zigbee Firmware:** option.

![Click on Zigbee firmware upgrade option.](/assets/images/mho-c401/9.jpg)

Confirm that you're aware the data will be deleted.

![Confirm that you're aware the data will be deleted](/assets/images/mho-c401/10.jpg)

Click on **Start Flashing**.

[![Click on **Start Flashing**](/assets/images/mho-c401/7.jpg)](/assets/images/mho-c401/11.jpg)

Confirm again.

![Confirm that you're aware the data will be deleted](/assets/images/mho-c401/12.jpg)

Watch the firmware update progress. First it will erase data then proceed to flash to the Zigbee firmware

![Erasing data progress](/assets/images/mho-c401/13.jpg)
![Flashing new firmware progress](/assets/images/mho-c401/14.jpg)

Log window will show when the process is completed and will display "Update done after xx seconds" if the firmware upgrade is successful.

[![The log window will show "Update done after xx seconds" once complete.](/assets/images/mho-c401/15.jpg)](/assets/images/mho-c401/15.jpg)

The device will now be in pairing mode and you can join it to your Zigbee coordinator. 

I've also recorded the flashing process as a [YouTube video](https://www.youtube.com/watch?v=-bbe58DRvDU) if that makes it easier for you.

## Troubleshooting

If an operation fails try to connect again. If that doesn't work, remove the battery for a few seconds and insert it back again then connect.

## Features 

When paired it will display the link icon left of the humidity reading.

![Paired to a Zigbee coordinator](/assets/images/mho-c401/linked.jpg)

When the "BT" icon is displayed it signifies a connection loss or that the device is not registered to a Zigbee network.

![BT icons](/assets/images/mho-c401/bt_icon.jpg)

When the "Identify" command is called or the back button is short pressed the "BT" icon will flash.

The button at the back has the following functionality with the new firmware:

- Short press - transmits temperature, humidity and battery readings.
- Short 2 seconds hold switches temperature display between Celsius and Fahrenheit.
- Long hold for 7+ seconds resets the device in pairing mode. It will remain in pairing mode until paired, even if you reinsert the battery.

## ZHA in Home Assistant

Your new Zigbee sensor will be discovered by Home Assistant when using [ZHA integration](https://www.home-assistant.io/integrations/zha/).

![Discovered by ZHA](/assets/images/mho-c401/zha_pairing.jpg)

Home Assistant will add temperature. humidity and battery entities.

![ZHA device card](/assets/images/mho-c401/zha_device_card.jpg)

### Settings

To configure device options such as unit display, on-screen smiley, etc, click on the three dots right of the **RECONFIGURE** option.

![Configure](/assets/images/mho-c401/config_1.jpg)

Select **Manage Zigbee device**.

![Select **Manage Zigbee device**](/assets/images/mho-c401/config_2.jpg)

In the "Clusters" dropdown select **UserInterface**

![In the "Clusters" dropdown select **UserInteface**](/assets/images/mho-c401/config_3.jpg)

#### Displayed Temperature Unit

To change temperature unit on the display open "Attributes of the selected cluster" dropdown and select "temperature_display_mode (id: 0x0000)".

![ open "Attributes of the selected cluster" ](/assets/images/mho-c401/config_4.jpg)

Clicking on **READ ATTRIBUTE** will show the current configuration. The current setting is shown in the "Value" field: 

- `TemperatureDisplayMode.Metric` is for Celsius
- `TemperatureDisplayMode.Imperial` for Fahrenheit

To change the setting enter in the same "Value" field:

- **0** for Celsius
- **1** for Fahrenheit

and click on **WRITE ATTRIBUTE**.

![To change enter in the same "Value" field](/assets/images/mho-c401/config_5.jpg)

After a few seconds you can check if the setting was successful by reading the attribute again.

#### Smiley Display

Same as the previous option but select "schedule_programming_visibility (id: 0x0002)".

![Smiley display options](/assets/images/mho-c401/config_6.jpg)

Clicking on **READ ATTRIBUTE** will show the current configuration. The current setting is shown in the "Value" field:

- `ScheduleProgrammingVisibility.Enabled` will display the smiley according to the comfort parameters
- `ScheduleProgrammingVisibility.undefined_0x01` for no smiley on display

To change the setting enter in the same "Value" field:

- **0** for displaying the smiley
- **1** for not displaying the smiley

#### Comfort Levels for Temperature and Humidity

To set comfort levels of temperature and humidity which will change the smiley display you need to install [ZHA Toolkit](https://github.com/mdeweerd/zha-toolkit?tab=readme-ov-file#setup) first to get the required write attribute service.

Open the Services tab with this link [![Open your Home Assistant instance and show your service developer tools.](https://my.home-assistant.io/badges/developer_services.svg)](https://my.home-assistant.io/redirect/developer_services/)

Find the "ZHA Toolkit: Write Attribute service". Select an entity that belongs to the MHO-C401N. "Target Endpoint" is always 1 and "Target Cluster" is always 516.

To set Attribute ID to:

- `258` for minimum comfort temperature in 1° steps in range of `-127..+127`
- `259` for maximum comfort temperature in 1° steps in range of `-127..+127`
- `260` for minimum comfort humidity in 1% steps in range of `0..100`
- `261` for maximum comfort humidity in 1% steps in range of `0..100`

Click **CALL SERVICE** to send the new configuration. If successful the button will show a green checkmark.

[![Set comfort levels](/assets/images/mho-c401/set_comfort.jpg)](/assets/images/mho-c401/set_comfort.jpg)

Video of the setting process:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/A38mhNQ1Jw4?si=zTZtQr_uOpK7QnSH" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### OTA Upgrade / Revert to BLE firmware

pvvx's firmware has the option to upgrade the firmware OTA and that includes "upgrading" back to the BLE version of the firmware.

First you need to create a folder for the update files and add it to `configuration.yaml`

```yaml
zha:
  zigpy_config:
    ota:
      otau_directory: /config/zigpy_ota
```

In this example a directory named `zigpy_ota` is placed in the default HA /config directory.

Download an upgrade file named `1141-0208-xxxxxxxx-ZMHOC401N.zigbee` (`xxxxxxxx` is the version number) from the [`/bin`](https://github.com/pvvx/ZigbeeTLc/tree/master/bin) folder of the GitHub repository. 

To revert to BLE download this [`.zigbee` file](https://github.com/pvvx/ATC_MiThermometer/raw/master/zigbee_ota/1141-0208-99993001-MHO_C401N_v46.zigbee). 

Place the downloaded file to the created directory. Restart Home Assistant to update the configuration. The OTA process should trigger during restart and start upgrading the device. Enable debug logging for ZHA integration and check in logs for the OTA process progress. This upgrade can take upwards to 25 minutes.

When the upgrade is complete delete the file from the OTA directory.

If the update doesn't start you can trigger it manually using the services tab: [![Open your Home Assistant instance and show your service developer tools.](https://my.home-assistant.io/badges/developer_services.svg)](https://my.home-assistant.io/redirect/developer_services/).

[![Service in the UI](/assets/images/mho-c401/update_trigger.jpg)](/assets/images/mho-c401/update_trigger.jpg)

```yaml
service: zha.issue_zigbee_cluster_command
data:
  ieee: "ieee address"
  endpoint_id: 1
  cluster_type: out
  command_type: client
  cluster_id: 25
  command: 0
  args:
    - 0
    - 100
```

If you've already installed [ZHA Toolkit](https://github.com/mdeweerd/zha-toolkit?tab=readme-ov-file#setup) its easy to run the update with its "Trigger Device's Firmware Update" service.

## Zigbee2MQTT

Naturally, it is also supported in Zigbee2MQTT and pairs without issues.

[![Zigbee2MQTT](/assets/images/mho-c401/z2m.jpg)](/assets/images/mho-c401/z2m.jpg)

The smiley display and comfort configuration options are not implemented at the time of writing this guide.

[![Zigbee2MQTT](/assets/images/mho-c401/z2m_exposes.jpg)](/assets/images/mho-c401/z2m_exposes.jpg)

## Final Words

The Zigbee firmware is a nice upgrade if you don't have any BT receivers already but have a developed network. The custom BLE firmware by pvvx has a lot more features and configuration options. Using the Zigbee firmware will reduce battery life due to higher power demands of the protocol. Ultimately it doesn't matter much in the long run since you can always revert back to BLE from Zigbee if you don't like it.