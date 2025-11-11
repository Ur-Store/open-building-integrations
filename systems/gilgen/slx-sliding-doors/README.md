# Gilgen SLX Sliding Doors

## Overview
*   **System Type:** All Gilgen SLX sliding door systems utilizing the Neste 3 control unit.

## Physical Interface & Pinout

This section details the primary control pins, their functions, and how to activate them.

![schema neste 3](./neste3.drawio.svg)

| Pin # | Label | Function Description | Activation Method | Notes |
|---|---|---|---|---|
| 1 | D-Bedix+ | | | |
| 2 | D-Bedix- | | | |
| 3 | OKA | | | |
| 4 | OKI | | | |
| 5 | GND | | | |
| 6 | +24V | | | |
| 7 | PS Nacht | | | |
| 8 | Not Stop | | | |
| 9 | GND | | | |
| 10 | SI Test |  | | |
| 11 | SIR2 |  |  |  |
| 12 | SIR1 | | | |
| 13 | GND | | | |
| 14 | +24V | | | |
| 15 | Gong | | | |

## Configuration & Behavior

### Operation Modes

- `AUTOMATIC`: Door not locked and operates automatically based on sensor inputs.
- `NIGHT`: Door is locked (if optional locking mechanism is installed) and does not respond to open commands except from Key operated impulse switch or the F-KEYs.
- `OPEN`: Door remains open continuously.
- `MANUAL`: Doors are unlocked and can be operated manually without motor assistance.
- `EXIT`: Door only allows traffic from inside towards the outside (typically for exit-only doors on shop closures).


## Practical Integration Examples

### Trigger Door Opening

*   **Goal:** Trigger a door opening impulse from a external system with a relay switch.
*   **Steps:**
    1.  Install relay for connection between Pin 11 (Open) and 13 (GND).
    2.  Close relay for the amount of time the door should open from external system to open the door.
