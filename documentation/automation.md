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

### trigger state

```yaml
trigger: state
entity_id: binary_sensor.front_door
not_to:
  - "unknown"
  - "unavailable"
```
