# Summary

This integration allows displaying and controlling [Sensi](https://sensi.emerson.com/en-us) thermostat.

It was developed by reverse engineering the mobile app and work done by https://github.com/w1ll1am23/pysensi, so the integration could fail at some point.

On adding the Sensi integration, you should see one device and up to 8 entities. You will need credentials used in the Sensi mobile app.

![image](https://github.com/iprak/sensi/assets/6459774/222a21ac-8d5f-4530-b3d6-ec87ae668b6d)


- Only single target temperature is supported; temperature range is not supported. You will have to set heat/cool mode yourself.
- The available operating modes `Auto/Heat/Cool/Off` are based on thermostat setup.
- Supported fan modes are: Auto, On and Circulate (10% duty cycle). Not all Sensi thermostats support circulation mode and the option will be unavailable in that case.
  - Fan support can be disabled, in which case `fan modes` will not be available.
- Data is refreshed every 30 seconds.
- Some Thermostat display properties such as Display Humidity, Display Time and Continuous Backlight can also be controlled. Not all thermostats support Continuous Backlight feature and the option will be unavailable in that case.
- The `Fan Support` configuration can be used to disable fan.
- If the thermostat is `Offline`, the entities will appear unavailable.
- Idle state
  - The target temperature from the previous action will be displayed.
- Auxiliary heating
  - Homeassistant currently doesn't handle aux heat case well. One will "Heating" as the action for aux heating.
- Min/Max setpoints
  - Homeassistant supports on one min and one max setpoint. The properties for those are cached and do not account for the hvac action. You can see the setpoint values under Diagnostic section of the device.

Sample attributes on the climate entity:

```
hvac_modes: off, heat, cool, auto
min_temp: 45
max_temp: 99
fan_modes: auto, on, Circulate
current_temperature: 70
temperature: 76
current_humidity: 99
fan_mode: Circulate
offline: false
wifi_connection_quality: 66
battery_voltage: 2.981
hvac_action: null
circulating_fan: on
circulating_fan_cuty_cycle: 10
attribution: Data provided by Sensi
friendly_name: Living Room
supported_features: 9
```

# Issues

So far, simultaneous logins from mobile app and integration have not been problematic. But it has been noticed that sometimes changing thermostat properties does not take effect, this could be either due to something going on at Sensi backend or the thermostat temporarily going offline.

# Installation

- Download and copy all the files from `custom_components/sensi/` to `<config directory>/custom_components/sensi/`.
- Restart HomeAssistant.
- Create an account on the Sensi mobile app.
- Add the integration using the `Add Integration` button in Integrations page on your Home Assistant instance.

# Configuration

None

# Breaking Changes

## Revision 1.2.0

The entity/unique ids have been correct. Unfortunately, this will cause the previous entities to appear duplicate/disabled. You would want to remove the previous entities and reference the new ones. The new entity_id is based on the name given to the thermostat and not the device_id which is more accessible.

## Revision 1.1.1

The battery level is now computed based on a formula. It is not perfect but should give some idea of the battery state. The battery voltage itself is now available as an attribute. You will see a warning like The unit of `sensor.sensi_36_6f_92_ff_fe_02_24_b7_battery (%) cannot be converted to the unit of previously compiled statistics (V).`

## Revision 1.1.0

The entity ids have changed to support multiple thermostats on the same account. Your previous entities would appear duplicate/disabled. You would want to remove the integration and add it back.
