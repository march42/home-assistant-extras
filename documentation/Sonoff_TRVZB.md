
### Sonoff TRVZB

- thermostatic radiator valve
- ZigBee communication

```json
{
  "node_descriptor": {
    "logical_type": 2,
    "complex_descriptor_available": 0,
    "user_descriptor_available": 0,
    "reserved": 0,
    "aps_flags": 0,
    "frequency_band": 8,
    "mac_capability_flags": 128,
    "manufacturer_code": 4742,
    "maximum_buffer_size": 82,
    "maximum_incoming_transfer_size": 255,
    "server_mask": 11264,
    "maximum_outgoing_transfer_size": 255,
    "descriptor_capability_field": 0
  },
  "endpoints": {
    "1": {
      "profile_id": "0x0104",
      "device_type": "0x0301",
      "input_clusters": [
        "0x0000",
        "0x0001",
        "0x0003",
        "0x0006",
        "0x0020",
        "0x0201",
        "0xfc11",
        "0xfc57"
      ],
      "output_clusters": [
        "0x000a",
        "0x0019"
      ]
    }
  },
  "manufacturer": "SONOFF",
  "model": "TRVZB",
  "class": "trvzb:(SONOFF / TRVZB)"
}
```

#### Cluster and Attributes

SONOFF_MANUFACTURER_SPECIFIC_CLUSTER_ID = 0xFC11
WWAH_MANUFACTURER_SPECIFIC_CLUSTER_ID = 0xFC57

- i named the constants, because i find it easier to read and change later

| Cluster | Attribute | Name | Notes |
| --- | --- | --- | --- |
| 0x0000 | - | Basic Cluster |  |
| 0x0001 | - | Power Configuration Cluster |  |
| 0x0001 | 0x0021 | Battery Percentage Remaining Attribute | in 0.5%, 0xC8=200=100% |
| 0x0003 | - | Identify Cluster |  |
| 0x0006 | - | On/Off Cluster |  |
| 0x0006 | 0x0000 | On/Off Cluster, Attribute OnOff |  |
| 0x000a | - | Time Cluster |  |
| 0x0019 | - | OTA Cluster |  |
| 0x0020 | - |  |  |
| 0x0201 | - | Thermostat Cluster |  |
| 0x0201 | 0x0000 | Local Temperature Attribute | in 1/100th degree celsius (2140=21.40°C) |
| 0x0201 | 0x0012 | Occupied Heating Set Point Attribute | in 1/100th degree celsius (3500=35.00°C) |
| 0x0201 | 0x001c | System Mode Attribute | enum8 0x00=Off (Anti Freeze), 0x01=Auto (scheduler activated), 0x04=Manual or Boost or Scheduled Override Active |
| 0x0201 | 0x0010 | Local Tempreature Calibration Attribute | int8 in 1/10th degree celsius (-2=-0.2°C) |
| 0x0201 | 0x0029 | Thermostat Running State Attribute | bitmap16 0x01=Heating |
| 0xFC11 | - | SONOFF_MANUFACTURER_SPECIFIC_CLUSTER_ID | SONOFF_MANUFACTURER_CODE 0x1217 |
| 0xFC11 | 0x6013 | SONOFF_ADAPTIVE_MODE_ATTR_ID | enum8 0x00=Inactive (temperature accuracy configurable), 0x02=Active (Valve opening based on target temperature and temperature changes) |
| 0xFC11 | 0x0000 | SONOFF_CHILD_LOCK_ATTR_ID | 0x00=False (TRV can be operated), 0x01=True (TRV operation blocked) |
| 0xFC11 | 0x0021 | SONOFF_DISPLAY_ORIENTATION_ATTR_ID | uint8 0x00=0°, 0x02=180° |
| 0xFC11 | 0x6000 | SONOFF_OPEN_WINDOW_DETECTION_ATTR_ID | 0x00=False, 0x01=True (temperature drop of >1.5°C in <4.5 minutes, target temperature 12°C) |
| 0xFC11 | 0x6002 | SONOFF_ANTI_FREEZE_TEMPERATURE_ATTR_ID | in 1/100th degree celsius (700=7.0°C) |
| 0xFC11 | 0x6003 | SONOFF_IDLE_STEPS_ATTR_ID | uint16 |
| 0xFC11 | 0x6004 | SONOFF_CLOSING_STEPS_ATTR_ID | uint16 |
| 0xFC11 | 0x6005 | SONOFF_OPENING_LIMIT_VOLTAGE_ATTR_ID | uint16 |
| 0xFC11 | 0x6006 | SONOFF_CLOSING_LIMIT_VOLTAGE_ATTR_ID | uint16 |
| 0xFC11 | 0x6007 | SONOFF_VALVE_RUNNING_VOLTAGE_ATTR_ID | int16 |
| 0xFC11 | 0x6008 | ??? Attribute | uint8, reported 0x7f=127 after saving scheduler |
| 0xFC11 | 0x6009 | Regular Heating Setpoint Attribute | int16 in 1/100th degree celsius 1500=15.0°C |
| 0xFC11 | 0x600B | SONOFF_VALVE_OPENING_PERCENTAGE_ATTR_ID | in percent (0x64=100=100%) |
| 0xFC11 | 0x600C | SONOFF_VALVE_CLOSING_PERCENTAGE_ATTR_ID | in percent (0x64=100=100%) |
| 0xFC11 | 0x600D | SONOFF_EXTERNAL_TEMP_SENSOR_VALUE_ATTR_ID | int16, in 1/100th degree celsius |
| 0xFC11 | 0x600E | SONOFF_EXTERNAL_TEMP_SENSOR_ENABLE_ATTR_ID | uint8, 0x00=disabled, 0x01=enabled |
| 0xFC11 | 0x6011 | SONOFF_TEMPERATURE_ACCURACY_ADJUST_ATTR_ID | doesn't want to write with firmware 1.4.4, not allowed if Adaptive Mode is activated (?=-1.0°C, ?=-0.8°C, ?=-0.6°C, ?=-0.4°C, ?=-0.2°C) |
| 0xFC11 | 0x6014 | SONOFF_TEMPORARY_ACTIVATION_MODE_ATTR_ID | 0x00=Boost (set 0x6015 activation time first), 0x01=Scheduled (set 0x6015 activation time and 0x6016 temperature first) |
| 0xFC11 | 0x6015 | SONOFF_TEMPORARY_ACTIVATION_TIME_ATTR_ID | in seconds (900=15 minutes) |
| 0xFC11 | 0x6016 | SONOFF_TEMPORARY_TARGET_TEMPERATURE_ATTR_ID | in 1/100th degree celsius (500=5.0°C) |
| 0xFC57 | - | WWAH (Works With All Hubs) | WWAH_MANUFACTURER_CODE 0x1217 Amazon Lab126 |

