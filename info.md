# Teletask Integration for Home Assistant

Control your Teletask home automation system (MICROS+, PICOS, AUTOBUS) directly from Home Assistant via the DoIP protocol.

## Features

- Control **lights** (on/off + dimming) and **switches**
- Supports Teletask types: relay, locmood, genmood, and flag
- Real-time state feedback
- Compatible with Home Assistant 2024.x, 2025.x, and 2026.x

## Requirements

- Teletask "OPEN DOIP PROTOCOL" license (SKU: TDS15132) must be activated on your controller
- Only one DoIP connection at a time is supported — create a dedicated user account for Home Assistant

## Configuration

Add to your `configuration.yaml`:

```yaml
teletask:
  host: 192.168.1.100
  port: 55957

  light:
    - doip_component: relay
      address: "1"
      name: "Living Room"
      unique_id: "teletask_light_living"

  switch:
    - doip_component: relay
      address: "10"
      name: "Carport Socket"
      unique_id: "teletask_switch_carport"
```

See the [full documentation](https://github.com/dsmagghe/homeassistant-teletask) for detailed setup instructions and troubleshooting.
