# Teletask Integration for Home Assistant

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-orange.svg)](https://github.com/hacs/integration)

Custom integration to control [Teletask](https://www.teletask.be/) home automation systems via the DoIP (Distributed over IP) protocol in [Home Assistant](https://www.home-assistant.io/).

This is an actively maintained fork of the original [Tiemooowh/homeassistant-teletask](https://github.com/Tiemooowh/homeassistant-teletask) repository, which has been archived. This fork is updated for compatibility with **Home Assistant 2024.x, 2025.x and 2026.x**.

## Features

- Control Teletask lights (on/off + dimming) and switches from Home Assistant
- Supports Teletask component types: **relay**, **locmood**, **genmood**, and **flag**
- Real-time state feedback via the DoIP protocol
- Full integration with Home Assistant automations, scenes, and dashboards

## Requirements

- A Teletask central unit (e.g. MICROS+, PICOS, AUTOBUS) with network connectivity
- The **Teletask "OPEN DOIP PROTOCOL" license** must be activated on your controller (SKU: **TDS15132**). Contact your Teletask installer or [Teletask support](https://www.teletask.be/) to activate this.
- The IP address and port (default: 55957) of your Teletask central unit

> **Important:** You can only have **one active DoIP connection** to your Teletask central unit at a time. If Home Assistant is connected, you cannot simultaneously use the Teletask PROSOFT or web interface with the same user account. **It is strongly recommended to create a dedicated user account** on your Teletask system for Home Assistant.

## Installation

### Via HACS (recommended)

Since the original repository was removed from the HACS default store, you need to add this as a **custom repository**:

1. Make sure [HACS](https://hacs.xyz/) is installed.
2. In Home Assistant, go to **HACS** > click the three-dot menu (top right) > **Custom repositories**.
3. Add this repository URL: `https://github.com/dsmagghe/homeassistant-teletask`
4. Select category: **Integration**
5. Click **Add**.
6. Search for **Teletask** in HACS and install it.
7. **Restart Home Assistant.**

### Manual installation

1. Download the [latest release](https://github.com/dsmagghe/homeassistant-teletask/archive/refs/heads/main.zip) and unzip it.
2. Copy the `custom_components/teletask` folder into your Home Assistant `config/custom_components/` directory.
3. **Restart Home Assistant.**

## Configuration

This integration is configured via YAML. Add the following to your `configuration.yaml` (or a separate file like `teletask.yaml` using `!include`).

### Basic example

```yaml
teletask:
  host: 192.168.1.100
  port: 55957

  light:
    - doip_component: relay
      address: "1"
      name: "Living Room Light"
      unique_id: "teletask_light_living"

    - doip_component: relay
      address: "31"
      brightness_address: "1"
      name: "Bedroom LED Strip"
      unique_id: "teletask_light_bedroom_led"

  switch:
    - doip_component: relay
      address: "10"
      name: "Carport Socket"
      unique_id: "teletask_switch_carport"
```

### Using a separate file (recommended for large setups)

In `configuration.yaml`:

```yaml
teletask: !include teletask.yaml
```

In `teletask.yaml`:

```yaml
host: 192.168.1.100
port: 55957

light:
  - doip_component: relay
    address: "1"
    name: "Living Room Light"
    unique_id: "teletask_light_living"

switch:
  - doip_component: relay
    address: "10"
    name: "Carport Socket"
    unique_id: "teletask_switch_carport"
```

### Global parameters

| Parameter | Type    | Required | Description                                                              |
| --------- | ------- | -------- | ------------------------------------------------------------------------ |
| `host`    | String  | Yes      | IP address or hostname of your Teletask central unit                     |
| `port`    | Integer | Yes      | DoIP port (default: **55957**)                                           |

### Light parameters

| Parameter            | Type   | Required | Description                                                                 |
| -------------------- | ------ | -------- | --------------------------------------------------------------------------- |
| `doip_component`     | String | Yes      | Teletask component type: `relay`, `locmood`, `genmood`, or `flag`           |
| `address`            | String | Yes      | Address/identifier on the Teletask DoIP bus for the component               |
| `brightness_address` | String | No       | Address for the dimmer channel (enables brightness control)                 |
| `name`               | String | Yes      | Friendly name displayed in Home Assistant                                   |
| `unique_id`          | String | No       | Unique identifier for the entity (recommended, e.g. a descriptive string)   |

### Switch parameters

| Parameter        | Type   | Required | Description                                                                 |
| ---------------- | ------ | -------- | --------------------------------------------------------------------------- |
| `doip_component` | String | Yes      | Teletask component type: `relay`, `locmood`, `genmood`, or `flag`           |
| `address`        | String | Yes      | Address/identifier on the Teletask DoIP bus for the component               |
| `name`           | String | Yes      | Friendly name displayed in Home Assistant                                   |
| `unique_id`      | String | No       | Unique identifier for the entity (recommended, e.g. a descriptive string)   |

## Finding your Teletask addresses

To find the correct `doip_component` type and `address` for each device, you can:

1. Open **PROSOFT** (Teletask's configuration software) and look at the function list for each relay, local mood, general mood, or flag.
2. Check the **Teletask web interface** (if available) for the assigned numbers.
3. Refer to the documentation provided by your Teletask installer.

The `address` corresponds to the function number in Teletask (e.g., Relay 1, Relay 31, Local Mood 5, etc.).

## Troubleshooting

### No devices appearing after installation

- Check the Home Assistant logs (**Settings > System > Logs**) and search for "teletask".
- Verify that no other application (PROSOFT, web interface) is connected to the Teletask central unit on the same user account.
- Confirm the DoIP license (TDS15132) is active on your controller.
- Make sure the `host` and `port` are correct and reachable from Home Assistant.

### Devices show as unavailable

- The Teletask central unit may have lost network connectivity.
- Another client may have taken over the DoIP session. Restart Home Assistant to reconnect.

### Lights or switches not responding

- Double-check the `doip_component` type and `address` against your PROSOFT configuration.
- Ensure the Teletask central unit is powered on and functioning.

## Compatibility

This fork has been tested with:

- Home Assistant 2024.x, 2025.x, 2026.x
- Teletask MICROS+ and PICOS central units
- DoIP protocol via the pyteletask library

## Credits

- Original integration by [@Tiemooowh](https://github.com/Tiemooowh)
- Maintained by [@dsmagghe](https://github.com/dsmagghe)
- Based on the [pyteletask](https://pypi.org/project/pyteletask/) library

## License

This project is open source. Feel free to fork and contribute.
