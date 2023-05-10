# dbus-mqtt-pv - Emulates a physical PV Inverter from MQTT data

<small>GitHub repository: [mr-manuel/venus-os_dbus-mqtt-pv](https://github.com/mr-manuel/venus-os_dbus-mqtt-pv)</small>

### Disclaimer

I wrote this script for myself. I'm not responsible, if you damage something using my script.


### Purpose

The script emulates a Photovoltaic AC Inverter in Venus OS. It gets the MQTT data from a subscribed topic and publishes the information on the dbus as the service `com.victronenergy.pvinverter.mqtt_pv` with the VRM instance `51`.


### Config

Copy or rename the `config.sample.ini` to `config.ini` in the `dbus-mqtt-pv` folder and change it as you need it.


### JSON structure

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
        "energy_forward": 0.0,
        "L1": {
            "power": 0.0,
            "voltage": 0.0,
            "current": 0.0,
            "energy_forward": 0.0,
        },
        "L2": {
            "power": 0.0,
            "voltage": 0.0,
            "current": 0.0,
            "energy_forward": 0.0,
        },
        "L3": {
            "power": 0.0,
            "voltage": 0.0,
            "current": 0.0,
            "energy_forward": 0.0,
        }
    }
}
```
</details>


### Install

1. Copy the `dbus-mqtt-pv` folder to `/data/etc` on your Venus OS device

2. Run `bash /data/etc/dbus-mqtt-pv/install.sh` as root

   The daemon-tools should start this service automatically within seconds.

### Uninstall

Run `/data/etc/dbus-mqtt-pv/uninstall.sh`

### Restart

Run `/data/etc/dbus-mqtt-pv/restart.sh`

### Debugging

The logs can be checked with `tail -n 100 -f /data/log/dbus-mqtt-pv/current | tai64nlocal`

The service status can be checked with svstat `svstat /service/dbus-mqtt-pv`

This will output somethink like `/service/dbus-mqtt-pv: up (pid 5845) 185 seconds`

If the seconds are under 5 then the service crashes and gets restarted all the time. If you do not see anything in the logs you can increase the log level in `/data/etc/dbus-mqtt-pv/dbus-mqtt-pv.py` by changing `level=logging.WARNING` to `level=logging.INFO` or `level=logging.DEBUG`

If the script stops with the message `dbus.exceptions.NameExistsException: Bus name already exists: com.victronenergy.pvinverter.mqtt_pv"` it means that the service is still running or another service is using that bus name.

### Multiple instances

It's possible to have multiple instances, but it's not automated. Follow these steps to achieve this:

1. Save the new name to a variable `driverclone=dbus-mqtt-pv-2`

2. Copy current folder `cp -r /data/etc/dbus-mqtt-pv/ /data/etc/$driverclone/`

3. Rename the main script `mv /data/etc/$driverclone/dbus-mqtt-pv.py /data/etc/$driverclone/$driverclone.py`

4. Fix the script references for service and log
    ```
    sed -i 's:dbus-mqtt-pv:'$driverclone':g' /data/etc/$driverclone/service/run
    sed -i 's:dbus-mqtt-pv:'$driverclone':g' /data/etc/$driverclone/service/log/run
    ```

5. Change the `device_name` and increase the `device_instance` in the `config.ini`

Now you can install and run the cloned driver. Should you need another instance just increase the number in step 1 and repeat all steps.

### Compatibility

It was tested on Venus OS Large `v2.92` on the following devices:

* RaspberryPi 4b
* MultiPlus II (GX Version)

### Screenshots

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


## Supporting/Sponsoring this project

You like the project and you want to support me?

[<img src="https://github.md0.eu/uploads/donate-button.svg" height="50">](https://www.paypal.com/donate/?hosted_button_id=3NEVZBDM5KABW)
