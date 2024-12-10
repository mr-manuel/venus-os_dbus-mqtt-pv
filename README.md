# dbus-mqtt-pv - Emulates a physical PV Inverter from MQTT data

<small>GitHub repository: [mr-manuel/venus-os_dbus-mqtt-pv](https://github.com/mr-manuel/venus-os_dbus-mqtt-pv)</small>

## Index

1. [Disclaimer](#disclaimer)
1. [Supporting/Sponsoring this project](#supportingsponsoring-this-project)
1. [Purpose](#purpose)
1. [Config](#config)
1. [JSON structure](#json-structure)
1. [Home Assistant](#home-assistant)
1. [Install / Update](#install--update)
1. [Uninstall](#uninstall)
1. [Restart](#restart)
1. [Debugging](#debugging)
1. [Compatibility](#compatibility)
1. [Screenshots](#screenshots)


## Disclaimer

I wrote this script for myself. I'm not responsible, if you damage something using my script.


## Supporting/Sponsoring this project

You like the project and you want to support me?

[<img src="https://github.md0.eu/uploads/donate-button.svg" height="50">](https://www.paypal.com/donate/?hosted_button_id=3NEVZBDM5KABW)


## Purpose

The script emulates a Photovoltaic AC Inverter in Venus OS. It gets the MQTT data from a subscribed topic and publishes the information on the dbus as the service `com.victronenergy.pvinverter.mqtt_pv` with the VRM instance `51`.


## Config

Copy or rename the `config.sample.ini` to `config.ini` in the `dbus-mqtt-pv` folder and change it as you need it.


## JSON structure

### Generic version

<details><summary>Minimum required</summary>

```json
{
    "pv": {
        "power": 0.0
    }
}
```
</details>

<details><summary>Minimum required with L1</summary>

```json
{
    "pv": {
        "power": 0.0,
        "L1": {
            "power": 0.0
        }
    }
}
```
</details>

<details><summary>Minimum required with L1, L2</summary>

```json
{
    "pv": {
        "power": 0.0,
        "L1": {
            "power": 0.0
        },
        "L2": {
            "power": 0.0
        }
    }
}
```
</details>

<details><summary>Minimum required with L1, L2, L3</summary>

```json
{
    "pv": {
        "power": 0.0,
        "L1": {
            "power": 0.0
        },
        "L2": {
            "power": 0.0
        },
        "L3": {
            "power": 0.0
        }
    }
}
```
</details>

<details><summary>Full</summary>

```json
{
    "pv": {
        "power": 0.0,
        "voltage": 0.0,
        "current": 0.0,
        "energy_forward": 0.0,           --> Total/Lifetime produced energy in kWh
        "L1": {
            "power": 0.0,
            "voltage": 0.0,
            "current": 0.0,
            "frequency": 0.0,
            "energy_forward": 0.0       --> Total/Lifetime produced energy in kWh
        },
        "L2": {
            "power": 0.0,
            "voltage": 0.0,
            "current": 0.0,
            "frequency": 0.0,
            "energy_forward": 0.0       --> Total/Lifetime produced energy in kWh
        },
        "L3": {
            "power": 0.0,
            "voltage": 0.0,
            "current": 0.0,
            "frequency": 0.0,
            "energy_forward": 0.0       --> Total/Lifetime produced energy in kWh
        }
    }
}
```
</details>

### Shelly version (Gen 2+)

```json
 {
    "apower": 0.0, 
    "voltage": ..., 
    "current": ..., 
    "freq": ..., 
    "aenergy": { 
        "total": ... 
    }
}
```
In the `config.ini` file, set `shelly = true`, and set your shelly device to use the same topic as the one configured in `config.ini`. Don't forget to enable MQTT on your Shelly device and correctly set the host, port and topic.

## Home Assistant

This is only a simple example that can be reduced expanded to match the minimum or full requirements shown above.

```yml
alias: mqtt publish sensor pv power
description: ""
trigger:
  - platform: state
    entity_id: sensor.YOUR_PV_POWER_ENTITY
condition: []
action:
  - service: mqtt.publish
    data_template:
      payload: |
        {
          "pv": {
            "power": {{ (states('sensor.YOUR_PV_POWER_ENTITY') | float(0)) }},
            "L1": {
                "power": {{ (states('sensor.YOUR_PV_L1_POWER_ENTITY') | float(0)) }}
            },
            "L2": {
                "power": {{ (states('sensor.YOUR_PV_L2_POWER_ENTITY') | float(0)) }}
            },
            "L3": {
                "power": {{ (states('sensor.YOUR_PV_L3_POWER_ENTITY') | float(0)) }}
            }
          }
        }
      topic: homeassistant/energy/pv
```

In the `config.ini` of `dbus-mqtt-pv` set the MQTT broker to the Home Assistant hostname/IP and the topic to the same as in your Home Assistant config (like above).


## Install / Update

1. Login to your Venus OS device via SSH. See [Venus OS:Root Access](https://www.victronenergy.com/live/ccgx:root_access#root_access) for more details.

2. Execute this commands to download and copy the files:

    ```bash
    wget -O /tmp/download_dbus-mqtt-pv.sh https://raw.githubusercontent.com/mr-manuel/venus-os_dbus-mqtt-pv/master/download.sh

    bash /tmp/download_dbus-mqtt-pv.sh
    ```

3. Select the version you want to install.

4. Press enter for a single instance. For multiple instances, enter a number and press enter.

    Example:

    - Pressing enter or entering `1` will install the driver to `/data/etc/dbus-mqtt-pv`.
    - Entering `2` will install the driver to `/data/etc/dbus-mqtt-pv-2`.

### Extra steps for your first installation

5. Edit the config file to fit your needs. The correct command for your installation is shown after the installation.

    - If you pressed enter or entered `1` during installation:
    ```bash
    nano /data/etc/dbus-mqtt-pv/config.ini
    ```

    - If you entered `2` during installation:
    ```bash
    nano /data/etc/dbus-mqtt-pv-2/config.ini
    ```

6. Install the driver as a service. The correct command for your installation is shown after the installation.

    - If you pressed enter or entered `1` during installation:
    ```bash
    bash /data/etc/dbus-mqtt-pv/install.sh
    ```

    - If you entered `2` during installation:
    ```bash
    bash /data/etc/dbus-mqtt-pv-2/install.sh
    ```

    The daemon-tools should start this service automatically within seconds.

## Uninstall

⚠️ If you have multiple instances, ensure you choose the correct one. For example:

- To uninstall the default instance:
    ```bash
    bash /data/etc/dbus-mqtt-pv/uninstall.sh
    ```

- To uninstall the second instance:
    ```bash
    bash /data/etc/dbus-mqtt-pv-2/uninstall.sh
    ```

## Restart

⚠️ If you have multiple instances, ensure you choose the correct one. For example:

- To restart the default instance:
    ```bash
    bash /data/etc/dbus-mqtt-pv/restart.sh
    ```

- To restart the second instance:
    ```bash
    bash /data/etc/dbus-mqtt-pv-2/restart.sh
    ```

## Debugging

⚠️ If you have multiple instances, ensure you choose the correct one.

The logs can be checked with `tail -n 100 -f /data/log/dbus-mqtt-pv/current | tai64nlocal`

The service status can be checked with svstat `svstat /service/dbus-mqtt-pv`

This will output somethink like `/service/dbus-mqtt-pv: up (pid 5845) 185 seconds`

If the seconds are under 5 then the service crashes and gets restarted all the time. If you do not see anything in the logs you can increase the log level in `/data/etc/dbus-mqtt-pv/dbus-mqtt-pv.py` by changing `level=logging.WARNING` to `level=logging.INFO` or `level=logging.DEBUG`

If the script stops with the message `dbus.exceptions.NameExistsException: Bus name already exists: com.victronenergy.pvinverter.mqtt_pv"` it means that the service is still running or another service is using that bus name.

## Compatibility

This software supports the latest three stable versions of Venus OS. It may also work on older versions, but this is not guaranteed.

## Screenshots

<details><summary>Power and/or L1</summary>

![Pv power L1 - pages](/screenshots/pv_power_L1_pages.png)
![Pv power L1 - device list](/screenshots/pv_power_L1_device-list.png)
![Pv power L1 - device list - mqtt pv 1](/screenshots/pv_power_L1_device-list_mqtt-pv-1.png)
![Pv power L1 - device list - mqtt pv 2](/screenshots/pv_power_L1_device-list_mqtt-pv-2.png)

</details>

<details><summary>Power, L1 and L2</summary>

![Pv power L1, L2 - pages](/screenshots/pv_power_L2_L1_pages.png)
![Pv power L1, L2 - device list](/screenshots/pv_power_L2_L1_device-list.png)
![Pv power L1, L2 - device list - mqtt pv 1](/screenshots/pv_power_L2_L1_device-list_mqtt-pv-1.png)
![Pv power L1, L2 - device list - mqtt pv 2](/screenshots/pv_power_L2_L1_device-list_mqtt-pv-2.png)

</details>

<details><summary>Power, L1, L2 and L3</summary>

![Pv power L1, L2, L3 - pages](/screenshots/pv_power_L3_L2_L1_pages.png)
![Pv power L1, L2, L3 - device list](/screenshots/pv_power_L3_L2_L1_device-list.png)
![Pv power L1, L2, L3 - device list - mqtt pv 1](/screenshots/pv_power_L3_L2_L1_device-list_mqtt-pv-1.png)
![Pv power L1, L2, L3 - device list - mqtt pv 2](/screenshots/pv_power_L3_L2_L1_device-list_mqtt-pv-2.png)

</details>
