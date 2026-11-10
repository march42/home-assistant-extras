## Heating blueprint for Sonoff TRVZB

This blueprint is designed and developed for the Sonoff TRVZB thermostatic radiator valve.
It should work with the Sonoff TRV-ZBT well, yet i didn't test this.

Starting with version 2.00 the TRV can be selected from domain climate.
This opens the opportunity to control other TRVs as well.
Specific Sonoff TRVZB functionality will most probably not function.

### Functions

    - sets target temperature according to configured settings
      - eco temp for heating period
      - comfort temp for scheduled time of day
      - open window temp, if window opened
      - away temp, if nobody home
    - manual winter mode activation via `input_boolean` helper or `binary_sensor`
    - comfort heating activation via `scheduler` or `binary_sensor`
    - remote window contact sensors for open window detection
    - choose `person`, `device_tracker` or `input_boolean` for somebody at home detection
    - revert to scheduled temp, after target temp was changed manually
    - specific functionality for Sonoff TRVZB
      - sets external temperature from configured remote sensor
      - if target_temp out of range, set min_temp and max_temp if supported
    - manual events
      - HEATING_RESET_TO_SCHEDULER activate scheduled temperature on the TRVs
      - HEATING_SET_ECO override TRVs target temperature to eco temp
      - manual event HEATING_WINTER_START, setting winter mode via turn_on
      - manual event HEATING_WINTER_STOPP, setting winter mode via turn_off

### Setting up

