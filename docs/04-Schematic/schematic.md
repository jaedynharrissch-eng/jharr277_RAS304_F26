# Schematics

## Project OpenField – Cliff Detection Subsystem

**Author:** Jaedyn Harris  
**Course:** RAS304 – Embedded Systems  
**Team:** Team 1  
**Microcontroller:** PIC18F57Q43 Curiosity Nano  
**Revision:** 2.0  

---

## Overview

This schematic is for my Cliff Detection subsystem board in Project OpenField.

The system uses two TCRT5000 infrared reflective sensors to monitor the ground beneath the left and right sides of the robot. The PIC18F57Q43 Curiosity Nano reads the analog sensor signals, determines whether a potential cliff or drop-off is present, and communicates the detection status to the other team subsystems.

The board includes:

- External DC power input.
- LM7805 regulated 5V power supply.
- Two TCRT5000 infrared reflective sensors.
- Analog signal filtering.
- Green, yellow, and red status LEDs.
- Debugging pushbutton.
- 8-pin ribbon cable communication interface.

[![Cliff Detection Schematic](Newskimcliff-1.png)](Newskimcliff-1.png)

*Figure 1: Complete Cliff Detection subsystem schematic designed in KiCad. Click the image to enlarge.*

[Download High-Resolution Schematic PDF](Newskimcliff.pdf)

---

## Schematic Sections

| Section | Description | Key Components |
|---------|-------------|----------------|
| Power Input | Receives external DC power and provides overcurrent protection. | J1, F3 |
| Voltage Regulation | Converts external voltage into regulated +5V power. | JH1, C5, C6 |
| Microcontroller | Processes sensor readings, controls LEDs, and communicates with other subsystems. | U1 PIC18F57Q43 |
| Left Cliff Sensor | Detects reflected infrared light beneath the left side of the robot. | U3, R8, R11, C9 |
| Right Cliff Sensor | Detects reflected infrared light beneath the right side of the robot. | U4, R9, R12, C11 |
| Status Indicators | Displays power and cliff detection conditions. | D1, D4, D5, R1, R4, R7 |
| Debugging | Provides manual input for testing and troubleshooting. | SW1, R10 |
| Team Communication | Connects the Cliff Detection subsystem to the team interface. | J2, RD1, RD2 |

---

## Microcontroller Pin Map

| Signal | Nano Pin | Symbol Pin Number | Direction | Connection |
|--------|----------|-------------------|-----------|------------|
| LEFT_ADC | RA0 | 42 | Analog Input | U3 collector |
| RIGHT_ADC | RA1 | 41 | Analog Input | U4 collector |
| LEFT_IR | RA3 | 16 | Digital Output | R8 and U3 anode |
| RIGHT_IR | RA4 | 17 | Digital Output | R9 and U4 anode |
| LEFT_LED | RA5 | 37 | Digital Output | R7 and yellow LED |
| RIGHT_LED | RA6 | 36 | Digital Output | R4 and red LED |
| BUTTON | RA7 | 35 | Digital Input | SW1 and R10 |
| UART_RX | RD2 | 22 | Input, planned | J2 pin 1 |
| UART_TX | RD1 | 21 | Output, planned | J2 pin 2 |
| VTG | VTG | 34 | Power Input | Regulated +5V |
| VOFF | VOFF | 30 | Control Input | GND |
| VBUS | VBUS | 29 | USB Power | Not connected |
| GND | GND | 15, 24, 33, 43, 52 | Ground | Common ground |

The symbol pin numbers correspond to the custom Curiosity Nano symbol and carrier PCB footprint, not the physical pin numbers of the PIC18F57Q43 integrated circuit.

The UART assignments are planned and will require Peripheral Pin Select (PPS) configuration in MPLAB X.

### Complete Nano Pin Reference

| Port | Pin Names and Symbol Numbers |
|------|------------------------------|
| Port A | RA0:42, RA1:41, RA2:40, RA3:16, RA4:17, RA5:37, RA6:36, RA7:35 |
| Port B | RB0:39, RB1:10, RB2:9, RB3:38, RB5:47 |
| Port C | RC0:56, RC1:55, RC2:7, RC3:8, RC4:11, RC5:12, RC6:13, RC7:19 |
| Port D | RD0:20, RD1:21, RD2:22, RD3:23, RD4:14, RD5:46, RD6:45, RD7:44 |
| Port E | RE0:54, RE1:53, RE2:18 |
| Port F | RF0:25, RF1:26, RF2:27, RF3:28, RF4:51, RF5:50, RF6:49, RF7:48 |
| Debug | DBG0:32, DBG1:5, DBG2:6, DBG3:31 |
| Serial Debug | CDC_RX:3, CDC_TX:4 |
| Power | VBUS:29, VOFF:30, VTG:34 |
| Ground | 15, 24, 33, 43, 52 |
| Other | NC:1, ID:2 |

Unused GPIO pins are not connected to the current subsystem.

The custom footprint numbering and physical connector layout must be verified before manufacturing.

---






The schematic as a PDF download is available [*here*](Newskimcliff.pdf), and the Zip folder of the project [*here*](dummyZip.zip).
