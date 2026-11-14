
# 05 - Power Budget

## Overview

The purpose of this power budget is to estimate the electrical power required to operate my TCRT5000 infrared cliff detection circuit.

A power budget is important because it verifies that the selected power supply can deliver sufficient current to all components without exceeding their electrical ratings.

My cliff detection system uses a TCRT5000 reflective optical sensor, consisting of an infrared LED and a phototransistor.

The calculations below estimate current consumption and power requirements for a single sensor circuit operating from a regulated 5V DC supply.

---

## Electrical Power Flow

The Cliff Detection subsystem receives power through the external DC barrel jack.

The positive input voltage passes through the 250mA fuse before reaching the LM7805 voltage regulator.

The regulator provides a regulated +5V supply to the Curiosity Nano, sensor pull-up resistors, power indicator LED, and debugging pushbutton.

```text
External DC Power Supply
          |
          v
     J1 Barrel Jack
          |
          v
     F3 250mA Fuse
          |
          v
     LM7805 Regulator
          |
          v
       +5V Rail
          |
          +----> Curiosity Nano VTG
          |
          +----> Green Power LED
          |
          +----> Sensor Pull-up Resistors
          |
          +----> Pushbutton Pull-up

Curiosity Nano Outputs
          |
          +----> RA3 --> Left IR LED
          |
          +----> RA4 --> Right IR LED
          |
          +----> RA5 --> Yellow LED
          |
          +----> RA6 --> Red LED

All circuits share a common ground.
```

### Power Input Connections

| J1 Pin | Function |
|--------|----------|
| 1 | Positive external DC input |
| 2 | Ground |
| 3 | Switched connection, unused |

### Voltage Regulator Connections

| LM7805 Pin | Function | Connection |
|------------|----------|------------|
| 1 | Input | External voltage through F3 |
| 2 | Ground | Common ground |
| 3 | Output | Regulated +5V |

The LM7805 requires sufficient input voltage above 5V to maintain regulation. A 9V DC input is used as an example for the power calculations.

The actual supply voltage must be confirmed before hardware testing.

### Bypass Capacitors

| Reference | Value | Purpose |
|-----------|-------|---------|
| C5 | 0.33 uF | Input voltage bypassing |
| C6 | 0.1 uF | Output voltage bypassing |

These capacitors help reduce voltage disturbances and provide local bypassing near the regulator.

---

## External Curiosity Nano Power

The Curiosity Nano receives external regulated power through its VTG pin.

| Pin | Connection |
|-----|------------|
| VTG | +5V from LM7805 |
| VOFF | GND |
| GND | Common circuit ground |
| VBUS | No external connection |

Connecting VOFF to ground disables the Nano's onboard target voltage regulator.

This allows the microcontroller to operate from the external power supply while USB remains available for programming and debugging.

### External Power Procedure

1. Connect VOFF to GND.
2. Connect the external power supply ground.
3. Apply regulated +5V to VTG.
4. Connect the USB cable for MPLAB programming if required.

### USB-Only Operation

To return to USB-only operation, disconnect the external supply from VTG and remove the VOFF-to-GND connection.

A removable jumper may be used to simplify switching between power configurations.

**Safety Notes:**

- Do not connect external power to VTG while USB is connected unless VOFF is grounded.
- Do not connect VOFF to a positive supply voltage.
- Do not exceed 5.5V on VTG.
- The external 5V rail must be disconnected or isolated before switching to USB-only operation.

---

## TCRT5000 Cliff Detection Sensors

The TCRT5000 is a reflective optical sensor containing an infrared light-emitting diode and a phototransistor.

The infrared LED emits light toward the ground. When a reflective surface is present, some light returns to the phototransistor.

The phototransistor conducts current depending on the amount of reflected infrared light.

The resulting collector voltage is measured by the Curiosity Nano's analog-to-digital converter.

### Sensor Pin Connections

| Terminal | Function | Left Sensor U3 | Right Sensor U4 |
|----------|----------|----------------|-----------------|
| A | Infrared LED Anode | RA3 through R8 | RA4 through R9 |
| CATH | Infrared LED Cathode | GND | GND |
| COL | Phototransistor Collector | RA0 | RA1 |
| E | Phototransistor Emitter | GND | GND |

### Left Sensor Circuit

The left sensor uses the following components:

| Component | Value | Function |
|-----------|-------|----------|
| U3 | TCRT5000 | Reflective sensor |
| R8 | 220 Ohms | IR LED current limiting |
| R11 | 10k Ohms | Collector pull-up |
| C9 | 0.1 uF, 16V | Analog signal filtering |

RA3 controls the infrared emitter.

RA0 measures the voltage produced by the phototransistor.

### Right Sensor Circuit

The right sensor uses the same operating principle.

