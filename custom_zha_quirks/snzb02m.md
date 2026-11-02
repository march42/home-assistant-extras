1) Create a new folder named zha_quirks in the Home Assistant directory (this tutorial uses the File Editor add-on).
2) Upload the Hydro series.py file.(Please check the email attachment)
3) Use File Editor to insert the required code into homeassistant/configuration.yaml  zha:   database_path: /config/zigbee.db   enable_quirks: true   custom_quirks_path: /config/zha_quirks/
4) Save the changes and restart Home Assistant.
5) Open the ZHA integration to add the device.
6) Take out the battery insulation sheet to power on the device.
  - The device will automatically enter pairing mode for 3 minutes.
  - If pairing times out, press and hold the reset button for 5 seconds to re-enter pairing mode.
7) After adding the device, you can set up automations.
