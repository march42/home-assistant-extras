## Sonoff SNZB-04P Magnetic Sensor

|:--- |:--- | --- |
| Manufacturer Name | eWeLink | ZCL, Cluster 0x0000 Basic, Attribute ID 0x0004 Manufacturer Name |
| Model Identifier | SNZB-04P | ZCL, CLuster 0x0000 Basic, Attribute ID 0x0005 Model Identifier |
| Software Build ID | 2.2.0 | ZCL, CLuster 0x0000 Basic, Attribute ID 0x4000 Software Build ID |

### During onboarding process, the eWeLink APP configures following.

ZCL Write Attribute, Cluster 0x0500 Intruder Alarm System Zone, Attribute ID 0x0010 IAS_CIE_Address, IEEE address 0xf0 =fa90d130004b1200
00:12:4b:00:30:d1:90:fa is the EUI64 of the Sonoff ZBBride-P

ZCL IAS Zone Enroll Request **FROM device**, Cluster 0x0500 IAS Zone, Command 0x09 Zone Enroll Request, Zone Type 0x0015 Contact Switch, Manufacturer Code 0x1286 Shenzhen CoolKit Technology

ZCL Configure Reporting, Cluster 0x0001 Power Configuration, Command 0x06 Configure Reporting, Attribiute ID 0x0021 Battery Percentage Remaining, Remaining 1.0%, Minimum Interval 0x0654=1620, Maximum Interval 0x06cc=1740

ZCL Read Attributes, Cluster 0x0019 OTA Upgrade, Attribute ID 0x0002 Current File Version, uint32=0x23 0x00002200

ZCL Unknown Command, Cluster 0xfc57 Manufacturer Specific, Manufacturer Code 0x1217 Amazon Lab126, Command 0x04, Data 05 02 1e 00 00 00 ff

ZCL Write Attribute, Cluster 0xfc11 Manufacturer Specific, Attribute ID 0x1fff, uint8=0x20 0x81=129
Response 0x86 Unsupported Attribute, Cluster 0xfc11 Attribute ID 0x1fff

ZCL Read Attribute, Cluster 0x0500 IAS Zone, Attribute ID 0x0002 IAS Zone Status
Response bitmap16=0x19 0x0001=Alarm1

ZCL Read Atribute, Cluster 0x0001 Power Configuration, Attribute ID 0x0021 Battery Percentage Remaining
Response uint8=0x20 0xc8=100.0% ==> 0xc8=200 steps of 0.5% ==> 100%

ZCL Read Attribute, Cluster 0xfc11 Manufacturer Specific, Attribute ID 0x001c 0x0002
Response 0x86 Unsupported Attribute, Cluster 0xfc11 Attribute ID 0x001c 0x0002

### Open/Close/Tampered

When opening or closing

ZCL IAS Zone Status Change Notification, Cluster 0x0500 IAS Zone, Command 0x00 Zone Status Change Notification, Zone ID 0x00, Zone Status 0x0000
.... .... .... ...0 = Alarm 1: Closed or not alarmed
.... .... .... ..0. = Alarm 2: Closed or not alarmed
.... .... .... .0.. = Tamper: Not tampered
.... .... .... 0... = Battery: Battery OK
.... .... ...0 .... = Supervision Reports: Does not report
.... .... ..0. .... = Restore Reports: Does not report restore
.... .... .0.. .... = Trouble: OK
.... .... 0... .... = AC (mains): AC/Mains OK

ZCL IAS Zone Status Change Notification, Cluster 0x0500 IAS Zone, Command 0x00 Zone Status Change Notification, Zone ID 0x00, Zone Status 0x0001
.... .... .... ...1 = Alarm 1: Opened or alarmed

ZCL IAS Zone Status Change Notification, Cluster 0x0500 IAS Zone, Command 0x00 Zone Status Change Notification, Zone ID 0x00, Zone Status 0x0000
.... .... .... ...0 = Alarm 1: Closed or not alarmed

When tamparing

ZCL Report Attribute, Cluster 0xfc11 Manufacturer Specific, Attribute ID 0x2000 unknown, uint8=0x20 0x00=0
ZCL Report Attribute, Cluster 0xfc11 Manufacturer Specific, Attribute ID 0x2000 unknown, uint8=0x20 0x01=1
