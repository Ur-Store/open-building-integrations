# Gilgen SL35 Sliding Doors (Control 35)

## Overview
*   **System Type:** Automatic sliding door control unit (Control 35 / Control 35 II) for Gilgen SL35 sliding door systems.
*   **Common Use Cases:** Connecting motion detectors, safety sensors, program selectors, key switches, and external cloud access control/building automation systems.
*   **Power Input & Limits:**
    *   **Control Power:** 24 VDC auxiliary output for external accessories (typically max. 15–20 W).
    *   **Main Input:** 230 VAC (50/60 Hz).

## Physical Interface & Pinout

This section details the primary control pins, their functions, and how to activate them.

![schema control 35](./control35.drawio.svg)

### Control Pins & Terminal Allocation

| Pin # | Label | Function Description | Activation Method | Notes |
|---|---|---|---|---|
| **1** | +24V | Auxiliary Power Output | - | Supplies +24 VDC to power external sensors, motion detectors, or access control modules. |
| **2** | GND | Reference Ground | - | Reference potential (0 V) for the control inputs, specifically the Key input. |
| **3** | Key | Key Switch Impulse (Schlüsseltaster) | Momentary connection to Pin 2 (GND) | Triggers a single door opening cycle. Active in all operating modes, including NIGHT/LOCKED mode. |
| **4** | G,E,L | Electric Lock Control / Mode Status | Determined by controller state | Connection for electric lock (Geschlossen, Einweg, Ladenöffnung) or mode feedback. |
| **5** | EmA | Emergency Circuit A (Not-Aus / Not-Auf) | NC loop connected to Pin 6 (EmB) | Part of the Normally Closed (NC) safety loop. Must be connected to Pin 6 for normal operation. |
| **6** | EmB | Emergency Circuit B (Not-Aus / Not-Auf) | NC loop connected to Pin 5 (EmA) | Breaking the connection between Pin 5 and Pin 6 triggers immediate emergency action (e.g. stop or open). |
| **7** | Locked | Locked Mode (Nacht / Geschlossen) | Continuous connection to Pin 13 (GND) | Sets the door to LOCKED/NIGHT mode. Door closes, locks (if installed), and ignores motion detectors. |
| **8** | Manual | Manual Mode (Handbetrieb) | Continuous connection to Pin 13 (GND) | Sets the door to MANUAL mode. The motor is de-energized, allowing manual movement of the door leaves. |
| **9** | OneWay | One-Way Mode (Einweg / Exit Only) | Continuous connection to Pin 13 (GND) | Sets the door to ONE-WAY mode. External sensors are disabled; only internal sensors trigger opening. |
| **10** | Auto | Automatic Mode (Ladenöffnung) | Continuous connection to Pin 13 (GND) | Sets the door to AUTOMATIC mode. Sensors on both sides are active for normal two-way traffic. |
| **11** | Open | Hold Open Mode (Dauer-Auf) | Continuous connection to Pin 13 (GND) | Sets the door to HOLD OPEN mode. The door opens fully and remains open as long as connection is active. |
| **12** | RedOpen | Redundant Open Mode (Fluchtweg) | Continuous connection to Pin 13 (GND) | Sets the door to Redundant Open mode, typically used to force the door open for emergency escape routes. |
| **13** | GND | Reference Ground for Modes | - | Common ground terminal used for the program selector inputs (Pins 7 through 12). |

## Configuration & Behavior

The Gilgen SL35 controller processes operating modes based on the grounded terminals of the program selector interface.

### Standard Operating Modes

1.  **AUTOMATIC (Auto):** Normal two-way traffic mode. Both internal and external motion detectors trigger door opening.
2.  **ONE-WAY (OneWay):** One-way traffic mode (Exit Only). Only the internal motion detector is active; the external sensor is ignored.
3.  **LOCKED (Night):** The door closes and locks mechanically (if an electric lock is installed). All standard motion detectors are disabled. Ingress is only possible via Key Switch input (Pin 3).
4.  **MANUAL (Manual):** The motor is de-energized. The door leaves can be moved freely by hand.
5.  **HOLD OPEN (Open):** The door opens fully and remains in the open position continuously.
6.  **REDUNDANT OPEN (RedOpen):** Forces the door open using a redundant control circuit, ensuring emergency escape route clearance.

## Practical Integration Examples

### Integration 1: "Public" Mode (Normal Operation)

*   **Goal:** The door acts as a standard automatic door. Both internal and external motion detectors are active.
*   **Wiring:**
    *   Standard internal and external motion detectors remain active.
    *   The operating mode is set by bridging **Pin 10 (Auto)** to **Pin 13 (GND)**.
*   **Operation:**
    *   The door is in **Auto** mode.
    *   People can pass through the door freely from both sides.

### Integration 2: "Secure" Mode (Access Control from Outside)

*   **Goal:** No free entry from outside and no free egress from inside via motion detectors (all movement sensors are disabled). Only secure signals open the door (e.g., from an access control relay or egress button).
*   **Wiring:**
    *   Connect the external access control system relay (Normally Open, potential-free contact) to **Pin 2 (GND)** and **Pin 3 (Key)**.
    *   Connect a momentary, potential-free egress push-button in parallel to the relay across **Pin 2 (GND)** and **Pin 3 (Key)**.
*   **Operation & Behavior:**
    *   The door is set to **Locked / Night** mode by bridging **Pin 7 (Locked)** to **Pin 13 (GND)**.
    *   Both internal and external motion detectors are disabled by the door controller.
    *   When the access relay or egress push-button closes momentarily, it pulls Pin 3 (`Key`) to Pin 2 (`GND`). The door performs a single opening cycle, then closes and locks again.

### Integration 3: "Lockdown" Mode (Full Lockout)

*   **Goal:** The door closes and locks immediately. All sensors (internal and external) are ignored.
*   **Wiring:**
    *   Connect a Normally Open (NO) potential-free contact of a "Lockdown" switch or building automation relay to **Pin 7 (Locked)** and **Pin 13 (GND)**.
*   **Operation & Behavior:**
    *   Closing the lockdown relay continuously bridges **Pin 7 (Locked)** and **Pin 13 (GND)**.
    *   This continuous input overrides the program selector and forces the controller into **Locked / Night** mode.
    *   The door closes immediately, engages the electric lock, and ignores all opening signals from motion detectors.
    *   *Emergency Stop Alternative:* For safety-critical physical emergency lockout, a Normally Closed (NC) "Emergency Stop" button can be wired in series into the safety loop between **Pin 5 (EmA)** and **Pin 6 (EmB)**. Breaking this connection immediately triggers an emergency state.
    *   Once the lockdown contact opens, the door automatically returns to its previously selected operating mode.