#### Factory Reset

1. remove the batteries, at least one of them
2. press and hold the knob button
3. insert the batteries, while holding the knob pressed
4. wait until the display flashes and shows **in**
5. release the knob button

#### ZigBee pairing

1. the TRVZB must be installed and working, showing target temperature
2. turn off the TRVZB, by rotating left until **OF** is displayed
3. press knob button for about 5 seconds, until **--** flashes
4. start pairing on your ZigBee coordinator

#### Special Functionality

##### Adaptive Mode, Smart Temperature Control

- [added in firmware 1.4.4](https://forum.ewelink.cc/t/trvzb-firmware-changelog/74377)
- valve opening is dynamically adjust in accordance to difference between actual and target temperature (PID algorythm)
- with Adaptive Mode enabled, Valve Opening Percentage and Temperature Accuracy features will be disabled

##### External Temperature Sensor

- [added in firmware 1.2.1](https://forum.ewelink.cc/t/trvzb-firmware-changelog/74377)
* use temperature value of external sensor instead of integrated one
* only if attribute SONOFF_EXTERNAL_TEMP_SENSOR_ENABLE_ATTR_ID is set to true
* external temperature needs to be written to attribute SONOFF_EXTERNAL_TEMP_SENSOR_VALUE_ATTR_ID
* external temperature value gets invalid (and ignored) after 120 minutes

##### Temporary Schedule

- [added in firmware 1.4.0](https://forum.ewelink.cc/t/trvzb-firmware-changelog/74377)
- override target temperature to SONOFF_TEMPORARY_TARGET_TEMPERATURE_ATTR_ID
- for SONOFF_TEMPORARY_ACTIVATION_TIME_ATTR_ID duration
- attribute SONOFF_REGULAR_HEATING_SETPOINT_ATTR_ID holds the regularly set target temperature

##### Temporary Boost

- [added in firmware 1.4.0](https://forum.ewelink.cc/t/trvzb-firmware-changelog/74377)
- sets target temperature to maximum
- for SONOFF_TEMPORARY_ACTIVATION_TIME_ATTR_ID duration, configurable between 1 minute and 2 hours and 59 minutes
- attribute SONOFF_REGULAR_HEATING_SETPOINT_ATTR_ID holds the regularly set target temperature

##### Temperature Offset

- [added in firmware 1.4.0](https://forum.ewelink.cc/t/trvzb-firmware-changelog/74377)
- adjust local temperature by +-0.2°C values for calibration

##### Display Orientation

- can be adjusted by opening battery case and pressing the configuration button for 3 seconds
- [added in firmware 1.4.0](https://forum.ewelink.cc/t/trvzb-firmware-changelog/74377)
- change display orientation
- Rotate 0° named Horizontal, for TRVZB mounted horizontally on raditor valve
- Rotate 180° named Vertical, for TRVZB mounted vertically on raditor valve

##### Adjust Accuracy

- [added in firmware 1.3.0](https://forum.ewelink.cc/t/trvzb-firmware-changelog/74377)
- adjust the heating start of TRVZB
- default value is -1.0°C, possible other values -0.8°c -0.6°c -0.4°c -0.2°c
- sets offset from target temperature, when valve is opened
- disabled, if Adaptive Mode enabled

##### Open Window Detection

- sets target temperature to 12°C, if temperature drop of >1.5°C in <4.5 minutes
- reverts back to normal after 30 minutes or after manually changing temperature

##### Anti Calcification

- Every Monday at 12:00 the TRVZB operates the valve, to prevent calcification
- display will show **AC** during process

##### Child Lock

- activate and deactivate by pressing the knob for 3 seconds
- display will show **CL** if activated
