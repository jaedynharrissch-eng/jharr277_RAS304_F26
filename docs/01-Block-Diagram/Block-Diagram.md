---
title: Individal Block Diagram
tags:
- tag1
- tag2
---


## Overview

The Cliff Detection Subsystem is designed to prevent the robot from driving off elevated surfaces. It uses two TCRT5000 reflective infrared sensors connected to a Microchip PIC18F57Q43 Curiosity Nano.

The microcontroller monitors reflected infrared light from the ground and communicates potential cliff hazards to the master controller.

## Power Source

- 3S Li-ion battery (9–12.6 V) or 9 V bench adapter
- J1 barrel jack
- 750 mA fuse (proposed)
- LM2940CT-5.0 voltage regulator
- Regulated 5 V power supply
- USB connection for programming and debugging

The Curiosity Nano's external power connection must be configured according to Microchip's hardware requirements.

## Controller

**Microchip PIC18F57Q43 Curiosity Nano**

| Pin | Function |
|---|---|
| RA0 / ANA0 | Left TCRT5000 analog input |
| RA1 / ANA1 | Right TCRT5000 analog input |
| RD0 | Left infrared LED control |
| RD1 | Right infrared LED control |
| RB0 | UART2 transmit (proposed) |
| RB1 | UART2 receive (proposed) |
| GND | Common ground |

## Sensors

The subsystem uses two TCRT5000 reflective infrared sensors, positioned on the left and right sides of the robot.

Each sensor contains an infrared LED and phototransistor. The LED emits infrared light toward the ground, while the phototransistor detects reflected light.

The sensor outputs are read through the Curiosity Nano's analog-to-digital converter.

## Team Interfaces

The subsystem communicates with the master controller through Connector 4 using UART2.

The ribbon cable carries UART transmit, receive, and common ground connections. Exact connector pin assignments must be confirmed with the team.

## Block Diagram

![Cliff Detection Block Diagram](Individual%20Pin%20diagram.drawio.png)

## Functional Description

Power enters the subsystem through the J1 barrel jack and passes through fuse protection and a 5 V voltage regulator.

The two TCRT5000 sensors continuously monitor reflected infrared light from the ground.

The Curiosity Nano reads the sensor signals through RA0 and RA1 and compares them against calibrated thresholds.

When either sensor detects a possible loss of ground, the Curiosity Nano sends a cliff warning to the master controller through UART2.

The master controller can then command the motor subsystem to stop or change direction.