| Component | Value | Function |
|-----------|-------|----------|
| U4 | TCRT5000 | Reflective sensor |
| R9 | 220 Ohms | IR LED current limiting |
| R12 | 10k Ohms | Collector pull-up |
| C11 | 0.1 uF, 16V | Analog signal filtering |

RA4 controls the infrared emitter.

RA1 measures the voltage produced by the phototransistor.

### Sensor Electrical Flow

```text
Nano GPIO Output
       |
  220 Ohm Resistor
       |
  Infrared LED Anode
       |
  Infrared LED Cathode
       |
      GND


       +5V
        |
   10k Pull-up
        |
        +--------> Nano ADC Input
        |
        +--------> 0.1uF Capacitor --> GND
        |
   Phototransistor Collector
        |
   Phototransistor Emitter
        |
       GND
```

### Ground Detection

When infrared reflection increases, the phototransistor conducts more current and pulls the collector voltage toward ground.

When reflection decreases, the 10k pull-up resistor raises the collector voltage toward +5V.

| Surface Condition | Reflected Infrared | ADC Voltage |
|-------------------|--------------------|-------------|
| Reflective ground | Higher | Lower |
| Dark ground | Lower | Higher |
| Cliff or drop-off | Usually lower | Higher |

A high ADC voltage can indicate a potential cliff.

However, dark flooring can produce a similar response. The sensors must be calibrated using different surfaces, mounting heights, and lighting conditions before reliable detection thresholds are selected.

---

## Electrical Calculations

### Infrared LED Current

Each TCRT5000 infrared emitter uses a 220 Ohm current-limiting resistor.

Assume:

- GPIO voltage = 5V
- Infrared LED forward voltage = 1.2V
- Resistance = 220 Ohms

The current is calculated using Ohm's Law:

**I = (V_supply - V_forward) / R**

I = (5 - 1.2) / 220

I = 0.01727 A

**I = 17.27 mA per sensor**

| Sensor | Estimated Current |
|--------|-------------------|
| Left IR emitter | 17.27 mA |
| Right IR emitter | 17.27 mA |
| Total | 34.54 mA |

These calculations assume an ideal 5V GPIO output. Actual current depends on the GPIO output voltage and sensor forward voltage.

The microcontroller's GPIO current limits must be verified. External transistor drivers may be required if the GPIO pins cannot supply sufficient current reliably.

### Resistor Power Dissipation

Power dissipated by each 220 Ohm resistor:

**P = I²R**

P = (0.01727)² x 220

P = 0.0656 W

**P = approximately 66 mW**

The selected resistor rating is 0.25W.

| Resistor | Estimated Operating Power | Power Rating |
|----------|---------------------------|--------------|
| 220 Ohms | 66 mW | 250 mW |
| 1k Ohms | 25 mW maximum at 5V | 250 mW |
| 10k Ohms | 2.5 mW maximum at 5V | 250 mW |

The selected 1/4W resistors provide adequate power margin for the intended circuit.

### Pull-up Resistor Current

Each 10k sensor pull-up resistor has a maximum approximate current of:

I = V / R

I = 5 / 10,000

**I = 0.5 mA**

The combined maximum current for both sensor pull-ups is approximately 1 mA.

---

## Analog Signal Filtering

C9 and C11 are 0.1 uF capacitors connected between the sensor collector signals and ground.

Together with the 10k pull-up resistors, they provide approximate RC filtering.

### RC Time Constant

**tau = R x C**

tau = 10,000 x 0.0000001

tau = 0.001 seconds

**Time Constant = 1 ms**

### Approximate Cutoff Frequency

**fc = 1 / (2 x pi x R x C)**

fc = 1 / (2 x pi x 10,000 x 0.0000001)

**fc = approximately 159 Hz**

| Parameter | Value |
|-----------|-------|
| Resistance | 10k Ohms |
| Capacitance | 0.1 uF |
| Time constant | 1 ms |
| Approximate cutoff frequency | 159 Hz |

The filtering capacitors help reduce high-frequency noise and short-duration voltage fluctuations.

The actual response depends on phototransistor conductance and ADC loading. Testing is required to ensure the filtering does not delay detection excessively.

---

## Analog-to-Digital Conversion

The PIC18F57Q43 uses a 12-bit ADC to convert analog sensor voltages into digital values.

RA0 reads the left sensor.

RA1 reads the right sensor.

Assuming a 5V ADC reference:

**ADC Value = (V_input / 5) x 4095**

| Input Voltage | ADC Value |
|---------------|-----------|
| 0V | 0 |
| 1V | 819 |
| 2V | 1638 |
| 2.5V | 2048 |
| 3V | 2457 |
| 4V | 3276 |
| 5V | 4095 |

The firmware will compare the measured values with calibrated thresholds to determine whether the robot is approaching a cliff.

