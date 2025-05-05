# ESPHome Alarm System – Proof of Concept

This is a work-in-progress ESPHome-based alarm system, designed to be:

- Independent: can operate without Home Assistant
- Integrated: optional integration with Home Assistant & HomeKit
- Customizable: based on ESP32 with wired sensors (I2C & GPIO)
- Extendable: support for external display and keypad planned

## Features

- Binary sensors (window contact + motion via MCP23017)
- Siren control via relay
- Away & Night alarm zones
- Entry delay support
- Anti-tamper loop
- Push and persistent notifications (HA + mobile)
- Modular configuration with YAML includes
- Alarm logic embedded in ESP32

## Usage

To compile and upload:

esphome run main.yaml

Requires the ESPHome CLI and a connected ESP32 (USB or via CH340 serial adapter)

## Project Structure

```
main.yaml                  # Main entry point
secrets.yaml              # Contains alarm_code, notify target, etc. (gitignored)
alarm.yaml                # Alarm logic (template platform)
outputs.yaml              # Relay outputs
switches.yaml             # Siren switch
scripts/
  ├── check.yaml            # Check opened zones before arming
  ├── notifications.yaml    # Persistent & push notifications
  └── trigger.yaml          # Trigger logic for alarm modes
binary_sensors/
  ├── pir.yaml              # Motion and sabotage sensors
  └── windows.yaml          # Window contact sensors
```

## Example secrets.yaml

alarm_code: "1234"
notify_device: notify.mobile_app_iphone_xxx

Used in YAML like:

codes:
  - !secret alarm_code

- homeassistant.service:
    service: !secret notify_device

## Requirements

- ESP32 (Ethernet-capable preferred, e.g. WT32-ETH01)
- MCP23017 I/O expander
- 3.3V relay module
- NC wired sensors (window, motion)
- Power supply (5V and/or 12V)
- Home Assistant (optional)

## Roadmap

- Add 1602 LCD via I²C
- Add matrix keypad via MCP23017
- Display alarm status and enter code
- Power loss & tamper alerts
- Fully standalone operation
- Home Assistant dashboard

## Status

This is a Proof of Concept. Functional, but actively evolving.
Contributions and feedback welcome!