## Automation

### how-to

#### throttle triggering

1. create a variable
2. test for time since last trigger
3. use time since last trigger in condition

```yaml
variables:
  seconds_since_last_triggered: > # calculate last_triggered offset
    {% set ts_last_triggered = as_timestamp(this.attributes.last_triggered | default(0)) %}
    {{ as_timestamp(now()) - ts_last_triggered }}
```

```yaml
conditions:
  - condition: template
    value_template: > # only run, if more time since last triggering
      {{ seconds_since_last_triggered >= 300 }}
```

combined

```yaml
conditions:
  - condition: template
    value_template: >
      {% set ts_last_triggered = as_timestamp(this.attributes.last_triggered | default(0)) %}
      {% set seconds_since_ts = as_timestamp(now()) - ts_last_triggered %}
      {{ seconds_since_ts >= 60 }}
    enabled: false
```

#### trigger entity_id

When triggered, trigger.entity_id shows which entity triggered.
Even configured trigger on device_id shows entity_id.

```yaml
trigger.entity_id: sensor.wohnzimmer_klimasensor_temperature
trigger.entity_id: sensor.aussentemperatur_dew_point
trigger.entity_id: sensor.wohnzimmer_klima_anzeige_temperature
```

Using variables, the triggering entity and correspondong device can be determined.

```yaml
variables:
  source_entity: '{{ trigger.entity_id }}'
  source_device: '{{ device_id(source_entity) }}'
```

#### trigger state

```yaml
trigger: state
entity_id: binary_sensor.front_door
not_to:
  - "unknown"
  - "unavailable"
```

#### checking and counting input states

```yaml
variables:
  is_winter: > # winter_mode unset or any is on
    {{ winter_mode is none or expand(winter_mode) | selectattr('state', 'in', ['on', 'active']) | list | count > 0 }}
```

#### ZigBee via ZHA

```yaml
action: zha.set_zigbee_cluster_attribute
data:
  ieee: "{{ device_attr('light.room3', 'identifiers') | selectattr(0,'eq','zha') | map(attribute=1) | first }}"
  endpoint_id: 1
  cluster_id: 0xFC11
  attribute: 0x600D
  value: "{{ temperature_source_value * 100 | round(1) }}"
enabled: false
```

Jinja function call `device_attr("sensor.aussentemperatur_dew_point","identifiers")` returns `{('zha', 'bc:93:2a:ff:fe:7b:e1:d9')}`

`ieee: "{{ device_attr('device_id or entity_id', 'identifiers') | selectattr(0,'eq','zha') | map(attribute=1) | first }}"`
checks for a map where **first attribute eq zha** and filters second attribute returning the first occurance
which is most definitely the only occurance

#### Automation disabled after Home Asstistant startup

```yaml
automation:
  - alias: "Automation disabled after Home Asstistant startup"
    initial_state: false # disabled after Home Asstistant startup
```

## blueprint

### input selector

You can make a selector optional with declaring an empty default.
However, this default needs to reflect the selectors return.
For selector with attribute **multiple==true an empty list** `default: []` needs to be declared
otherwise an empty entity `default:` is correct.
If you define an selector with multiple==true and empty default the selector will not show up in the configuration.

```yaml
  input:
    trv: # choose TRV
      name: Radiator TRV
      description: **NO default**, value is mandatory, selector will show
      selector:
        device:
          multiple: true
          entity:
            - domain: climate
    window_sensor: # contact sensor on windows or doors
      name: (Optional) Window sensor
      description: **empty default**, value is optional, **selector will NOT show**
      selector:
        entity:
          multiple: true
          filter:
            - domain:
              - binary_sensor
      default: # if multiple==true an empty list would be necessary
    persons:
      name: (optional) Persons for presence detection
      description: **empty list default**, value is optional, selector will show
      selector:
        entity:
          multiple: true
          filter:
            - domain:
              - person
              - device_tracker
              - input_boolean
      default: [] # if multiple==true an empty list is necessary
```

Make sure you check for **none** value in the script.

## action

### write ZigBee cluster attribute

[ZigBee write attribute](https://www.home-assistant.io/actions/zha.set_zigbee_cluster_attribute/)

```yaml
```
