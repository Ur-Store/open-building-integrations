# TORMAX TEP / TCP-51 Sliding Doors

## Overview
*   **System Type:** Legacy generation automatic sliding door system (pre-iMotion series).
*   **Drive Models (Antriebe):** TEP, TKP, TSP, TOP.
*   **Control Unit (Steuerung):** TCP-51 (and variants like TCP-51LC, TCP-101).
*   **System Name:** Often referred to as **TEP STARDOR** (where Stardor describes the door profile/system).
*   **Common Use Cases:** Older commercial installations (shopping centers, hospitals) using classical gear-driven technology.
*   **Power Input & Limits:**
    *   **Main Power:** 230/115 VAC, 50/60 Hz, max. 160 W.
    *   **Auxiliary Output:** 24 VDC, max. load 12 W (total load across all 24V outputs combined).

## Nomenclature & Classification
The naming can be confusing as it refers to different components:
*   **TEP / TKP:** The specific motor/drive unit installed in the header.
*   **TCP-51:** The actual electronic control board where all wiring and programming occur.
*   **Stardor / Teldor / Picdor:** The mechanical door system types (e.g., Stardor = Standard Sliding Door, Teldor = Telescopic).

## Physical Interface (TCP-51 Control Unit)

The TCP-51 controller uses four main terminal blocks (A, B, C, D) located on the main board, plus optional expansion modules.

### Terminal Overview

![TCP51](./tcp51.drawio.svg)

### Terminal Block A (Security Light Beams - default)

| Terminal | Signal | Default Function Description | Activation Method / Level |
|---|---|---|---|
| **A1** | GND | Reference ground (Masse) for light beam 1 receiver | - |
| **A2** | IN | Light beam 1 receiver input | NPN transistor (pulled to GND when active) |
| **A3** | +24V | Power supply for light beam 1 receiver | +24 VDC, max. load across system: 12 W |
| **A4** | GND | Reference ground for light beam 1 transmitter | - |
| **A5** | OUT | Test signal output to light beam 1 transmitter | Square-wave pulse for self-monitoring |
| **A6** | GND | Reference ground for light beam 2 receiver | - |
| **A7** | IN | Light beam 2 receiver input | NPN transistor (pulled to GND when active) |
| **A8** | +24V | Power supply for light beam 2 receiver | +24 VDC, max. load across system: 12 W |
| **A9** | GND | Reference ground for light beam 2 transmitter | - |
| **A10** | OUT | Test signal output to light beam 2 transmitter | Square-wave pulse for self-monitoring |

### Terminal Block B (Control Panel)

Used for connecting the 5-position TORMAX functional control panel (FCP) and the panel lock (Panelschloss).

| Terminal | Signal | Default Function Description | Type / Behavior |
|---|---|---|---|
| **B1** | GND | Reference ground / Shield (Abschirmung) | Reference |
| **B2** | UP | FCP key/button UP signal | Input (closed to GND) |
| **B3** | DOWN | FCP key/button DOWN signal | Input (closed to GND) |
| **B4** | P.LOCK | Panel lock contact | Input (closed to B1 to lock panel adjustments) |
| **B5** | OFF | Mode "OFF" indicator | Output (switches to GND to drive panel LED) |
| **B6** | AUTO | Mode "AUTOMAT" indicator | Output (switches to GND to drive panel LED) |
| **B7** | RED | Mode "REDUCIERT" (Reduced width) indicator | Output (switches to GND to drive panel LED) |
| **B8** | EXIT | Mode "EXIT" (One-way) indicator | Output (switches to GND to drive panel LED) |
| **B9** | OPEN | Mode "OPEN" (Permanent open) indicator | Output (switches to GND to drive panel LED) |
| **B10** | +24V | Power supply for FCP LEDs and logic | +24 VDC |

### Terminal Block C (Activators & Programmable Inputs)