1. ensure your [Home Assiatant](https://home-assistant.io/) instance is compatible and prepared
  - version of Home Assistant "2024.10.0" or newer
  - Sonoff TRVZB firmware version 1.3.0 (0x00001300) or newer for external temperature sensor
  - current version of zigpy or custom quirk for TRVZB installed
    - `zhaquirks/sonoff/trvzb.py` from [zigpy zha-device-handlers](https://github.com/zigpy/zha-device-handlers) commited on 2025-04-30 or later
    - `custom_zha_quirks/trvzb-zigpy.py` from [march42](https://github.com/march42/home-assistant-extras)
2. copy the blueprint to your Home Assistant instance
  - [import to Home Assistant](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fmarch42%2Fhome-assistant-extras%2Fblob%2Fmain%2Fblueprints%2Fautomation%2Fmarch42%2FSONOFF_TRVZB_heating.yaml)
  - default path `config/blueprints/automation/march42/SONOFF_TRVZB_heating.yaml`
3. restart Home Assistant, for the changes to take effect
  - under settings/Tools reload YAML configuration or restart
  - restart your instance
  - reboot your host
4. prepare your configuration
  - If you intend to use multiple temperature sensors for remote temperature sensor coverage,
    create appropriate helper entities.
    - open: Settings
    - choose: Integrations, Devices, Entities, Helpers
    - switch to: Helpers
    - button: Create helper
    - select: **Combine the state of several sensors**
    - name your helper e.g. **heating mean temp living room**
    - choose: input temperature sensors
    - select: Statistic characteristic - **arithmetic mean** see <https://www.home-assistant.io/integrations/statistics>
    - save and activate your arithmetic mean sensor
  - If you intend to use winter mode
    - open: Settings
    - choose: Integrations, Devices, Entities, Helpers
    - switch to: Helpers
    - button: Create helper
    - select: **Toggle**
    - name your helper e.g. **heating winter mode**
    - save helper
  - If you intend to use scheduler for comfort temperature
    - open: Settings
    - choose: Integrations, Devices, Entities, Helpers
    - switch to: Helpers
    - button: Create helper
    - select: **Schedule**
    - name your helper e.g. **heating schedule comfort temp**
    - select: your time spans for comfort temperature e.g. Mon-Sun 08.00-20.00
  - If you intend to use guests at home override
    - open: Settings
    - choose: Devices & Services
    - switch to: Helpers
    - button: Create helper
    - select: **Toggle**
    - name your helper e.g. **heating guests at home**
    - save helper
5. create your automation
  - open: Settings
  - choose: Automations & Scenes
  - button: Create automation
  - select: **Sonoff TRVZB heating**
    - select one or more TRV and configure the corresponding settings
    - save your automation, name it and assign appropriate category and area

### Configuration settings


### Winter mode

When winter mode input is configured
the triggering changes will be ignored if not activated.
The TRVs will be turned on and off, when the input changes.

**Note**:
When winter mode is inactive, the actions will be cut down the a minimum.
This saves battery while heating or actions are unnecessary.

I created a **toggle** helper named **heating winter mode**.
In autumn, when it's getting colder, i switch on the helper winter mode and initiate the heating period.
In spring, when it's getting warmer, i switch off the helper and stop the heating functions.

TODO:
- Calender automation of winter mode,
    switching ON on Oktober 1st and OFF on April 30th.
- Statistical automation of winter mode,
    switching ON when 3 consecutive days outside temp below 10°C
    and switching OFF when 3 consecutive days outside temp not below 20°C.

### Temperature settings

    temperature settings and correlations, i found to be useful
    - open window temp 7.0°C
      - sufficient for frost protection
    - away temp 12-15 °C
      - if nobody at home it doesn't bother if chilly
      - when coming back it can take a few moments longer to heat up to temperature
    - eco temp 15°C
      - over night
      - during the day, when i usually wear thicker clothes
    - comfort temp 18°C-21°C
      - in the evening when i want to relax
      - free days, when i'm having a long brunchy breakfast
    **open window temp < away temp <= eco temp <= comfort temp**

    selection logic
    1. open window temp if one or more windows opened
    2. away temp if persons at home count == 0
    3. comfort temp if comfort scheduler active
    4. eco temp otherwise

### Remote temperature sensor

If `Remote temperature sensor` is configured, it will be watched for temperature changes.
The changed temperature value is written to the TRVs
`external_temperature_sensor_value` value attribute
if the TRVs `external_temperature_sensor` setting is activated.

**Note**:
For the Sonoff TRVZB and TRV-ZBT this two attributes are provided by the device
and declared in the zha quirk.
This feature was introduced with TRVZB firmware v1.3.0.
Attributes were added to the quirk on 2025-04-30.

**Info**:
As long as your TRV provides the attributes mentioned above
this function should work as well.

**TODO**:
For TRV without the remote sensor attributes and with temperature_calibration attribute
we could calculate the current calibration value
from difference between the local and the remote temperature.

### Open window detection

Reliable detection via chosen contact sensor.

On sensor activation (window opened)
the target temperature is set very low
only to prevent damages to the heating system.

On sensor clearance (window closed)
the target temperature is reverted back to scheduled.

### Presence detection

Detection configured persons at home via `person` and `device_tracker`.

The away temperature will be set instead of confort temperature
if none of the configured persons are detected home.

Via `input_boolean` helper you can act on situations where you want to override detection.
You can create a `guests at home` helper, to activate everytime you have guests visiting.

Via `binary_sensor` you can specifying a presence detection sensor
to automate heating on room level.
Choosing a switch may e.g. activate heating in the work room when the power outlet is switched on.

You need to choose the away delay timespan in accordance to your sensors.
Reacting without delay can be okay, when using manually set helper.
Delay can be low, if using person and device_tracker.
More time should be considered, when using presence sensors.

**Note**: If no persons are defined, the script will always set persons_home=-1 so it's not 0 for testing.

### Events consumed

- HEATING_RESET_TO_SCHEDULER
- HEATING_SET_ECO
- HEATING_WINTER_START
- HEATING_WINTER_STOPP

- Winter mode activating and deactivating
- Window opening and closing
- Detecting nobody and somebody at home
- Scheduler activating and deactivating

- Temperature changed on remote sensor
- Target temperature changed on TRV

- Home Assistant starting/stopping

#### Home Assistant system events

- On Home Assistant stopping the target temperature will be set to eco temp.
- On Home Assistant start the target temperature will be reset to scheduled temperature.

### Changelog

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)

#### Work In Progress

##### added

- Home Assistant system events (v2.02-b15)
- input sections structuring configuration settings (v2.01-b14)

#### Version 2.00

##### breaking changes

- selector TRV switched from device_id to entity_id and renamed from trv to input_trv
- selector TRV filtering on domain climate, instead of Sonoff TRVZB and TRV-ZBT

##### added

- input enable_winter_set
- manual event HEATING_WINTER_START, setting winter mode via turn_on
- manual event HEATING_WINTER_STOPP, setting winter mode via turn_off
- input enable_min_max_correct
- if target_temp out of range, set min_temp and max_temp if supported

#### Version 1.00

##### breaking changes

- enumerating scheduled_temp changed
- changed running mode to queued, was parallel
- setting target temperature when winter mode is on or manual event running
- temperature_source_deskew changed to minutes, was seconds
- requirement Home Assistant min_version: "2024.10.0"

##### added

- blueprint conditions added
- selector for TRV, filtering Sonoff TRVZB and TRV-ZBT connected via ZHA
- Revert from manually setting target temperature (wip)
- selector added domain binary_sensor to triggering entities
- handling of temperature changed on remote sensor
- platform event homeassistant_start
- platform event homeassistant_shutdown
- manual event HEATING_SET_ECO
- manual event HEATING_RESET_TO_SCHEDULER

#### pre release

- selector for device TRVZB, filtering Sonoff TRVZB connected via ZHA
- remote temperature sensor
- input winter mode
- input scheduler for comfort temperature
- input min_temp for eco temperature
- input day_temp for comfort temperature
- input window sensor for opening detection
- input windowopened_delay
- input opened_temp for temperature when window opened
- input persons for presence detection
- input away_temp for temperature when nobody home
- input nobodyhome_delay
- input event_delay
- input action_delay
