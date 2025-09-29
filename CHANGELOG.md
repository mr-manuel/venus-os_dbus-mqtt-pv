# Changelog

## 0.1.10-dev
* Added: Standby power, values below are always shown as zero by @randomname32
* Changed: Fix restart issue

## v0.1.9
⚠️ This version is required for Venus OS v3.60~27 or later, but it is also compatible with older versions.
* Added: paho-mqtt module to driver
* Added: Power factor
* Added: Shelly Gen2+ device support by @jjdejong
* Added: Tasmota device support
* Changed: Broker port missing on reconnect
* Changed: Default device instance is now `100`
* Changed: Fixed service not starting sometimes

## v0.1.8
* Changed: Add VRM ID to MQTT client name
* Changed: Fix registration to dbus https://github.com/victronenergy/velib_python/commit/494f9aef38f46d6cfcddd8b1242336a0a3a79563

## v0.1.7
* Changed: Update `energy_forward` only if it was received
* Changed: Fixed problems when timeout was set to `0`.

## v0.1.6
* Changed: Allow to populate only L2 and/or L3 even if L1 is missing

## v0.1.5
* Added: Timeout on driver startup. Prevents problems, if the MQTT broker is not reachable on driver startup
* Changed: Fixed status flapping between running and standby

## v0.1.4
* Added: Frequency
* Added: StatusCode: 7 = Running; 8 = Standby

## v0.1.3
* Changed: Fix crash when rounding none value
* Changed: Fixed a typo

## v0.1.2
* Added: Frequency
* Added: Show to which broker and port the connection was made when logging is set to INFO
* Added: Try to reconnect every 15 seconds to MQTT broker, if connection is closed abnormally
* Changed: Improved error handling and output

## v0.1.1
* Added: Timeout in order to disconnect the pv inverter, if no new MQTT message is received after x seconds (configurable in `config.ini`)

## v0.1.0
* Added: Device name can be changed in the `config.ini`
* Added: Device instance can be changed in the `config.ini`
* Added: How to create multiple instances in `README.md`
* Changed: Topic variable name in `config.ini`

## v0.0.2
* Added: Set logging level in `config.default.ini`
* Changed: Logging levels of different messages for clearer output
* Changed: Optimized log output for faster troubleshooting

## v0.0.1
Initial release
