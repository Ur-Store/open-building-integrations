# Gilgen SL35 Sliding Doors

## Overview
*   **System Type:** All Gilgen SL35 sliding door systems utilizing the Control 35 control unit.

## Physical Interface & Pinout

This section details the primary control pins, their functions, and how to activate them.

![schema control 35](./control35.drawio.svg)

| Pin # | Label | Function Description | Activation Method | Notes |
|---|---|---|---|---|
| 1 | +24V | | | |
| 2 | GND | | - | |
| 3 | Key | | | |
| 4 | G,E,L | | | |
| 5 | EmA | | | |
| 6 | EmB | | | |
| 7 | Locked | | | |
| 8 | Manual | | | |
| 9 | OneWay | | | |
| 10 | Auto | Activates AUTOMATIC mode for door (door not locked) | Connection to Pin 13 (GND) | |
| 11 | Open | Opens the door. | Connection to Pin 13 (GND) | Door stays open as long as connection is active. |
| 12 | RedOpen | | | |
| 13 | GND | | - | |

## Configuration & Behavior

### Operation Modes

- `Automatic`: Door not locked and operates automatically based on sensor inputs.
- `One-Way`: Door only allows entry from one side (typically for exit-only doors on shop closures).
- `Night`: Door is locked (if optional locking mechanism is installed) and does not respond to open commands except from Key command.
- `Manual`: Doors are unlocked and can be operated manually without motor assistance.
- `Open`: Door remains open continuously.

## Practical Integration Examples

### Trigger Door Opening

*   **Goal:** Trigger a door opening impulse from a external system with a relay switch.
*   **Steps:**
    1.  Install relay for connection between Pin 11 (Open) and 13 (GND).
    2.  Close relay for the amount of time the door should open from external system to open the door.
