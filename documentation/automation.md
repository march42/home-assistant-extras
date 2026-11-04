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

### trigger entity_id

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

### trigger state

```yaml
trigger: state
entity_id: binary_sensor.front_door
not_to:
  - "unknown"
  - "unavailable"
```

### checking and counting input states

```yaml
variables:
  is_winter: > # winter_mode unset or any is on
    {{ winter_mode is none or expand(winter_mode) | selectattr('state', 'in', ['on', 'active']) | list | count > 0 }}
```

### ZigBee via ZHA

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