| Terminal | Signal | Default Function Description | Activation Method / Level |
|---|---|---|---|
| **C1** | GND | Reference ground for internal motion sensor | - |
| **C2** | IN | Internal motion sensor activation contact | Close contact to GND to open |
| **C3** | GND | Reference ground for programmable input 1 | - |
| **C4** | IN | **Programmable Input 1** (e.g. auxiliary sensor/override) | Close contact to GND (configured via SERCOM) |
| **C5** | +24V | Power supply output for internal motion sensor | +24 VDC |
| **C6** | GND | Reference ground for external motion sensor | - |
| **C7** | IN | External motion sensor activation contact | Close contact to GND to open |
| **C8** | GND | Reference ground for programmable input 2 | - |
| **C9** | IN | **Programmable Input 2** (e.g. auxiliary sensor/override) | Close contact to GND (configured via SERCOM) |
| **C10** | +24V | Power supply output for external motion sensor | +24 VDC |


### Terminal Block D (Service & Emergency)

| Terminal | Label | Default Function Description | Activation Method / Level |
|---|---|---|---|
| **D1** | +24 VDC | Power supply output for emergency stop loop | +24 VDC |
| **D2** | E. PB | Emergency Push Button (Notaus-Schalter) loop | Normally Closed (NC) loop. Opening contact stops door |
| **D3** | IN & RES | **Key Switch & Fault Reset** (Schlüsselschalter) | Normally Open (NO). Close to GND (D4) to reset or open |
| **D4** | GND | Reference ground | Reference |


### Optional I/O-Module Expansion

The TCP-51 controller supports the optional connection of up to three external I/O modules, providing additional programmable inputs and relay outputs.

#### Terminal Block F (Expansion Inputs)

| Terminal | Signal | Function Description | Type / Behavior |
|---|---|---|---|
| **F1** | GND | Reference ground for additional sensor power | - |
| **F2** | IN | **Programmable Input F2** | Configured via TORMAX-SERCOM |
| **F3** | GND | Reference ground | - |
| **F4** | +24V | Power supply output for expansion sensors | +24 VDC |
| **F5** | GND | Reference ground | - |
| **F6** | IN | **Programmable Input F6** | Configured via TORMAX-SERCOM |
| **F7** | IN | **Programmable Input F7** | Configured via TORMAX-SERCOM |

#### Relay Outputs (REL A / REL B)

Each physical I/O module contains up to two relays (**REL A** and **REL B**) mapping to programmable outputs OUT 1-6.
*   **Terminals E11, E12, E14:** Standard potential-free changeover (SPDT) contacts representing Normally Closed (NC), Normally Open (NO), and Common (COM).
*   **Max Switching Capacity:** 42 VADC / 10 A (AC1).
*   **Default Configurations:** Standard indicators like "Door Closed" (Tür geschlossen), "Door Open" (Tür offen), or "Fault" (Sammelstörung) are programmed via TORMAX-SERCOM.


## Configuration & Behavior

The TCP-51 controller processes activator inputs based on the selected operating mode and safety loops.

### Standard Operating Modes
Modes are selected using the UP/DOWN keys on the functional control panel (FCP):
1.  **AUTO (Automat):** Standard two-way traffic. Both the internal activator (`C2`) and external activator (`C7`) trigger door opening.
2.  **RED (Reduced):** Same as AUTO, but the door leaves only open to a programmed reduced width.
3.  **EXIT:** One-way traffic. Only the internal activator (`C2`) triggers door opening. The external activator (`C7`) is ignored.
4.  **OFF:** The door closes and locks mechanically (if an electric lock is installed). All movement sensors (`C2`, `C7`) are disabled. Ingress is only possible via Key Switch input `D3`.
5.  **OPEN (Permanent Open):** The door opens fully and remains open.

