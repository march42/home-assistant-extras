## OTA updates

[official SONOFF upgrades](https://zigbee-ota.sonoff.tech/releases/upgrade.json)

```json
[
  {
    "fw_binary_url": "https://zigbee-ota.sonoff.tech/releases/86-0001-00001101.zigbee",
    "fw_file_version": 4353,
    "fw_filesize": 131086,
    "fw_image_type": 1,
    "fw_manufacturer_id": 4742,
    "model_id": "ZBMINI-L"
  },
  {
    "fw_binary_url": "https://zigbee-ota.sonoff.tech/releases/snzb-06p_v1.0.5.ota",
    "fw_file_version": 4101,
    "fw_filesize": 258206,
    "fw_image_type": 2060,
    "fw_manufacturer_id": 4742,
    "model_id": "SNZB-06P"
  }
```

### SNZB-TRV Thermostatic Radiator Valve

"profile_id": "0x0104",
"device_type": "0x0301",

- input cluster

ClusterID: 0x0000 (Basic)
ClusterID: 0x0001 (Power Configuration)
ClusterID: 0x0003 (Identify)
ClusterID: 0x0006 (On/Off)
ClusterID: 0x0020 (Poll Control)
ClusterID: 0x0201 (Thermostat)

ClusterID: 0xfc11 (Custom SONOFF Cluster)
Attr: 0x0000, BOOL, Child Lock
Attr: 0x6000, BOOL, Open Window
Attr: 0x6002, INT16 (id:0x29), Frost Protection Temperature, value in 1/100th Celsius (700=7.0°C)
Attr: 0x6003, UINT16 (id:0x21), Idle Steps
Attr: 0x6004, UINT16 (id:0x21), Closing Steps
Attr: 0x6005, UINT16 (id:0x21), Valve Opening Limit Voltage
Attr: 0x6006, UINT16 (id:0x21), Valve Closing Limit Voltage
Attr: 0x6007, UINT16 (id:0x21), Valve Motor Running Voltage
Attr: 0x600B, UINT8 (id:0x20), Valve Opening Degree, value in percent 0..100
Attr: 0x600C, UINT8 (id:0x20), Valve Closing Degree, value in percent 0..100
Attr: 0x600D, INT16 (id:0x29), External Temperature Sensor Value, value in 1/100th Celsius (2000=20.0°C)
Attr: 0x600E, UINT8 (id:0x20), External Temperature Sensor Enable, 0=Off, 1=On
Attr: 0x6011, INT16 (id:0x29), Temperature Control Accuracy
Attr: 0x6014, UINT8 (id:0x20), Temporary Mode, 0x00=Boost Mode, 0x01=Timer Mode
Attr: 0x6015, UINT32, Temporary Mode Duration, value in seconds
Attr: 0x6016, INT16 (id:0x29), Timer Mode Target Temperature, value in 1/100th Celsius (2000=20.0°C)
Attr: 0x6017, BITMAP8, Smart Temperature Control, 0x02=Adaptive Mode

ClusterID: 0xfc57 (Works With All Hubs)

- output cluster

ClusterID: 0x000a (Time)
ClusterID: 0x0019 ()

#### Remote Temperature Sensor


### SNZB-02DR2 Climate Sensor with display

Manufacturer ID: 0x1286
Image type: 0x0810
File version: 0x00001005 (1.0.5)

#### Remote temperature sensor

ieee: "{{ target_ieee | trim }}"
endpoint_id: 1
cluster_id: 0xFC11
cluster_type: in
attribute: 0xFFF1
value: "{{ slot | int - 1 }}"

ieee: "{{ target_ieee | trim }}"
endpoint_id: 1
cluster_id: 0xFC11
cluster_type: in
attribute: 0xFFF3
value: "{{ (temperature_c | float * 100) | round(0) | int }}"

### SNZB-04P Magnetic Contact Sensor

When tamparing

ZCL Report Attribute, Cluster 0xfc11 Manufacturer Specific, Attribute ID 0x2000 unknown, uint8=0x20 0x00=0
ZCL Report Attribute, Cluster 0xfc11 Manufacturer Specific, Attribute ID 0x2000 unknown, uint8=0x20 0x01=1

## Device Types

Category	Models	Description
Smart Switches	BASICZBR3, ZBMINI-L, ZBMINIL2, ZBMINI	Single relay switches
Multi-gang Switches	ZB-SW02, ZB-SW03, ZB-SW04, ZB-SW05	Multiple relay switches
Smart Plugs	S31ZB, S26R2ZB, S40ZBTPB	Power outlets
Contact Sensors	SNZB-04, SNZB-04P	Door/window sensors
Temperature/Humidity Sensors	SNZB-02, SNZB-02D, SNZB-02LD, SNZB-02WD	Climate sensors with various features
Motion Sensors	SNZB-03, SNZB-03P	PIR motion detection
Water Sensors	SNZB-05, SNZB-05P	Water leak detection
Wireless Buttons	SNZB-01, SNZB-01P, SNZB-01-KF	Remote control buttons
Thermostatic Valves	TRVZB	Radiator thermostat control
Curtain Controllers	CK-MG22-JLDJ-01(7015), MYDY25Z-1, MYRX25Z-1	Motor controllers for curtains
Water Valves	GZW01, GZW02	Smart water valve controllers
Routers	NSPanelP-Router, ZBDongle-E	Network extenders
eWeLink Devices	CK-BL702-ROUTER-01, SA-003-Zigbee, WS01	Various eWeLink branded devices

### white labelled

Common white-labeled devices include

SNZB-04 (also sold as Tuya WL-19DWZ)
SNZB-01, SNZB-02, SNZB-03 (sold as eWeLink RHK07, RHK08, RHK09)
