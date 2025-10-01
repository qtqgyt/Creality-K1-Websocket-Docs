# Creality K1 WebSocket Protocol Documentation

This document provides reverse-engineered details about the Creality K1 3D printer's WebSocket protocol, used by the printer's web UI, Creality Print, and other tools.

## General Information

- **WebSocket Port:** `9999`  
  The printer's WebSocket server listens on port 9999 of its WiFi interface.
- **Purpose:**  
  The WebSocket handles all communication for the printer, including control commands and status updates.

## Fan Control

The K1 printer has three fans, each with its own identifier:

| Fan Name    | Command Key   | Fan Number |
|-------------|---------------|------------|
| Model Fan   | `fan`         | `0`        |
| Back Fan    | `fanCase`     | `1`        |
| Side Fan    | `fanAuxilary` | `2`        |

### Turning Fans On or Off

To turn a fan on or off, send a command in this format: `{"method": "set", "params": {"<fanValue>": <onoff>}}`

- `fanValue`: One of `fan`, `fanCase`, or `fanAuxilary` (see table above)
- `onoff`: `1` to turn on, `0` to turn off

**Example:** Turn on the model fan: `{"method": "set", "params": {"fan": 1}}`

### Setting Fan Speed

To set a fan's speed, use a G-code command via the WebSocket: `{"method": "set", "params": {"gcodeCmd": "M106 P<fanNumber> S<speed>"}}`

- `fanNumber`:  
  - `0` = Model Fan  
  - `1` = Back Fan  
  - `2` = Side Fan
- `speed`: Integer from `0` (off) to `255` (full speed)

**Example:** Set the side fan to half speed: `{"method": "set", "params": {"gcodeCmd": "M106 P2 S128"}}`
