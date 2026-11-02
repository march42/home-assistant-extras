1) Create a new folder named "zha_quirks" in the Home Assistant configuration directory using the File Editor add-on. Skip this step if the folder already exists.
2) Upload the TRV-ZBT.py file.(Please check the email attachment)
3) Use File Editor to insert the required code into homeassistant/configuration.yaml  zha:   database_path: /config/zigbee.db   enable_quirks: true   custom_quirks_path: /config/zha_quirks/
4) Save the changes and restart Home Assistant.
5) Open the ZHA integration to add the device.
6) Put the Device into Pairing Mode.
  - In Manual mode, rotate the knob anticlockwise to lower the set temperature until “OFF” appears on the screen.
  - Press and hold the centre button for 2 seconds until the pairing icon flashes.
  - The device will then enter pairing mode.
  - When the "clock" icon is displayed, the device is in Automatic mode.
  - When the "hand" icon is displayed, the device is in Manual mode.
  - Briefly press the centre button to switch between the two modes.
7) After adding the device, you can start using it.