### System Reset & Calibration Run (Eichlauf)
Following mechanical changes, adjustments, or to clear faults (e.g. after emergency stop):
*   Press and hold any FCP button (or perform a power cycle) for **5 seconds**.
*   The system executes a slow calibration run (*Kriechgang*) to determine the end stops.
*   After three unobstructed opening/closing cycles, normal speed and operation are restored.

### Programmability
The behavior of inputs `C4`, `C9`, `F2`, `F6`, `F7` and the relay outputs can be customized using the **TORMAX-SERCOM** service computer.

## Practical Integration Examples

### Integration 1: "Public" Mode (Normal Operation)

*   **Goal:** The door acts as a standard automatic door. Both internal and external motion detectors are active.
*   **Wiring:**
    *   Connect the standard internal motion detector's potential-free contact to **C1 (GND)** and **C2 (IN)**. Power the sensor from **C3 (GND)** and **C5 (+24V)**.
    *   Connect the standard external motion detector's potential-free contact to **C6 (GND)** and **C7 (IN)**. Power the sensor from **C8 (GND)** and **C10 (+24V)**.
*   **Operation:**
    *   Set the door to **AUTO** mode on the functional control panel.
    *   People can pass through the door freely from both sides.


### Integration 2: "Secure" Mode (Access Control from Outside)

*   **Goal:** No free entry from outside and no free egress from inside via motion detectors (all movement sensors are disabled). Only signals on the key switch open the door (e.g., from a physical card reader, code lock, or cloud access system relay).
*   **Wiring:**
    *   Connect the access control system's potential-free dry relay contact across **D3 (IN & RES)** and **D4 (GND)** of Terminal Block D (Key Switch / Schlüsselschalter input).
    *   Connect a momentary, potential-free egress push-button in parallel across **D3 (IN & RES)** and **D4 (GND)** to authorize exiting.
*   **Operation & Behavior:**
    *   Set the door to **OFF** mode on the functional control panel.
    *   Both the internal and external motion detectors (`C2` and `C7`) are disabled by the door controller, and the mechanical lock engages.
    *   When the access system relay closes momentarily (for entry) or the egress button is pressed (for exit), the contact between **D3** and **D4** closes.
    *   The controller unlocks the door, performs a single complete opening and closing cycle, and then re-locks.


### Integration 3: "Lockdown" Mode (Full Lockout)

*   **Goal:** The door closes and locks immediately. All motion sensors (internal and external) are ignored.

The legacy TCP-51 does not feature a dedicated hardwired lockdown input on the main board. Two different integration methods are standard:

#### Method A: Software Override (Recommended)
*   **Wiring:**
    *   Configure programmable input **C4** (or **C9**) via the *TORMAX-SERCOM* service computer to act as the **"Operating Mode OFF" override** (Betriebsart AUS).
    *   Connect the continuous potential-free contact of the "Lockdown" relay across **C3 (GND)** and **C4 (IN)**.
*   **Operation & Behavior:**
    *   When the Lockdown relay closes continuously, the input on **C4** overrides the current operating mode and forces the controller into **OFF** mode.
    *   The door closes, locks, and ignores all activator signals (`C2`, `C7`).
    *   Once the Lockdown relay opens, the door automatically returns to the mode selected on the control panel (e.g., AUTO).

#### Method B: Emergency Loop Interrupt (Hardware-based)
*   **Wiring:**
    *   Connect the potential-free normally closed (NC) contact of the "Lockdown" relay in series with the Emergency Push Button (Notaus-Schalter) loop on **D1 (+24 VDC)** and **D2 (E. PB)**.
*   **Operation & Behavior:**
    *   When the Lockdown relay opens, it interrupts the NC emergency loop.
    *   The controller immediately executes its emergency shutdown profile (configured via SERCOM to either stop instantly or close and lock the door) and displays Fault Code 10 (Emergency Stop).
    *   Once the Lockdown relay closes the emergency loop again, the system remains in the emergency-off state until it receives a reset pulse (closing **D3** to **D4** momentarily) or mode OFF is re-selected on the control panel.
