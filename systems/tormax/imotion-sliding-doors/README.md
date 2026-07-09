# TORMAX iMotion Sliding Doors (MCU32-TERM-B)

## Overview
*   **System Type:** Automatic sliding door terminal module (MCU32-TERM-B) for TORMAX iMotion 2202, 2301, and 2401 drive controllers.
*   **Common Use Cases:** Connecting motion detectors, safety sensors, key switches, and external cloud access control systems to automatic sliding doors.
*   **Power Input & Limits:**
    *   **iMotion 2202 / 2301:** 24 VDC max. 18 W / 0.75 A | Total load max. 30 W
    *   **iMotion 2401:** 24 VDC max. 36 W / 1.5 A | Total load max. 50 W
    *   **40 V PWM Output (all models):** Max. 24 W / 2 A (equivalent 6 ... 24 VDC, programmable). Only suitable for purely resistive or inductive loads like holding magnets, relays, or signal lamps.

## Physical Interface & Pinout

The MCU32-TERM-B terminal module provides pre-configured inputs, outputs, and safety connections. The functions of the "IN" inputs, and the "OUT" and "PWM" outputs can be programmed via the user interface. The following table describes the default terminal allocation.

![schema imotion](./imotion.drawio.svg)

### Inputs & Outputs (Default Programming)

| Terminal | Signal | Type | Default Function Description | Activation Method / Level |
|---|---|---|---|---|
| **C1** | GND | Ground | Reference ground for internal impulse generator | - |
| **C2** | IN | Input | Internal impulse generator (in1) | Close contact to GND |
| **C3** | 24V | Output | Power supply for internal impulse generator | 22.5 – 24.5 VDC (min. 16.5 V in battery operation) |
| **C4** | GND | Ground | Reference ground for external impulse generator | - |
| **C5** | IN | Input | External impulse generator (in2) | Close contact to GND |
| **C6** | 24V | Output | Power supply for external impulse generator | 22.5 – 24.5 VDC (min. 16.5 V in battery operation) |
| **D1** | GND | Ground | Reference ground for key switch | - |
| **D2** | IN | Input | Key switch / Cloud access signal (in3) | Close contact to GND |
| **D3** | 24V | Output | Power supply for key switch | 22.5 – 24.5 VDC (min. 16.5 V in battery operation) |
| **D4** | GND | Ground | Reference ground for operating mode override | - |
| **D5** | IN | Input | Operating mode OFF / Lockdown (in4) | Close contact to GND |
| **D6** | 24V | Output | Power supply for operating mode override | 22.5 – 24.5 VDC (min. 16.5 V in battery operation) |
| **E1** | OUT | Output | Holding brake control signal (pwm) | Switches to GND (Open Collector) |
| **E2** | 40V | Output | Holding brake power supply (pwm) | 40 V PWM (equivalent 6 ... 24 VDC) |
| **E3** | OUT | Output | Chime / Bell control signal (out1) | Switches to GND (Open Collector) |
| **E4** | 24V | Output | Chime / Bell power supply (out1) | 22.5 – 24.5 VDC |
| **E5** | OUT | Output | "Door Closed" status signal (out2) | Switches to GND (Open Collector) |
| **E6** | 24V | Output | Status signal power supply (out2) | 22.5 – 24.5 VDC |

### Safety Connections (Safeties)

Dedicated interfaces are available for safety sensors (Single Plug SE1–SE4 or Double Plug SR1–SR4 / ST1–ST4):

*   **sf1 (Terminals A1–A4 / SE1 / SR1/ST1):** Safety Closing 1 (Internal combination sensor)
*   **sf2 (Terminals A5–A8 / SE2 / SR2/ST2):** Safety Closing 2 (External combination sensor)
*   **sf3 (Terminals B1–B4 / SE3 / SR3/ST3):** Safety Opening 1
*   **sf4 (Terminals B5–B8 / SE4 / SR4/ST4):** Safety Opening 2

*Pin allocation per safety channel (e.g. sf1):*
*   **A1 / B1 / B5 / A5:** GND (Reference ground)
*   **A2 / B2 / B6 / A6:** IN (Safety signal)
*   **A3 / B3 / B7 / A7:** OUT (Test signal from controller to sensor)
*   **A4 / B4 / B8 / A8:** 24V (Power supply)


## Configuration & Behavior

