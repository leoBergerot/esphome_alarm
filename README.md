# ESPHome Alarm System – Proof of Concept

This is a work-in-progress ESPHome-based alarm system, designed to be:

- **Independent**: can operate without Home Assistant
- **Integrated**: optional integration with Home Assistant
- **Customizable**: based on ESP32 with wired sensors (I2C & GPIO)
- **Extendable**: now includes support for external LCD display and keypad

## Features

- Binary sensors (window contact + motion via MCP23017)
- Siren control via relay
- Away & Night alarm zones
- Entry delay support
- Anti-tamper loop
- Push and persistent notifications (HA + mobile)
- Modular configuration with YAML includes
- Alarm logic embedded in ESP32
- LCD 1602 I²C display (status messages, code entry)
- Matrix keypad (arming/disarming via physical code)
- Automatic screen wake-up on keypress  
- Code validation and mode switching via keypad  
- Scrollable text on the LCD when the message is too long
- When arming, if any windows are open, a warning is shown on the LCD screen (push notifications also sent if Home Assistant is available)  

## Usage

To compile and upload:

```
esphome run main.yaml
```

Requires the ESPHome CLI and a connected ESP32 (USB or via CH340 serial adapter)

## Project Structure

```
main.yaml                  # Main entry point
secrets.yaml              # Contains alarm_code, notify target, etc. (gitignored)
alarm.yaml                # Alarm logic (template platform)
outputs.yaml              # Relay outputs
switches.yaml             # Siren switch
globals.yaml              # Global variables (e.g., display state)
display.yaml              # LCD screen configuration
keypad.yaml               # Keypad matrix config and code logic
scripts/
  ├── check.yaml            # Check opened zones before arming
  ├── notifications.yaml    # Persistent & push notifications
  ├── trigger.yaml          # Trigger logic for alarm modes
  └── lcd_scroll_line.yaml # Helper script to update display
binary_sensors/
  ├── pir.yaml              # Motion and sabotage sensors
  └── windows.yaml          # Window contact sensors
```

## Example `secrets.yaml`

```yaml
alarm_code: "1234"
notify_device: notify.mobile_app_iphone_xxx
alarm_name: "Alarme Maison"
```

Used in YAML like:

```yaml
codes:
  - !secret alarm_code

- homeassistant.service:
    service: !secret notify_device
```

## How to Use the Keypad

The matrix keypad allows local control of the alarm system using a 4-digit code:

| Key | Function                  |
|-----|---------------------------|
| `A` | Arm in "Away" mode        |
| `B` | Arm in "Night" mode       |
| `C` | Disarm the alarm          |
| `*` | Delete last entered digit |

### Example:
To arm the system in *Away* mode:

1. Type your 4-digit code  
2. Press `A`

To disarm:

1. Type your 4-digit code  
2. Press `C`

If the code is incorrect, a message will appear.  
You **must disarm first** before switching between *Away* and *Night* modes.

## Requirements

- ESP32 (Ethernet-capable preferred, e.g. WT32-ETH01)
- MCP23017 I/O expanders (for sensors and keypad)
- 3.3V relay module
- NC wired sensors (window, motion)
- LCD 1602 with I²C interface
- Matrix 4x4 keypad
- Power supply (5V and/or 12V)
- Home Assistant (optional)

## Roadmap
- Clear the entire code when pressing #
- Change or add a password
- Power loss & tamper alerts  
- Battery backup integration  
- Wiring diagram
- Wireless keyboard/screen ?

## Status

This is a **Proof of Concept**. Functional and modular, but actively evolving.  
**Contributions and feedback welcome!**