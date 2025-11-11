# [Manufacturer] [Model]

## Overview
*   **System Type:** (e.g., Automatic Sliding Door Controller, Access Control Unit)
*   **Common Use Cases:** (e.g., Used for retail entrances, integration with fire alarm systems.)
*   **Power Input:** (e.g., 230V AC, 24V DC)

## Physical Interface & Pinout

This section details the primary control pins, their functions, and how to activate them.

| Pin # | Label | Function Description | Activation Method | Notes |
|---|---|---|---|---|
| | | | | |
| | | | | |
| | | | | |
| | | | | |

#### **Example Pinout:**

| Pin # | Label | Function Description | Activation Method | Notes |
|---|---|---|---|---|
| 4 | GND | Common ground for control signals. | - | Reference for all impulse signals. |
| 5 | IMP | Triggers a single open/close cycle ("Impulse"). | Momentary connection to Pin 4 (GND). | Standard behavior for push buttons. |
| 6 | DAUER | Holds the door open ("Permanent Open"). | Continuous connection to Pin 4 (GND). | Overrides impulse signals. |
| 10| NOT | Emergency Stop. | Connection to Pin 4 (GND). | Requires reset after activation. |
| 12| 24V | 24V DC Output | - | Can be used to power small external devices (e.g., a receiver). Max 100mA. |

## Configuration & Behavior

The behavior of the physical interface can often be modified by on-board settings. This section documents those configurations.

### DIP Switch Settings

| Switch # | Position | Function |
|---|---|---|
| 1 | ON | Activates "Pharmacy Mode" - door opens only slightly. |
| 1 | OFF | Standard full opening width. |
| 4 | ON | Pin 6 (DAUER) function is inverted; requires an *open* circuit to hold the door open. |
| 4 | OFF | Standard behavior for Pin 6. |

### Service Menu Options

(If applicable, describe key software settings accessible via a service menu)

*   **Menu 2.1 - Impulse Type:**
    *   `Standard` (Default): A single impulse opens the door. Another impulse while open will close it.
    *   `Toggle`: Every impulse toggles the state (Open -> Close -> Open).
*   **Menu 5.4 - Lock on Close:**
    *   `Enabled`: The system will engage an electric lock when the door is fully closed.
    *   `Disabled`: The door is not locked when closed.


## Practical Integration Examples

### Example 1: Connect a Simple Push-Button

*   **Goal:** A button that opens the door when pressed.
*   **Steps:**
    1.  Ensure **DIP Switch 1** is **OFF**.
    2.  Connect the two wires from a standard momentary push-button to **Pin 4 (GND)** and **Pin 5 (IMP)**.
    3.  Pressing the button will now trigger a standard open/close cycle.

### Example 2: Integrate with a Fire Alarm Relay

*   **Goal:** The door should automatically open and stay open when the fire alarm is active.
*   **Steps:**
    1.  Ensure **DIP Switch 4** is **OFF**.
    2.  Use a Normally Open (NO) relay from the fire alarm system.
    3.  Connect the relay's common (COM) and NO terminals to **Pin 4 (GND)** and **Pin 6 (DAUER)**.
    4.  When the alarm triggers, the relay closes, creating a continuous connection that holds the door open.