The final thresholds will be determined during breadboard and hardware testing.

---

## Status LEDs

Three LEDs provide visual feedback during system operation.

| LED | Color | Connection | Function |
|-----|-------|------------|----------|
| D1 | Green | +5V through R1 | System power |
| D5 | Yellow | RA5 through R7 | Left cliff detected |
| D4 | Red | RA6 through R4 | Right cliff detected |

Each LED uses a 1k Ohm resistor.

Assuming an LED forward voltage of 2V:

I = (5 - 2) / 1000

**I = 3 mA per LED**

| Operating Condition | Green LED | Yellow LED | Red LED |
|---------------------|-----------|------------|---------|
| Power On | ON | OFF | OFF |
| Left Cliff | ON | ON | OFF |
| Right Cliff | ON | OFF | ON |
| Both Cliffs | ON | ON | ON |

The green LED operates independently of the firmware.

The yellow and red LEDs are controlled by the microcontroller.

---

## Debugging Pushbutton

SW1 provides a manual debugging input connected to RA7.

R10 is a 10k Ohm pull-up resistor connected to +5V.

```text
        +5V
         |
     R10 10k
         |
RA7 -----+
         |
        SW1
         |
        GND
```

| Button State | Voltage at RA7 | Logic |
|--------------|----------------|-------|
| Released | Approximately 5V | HIGH |
| Pressed | Approximately 0V | LOW |

When the button is released, the pull-up resistor keeps RA7 HIGH.

When the button is pressed, RA7 is connected to ground and reads LOW.

Maximum pull-up current when pressed:

I = 5 / 10,000

**I = 0.5 mA**

RA7 will be configured as a digital input. Software debouncing will be implemented if necessary.

---

## Team Bus and Ribbon Connector

J2 is a 2x4, 8-pin ribbon connector used to communicate with other Project OpenField subsystems.

| J2 Pin | Signal | Nano Connection |
|--------|--------|-----------------|
| 1 | UART RX, planned | RD2 |
| 2 | UART TX, planned | RD1 |
| 3 | Digital | Unused |
| 4 | Digital | Unused |
| 5 | Digital | Unused |
| 6 | Analog | Unused |
| 7 | Analog | Unused |
| 8 | GND | Common ground |

The UART communication assignments are planned to follow the team interface convention.

RD1 and RD2 will require configuration using Peripheral Pin Select (PPS).

The transmit and receive connections, baud rate, and communication protocol must be confirmed with the team.

All connected boards must share a common ground.

**Important:** The Cliff Detection subsystem operates at 5V. If another subsystem operates at 3.3V, signal voltage compatibility must be verified. Level shifting may be required before connecting the boards.

---

## Power Budget

The estimated power consumption of the subsystem is calculated from its individual circuit loads.

| Component | Quantity | Estimated Total Current |
|-----------|----------|-------------------------|
| TCRT5000 IR Emitters | 2 | 34.54 mA |
| Sensor Pull-ups | 2 | 1.00 mA |
| Status LEDs | 3 | 9.00 mA |
| Pushbutton Pull-up | 1 | 0.50 mA |
| Curiosity Nano | 1 | TBD |
| External Circuit Total | | 45.04 mA |

The estimated external circuit current is 45.04 mA when both emitters and all three LEDs are active and the pushbutton is pressed.

The actual total system current must also include the Curiosity Nano and any connected interface loads.

### External Circuit Power

**P = V x I**

P = 5 x 0.04504

**P = 0.225 W**

The estimated external circuit power consumption is approximately 225 mW, excluding the microcontroller and voltage regulator losses.

The complete power budget will be verified using current measurements.

---

## Electrical Efficiency

The LM7805 is a linear voltage regulator.

It reduces the external input voltage to 5V and dissipates the excess voltage as heat.

Ignoring the regulator's quiescent current, the idealized efficiency is:

**Efficiency = (V_output / V_input) x 100%**

| Input Voltage | Output Voltage | Idealized Efficiency |
|---------------|----------------|----------------------|
| 7.5V | 5V | 66.7% |
| 9V | 5V | 55.6% |
| 12V | 5V | 41.7% |

A lower suitable input voltage reduces the amount of power lost as heat.

### Example Power Calculation

Assume:

- Input voltage = 9V
- Output voltage = 5V
- Example total load current = 100 mA

Input Power:

P_input = 9 x 0.1

P_input = 0.9W

Output Power:

P_output = 5 x 0.1

P_output = 0.5W

Regulator Power Loss:

P_loss = (9 - 5) x 0.1

P_loss = 0.4W

Efficiency:

Efficiency = (0.5 / 0.9) x 100%

**Efficiency = 55.6%**

This is an illustrative calculation, not a measured system result. Actual regulator efficiency will be lower when quiescent current is included.

### Efficiency Improvements