The iMotion controller processes inputs depending on the selected operating mode.

### Standard Operating Modes
The operating mode can be changed via the TORMAX Functional Control Panel (FCP) or external control inputs:

1.  **AUTOMAT 1:** Normal two-way traffic mode. Both internal (`in1`) and external (`in2`) sensors trigger door opening.
2.  **EXIT:** One-way traffic mode. Only the internal sensor (`in1`) is active. The external sensor (`in2`) is ignored.
3.  **OFF:** The door closes and locks mechanically (if an electric lock is installed). All movement sensors (`in1`, `in2`) are disabled. Ingress is only possible via Key Switch input `in3`.
4.  **PERMANENT OPEN:** The door opens fully and remains open.
5.  **MANUAL:** The motor is de-energized. The door leaves can be moved freely by hand.

### Programmability
The input and output logic can be customized using the TORMAX control panel or the service software (e.g., *iMotion Skipper*).
*   **Inputs:** Can be configured as Normally Open (N.O.) or Normally Closed (N.C.).
*   **Outputs:** By default, outputs switch to ground (GND) when active.

## Practical Integration Examples

### Integration 1: "Public" Mode (Normal Operation)

*   **Goal:** The door acts as a standard automatic door. Both internal and external motion detectors are active.
*   **Wiring:**
    *   Standard internal and external motion detectors remain connected to `in1` (C1–C3) and `in2` (C4–C6) respectively.
*   **Operation:**
    *   The door is set to **AUTOMAT 1** mode on the physical control panel.
    *   People can pass through the door freely from both sides.


### Integration 2: "Secure" Mode (Access Control from Outside)

*   **Goal:** No free entry from outside and no free egress from inside via motion detectors (all movement sensors are disabled). Only signals on key switch `in3` open the door (e.g. from a physical push button or an "Access" relay).
*   **Wiring:**
    *   Connect "Access" relay to **D1 (GND)** and **D2 (IN)** of input `in3` (Key switch).
    *   Connect a momentary, potential-free egress push-button in parallel to the relay across **D1 (GND)** and **D2 (IN)** of input `in3`.
*   **Operation & Behavior:**
    *   Set the door to **OFF** mode on the physical control panel (or use lockdown as described below).
    *   Both the internal and external motion detectors (`in1` and `in2`) are disabled by the door controller.
    *   When the relay (for outside entry) or a user presses the egress push-button (for inside egress), the contact on `in3` closes momentarily to GND. The door performs an opening cycle and closes again.


### Integration 3: "Lockdown" Mode (Full Lockout)

*   **Goal:** The door closes and locks immediately. All sensors (internal and external) are ignored.
*   **Wiring:**
    *   Connect "Lockdown" relay to **D4 (GND)** and **D5 (IN)** of input `in4` (Mode OFF).
*   **Operation & Behavior:**
    *   Close "Lockdown" relay continuously.
    *   The closed contact on `in4` overrides the current operating mode and forces the controller into **OFF** mode.
    *   The door closes, locks, and ignores all opening signals from `in1` and `in2`.
    *   *Note:* Once the "Lockdown" relay opens, the door automatically returns to the mode selected on the control panel (e.g., AUTOMAT 1).

> [!IMPORTANT]
> **Emergency Exit & Life Safety Compliance (Lockdown Override):**
> Activating continuous lockdown via `in4` puts the controller in `OFF` mode, disabling motion sensors and engaging the lock. To ensure escape route clearance under fire or evacuation scenarios:
> 1. **Fire Alarm / Emergency Open Input:** Configure an unused input (e.g., `in5` or `in6`) via the control panel or *iMotion Skipper* software as **"Emergency Open" (Not-Auf)** or **"Fire Alarm" (Brandfall)**. Connect the Fire Alarm System (FAS) or emergency break-glass switch to this input. Triggering this input immediately overrides the lockdown state on `in4` and forces the door fully open.
> 2. **FR-Model Redundancy (Escape Routes):** Ensure the drive is an Escape Route ("FR") model equipped with a monitored dual-motor/battery backup system. It is certified to open the door automatically in an emergency or during power failure.
> 3. **Mechanical Break-out:** If installed on a designated evacuation route, the door system must utilize breakout-capable door leaves that swing outward mechanically when pushed from the inside, bypassing any active electronic locks or motor commands.

