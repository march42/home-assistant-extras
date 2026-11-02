1) Create a new folder named "zha_quirks" in the Home Assistant configuration directory using the File Editor add-on. Skip this step if the folder already exists.
2) Upload the tp_wgzba.py file.
3) Use File Editor to insert the required code into homeassistant/configuration.yaml  zha:   database_path: /config/zigbee.db   enable_quirks: true   custom_quirks_path: /config/zha_quirks/
4) Save the changes and restart Home Assistant.
5) Open the ZHA integration to add the device.
6) Put the Device into Pairing Mode.
  - When powered on for the first time, the device automatically enters pairing mode.
  - The pairing indicator flashes.
  - If the device remains unpaired for 3 minutes after entering pairing mode, it will exit pairing mode automatically.
  - To enter pairing mode again, switch the device to Manual Mode, rotate the knob counterclockwise until the screen displays "OF", then press and hold the center button for 2 seconds.
  - The pairing icon will start flashing.
7) After adding the device, you can start using it.