The system could improve energy efficiency by:

- Activating the infrared emitters only during measurements.
- Sampling the left and right sensors sequentially.
- Reducing unnecessary LED operating time.
- Using microcontroller low-power modes.
- Selecting a suitable lower input voltage.
- Replacing the linear regulator with a switching regulator in a future revision.

The current design prioritizes a simple regulated power supply and straightforward debugging.

---

## System Operation

The planned operating sequence is:

1. External DC power enters through J1.
2. The fuse provides overcurrent protection.
3. The LM7805 produces regulated +5V.
4. The green LED indicates system power.
5. The Curiosity Nano initializes its GPIO and ADC peripherals.
6. RA3 and RA4 activate the infrared emitters.
7. RA0 and RA1 read the sensor collector voltages.
8. The firmware compares the ADC readings with calibrated thresholds.
9. The yellow or red LED activates if a potential cliff is detected.
10. The detection status is communicated to the MASTER board.
11. The system repeats the sensing process.

### Detection Logic

| Left Sensor | Right Sensor | Intended Response |
|-------------|--------------|-------------------|
| Ground | Ground | Continue normal operation |
| Cliff | Ground | Yellow LED and left cliff status |
| Ground | Cliff | Red LED and right cliff status |
| Cliff | Cliff | Both LEDs and hazard status |

The robot's motion controller should respond to a confirmed hazard by stopping or avoiding the drop-off.

---

## Design Notes

- All resistors are rated at 1/4W.
- The input fuse is rated at 250mA.
- The fuse uses two Keystone 3512 clips for a 5x20mm cartridge.
- The system operates from a regulated 5V supply.
- C9 and C11 are rated at 16V.
- C5 and C6 should have voltage ratings appropriate for the final supply voltage.
- Sensor filtering capacitors should be placed near the analog input connections.
- Regulator bypass capacitors should be placed near the LM7805.
- Analog traces should be kept away from noisy digital signals.
- A common ground plane should be used when possible.
- The Curiosity Nano footprint must match the actual development board.
- The TCRT5000 footprints must match the physical sensor terminal positions.
- The sensors must be oriented toward the ground.
- Sensor mounting height must be verified experimentally.
- UART logic voltage compatibility must be checked with the team.

---

## Electrical Rules Check

The latest documented KiCad ERC results are:

| Check | Result |
|-------|--------|
| Errors | 0 |
| Warnings | 2 |

The remaining warnings are related to differences between the custom Curiosity Nano and LM7805 schematic symbols and their saved library versions.

The custom symbols and footprints must be verified before PCB manufacturing.

A successful ERC does not independently verify current limits, mechanical fit, thermal performance, or firmware behavior.

---

## Breadboard Verification

The following tests are planned to verify the operation of the Cliff Detection subsystem.

| Test | Expected Result | Status |
|------|-----------------|--------|
| LM7805 Output | Approximately 5V | Planned |
| Green LED | Illuminates with regulated power | Planned |
| Left Sensor | ADC voltage changes with reflection | Planned |
| Right Sensor | ADC voltage changes with reflection | Planned |
| Left Cliff Detection | Yellow LED activates | Planned |
| Right Cliff Detection | Red LED activates | Planned |
| Pushbutton | RA7 changes HIGH to LOW | Planned |
| UART Communication | Correct data transmitted and received | Planned |
| Current Consumption | Within system power budget | Planned |
| Regulator Temperature | Within safe operating limits | Planned |

### Testing Procedure

1. Verify the regulator output voltage before connecting the Nano.
2. Confirm that VTG receives regulated 5V.
3. Verify VOFF and common ground connections.
4. Test each infrared emitter independently.
5. Measure the collector voltage at RA0 and RA1.
6. Test multiple ground colors and surface materials.
7. Test different sensor mounting heights.
8. Determine reliable cliff detection thresholds.
9. Verify the status LED responses.
10. Test the ribbon cable communication interface.
11. Measure total system current and regulator temperature.

Measured voltages, current consumption, and calibrated ADC thresholds will be recorded after hardware testing.

---

## Resources

- [Bill of Materials](../03-BOM/BOM.md)
- [Download Cliff Detection BOM Excel File](../03-BOM/Cliff_Detection_BOM_Revised.xlsx)
- [Complete Schematic PDF](Newskimcliff.pdf)
- [PIC18F57Q43 Curiosity Nano – Microchip](https://www.microchip.com/en-us/development-tool/DM164150)
- [TCRT5000 Datasheet – Vishay](https://www.vishay.com/docs/83760/tcrt5000.pdf)
- [LM7805 Voltage Regulator Datasheet](https://www.ti.com/lit/ds/symlink/lm340.pdf)
- [RAS304 Embedded Systems Course](https://ras-embedded-systems.pages.dev/304/)
