
# Cliff Detection System

### RAS 304 | Fall 2026
**Arizona State University**  
**Robotics Engineering and Autonomous Systems**  
**Engineer:** Jaedyn Harris

---

## Welcome

Welcome to my RAS 304 engineering project portfolio!

This website documents the research, design, and development of an **Infrared-Based Cliff Detection System** using the Vishay TCRT5000 reflective optical sensor.

The purpose of this project is to explore how infrared sensing technology can improve the safety and navigation of autonomous robotic systems.

Throughout this portfolio, I will document the engineering design process, including component selection, circuit calculations, system architecture, and electrical schematic development.

---

## Project Overview

Autonomous robots rely on sensors to detect and respond to their surroundings. One important safety challenge is identifying sudden changes in floor elevation, such as stairs, ledges, and drop-offs.

Without an effective detection system, a mobile robot could travel beyond the edge of a supporting surface, potentially damaging its components.

My project focuses on designing a compact and cost-effective infrared cliff detection circuit capable of identifying the presence or absence of a nearby surface.

The system uses the **TCRT5000 reflective infrared sensor**, which contains both an infrared-emitting diode and a phototransistor.

---

## How Does Cliff Detection Work?

The cliff detection system operates using reflected infrared light.

1. **Infrared Emission:** The infrared LED emits light toward the ground beneath the robot.

2. **Surface Reflection:** When a surface is present, some infrared light reflects toward the phototransistor.

3. **Signal Detection:** The phototransistor responds to the reflected light, producing an electrical signal.

4. **Cliff Identification:** When the reflected light decreases below a calibrated threshold, the circuit identifies a possible drop-off.

5. **Robot Response:** The detection output can be used by a robotic controller to initiate a safety response.

> **Design Principle:** A decrease in reflected infrared light can indicate that the supporting surface is no longer present. Detection reliability depends on sensor positioning, surface reflectivity, and calibration.

---

## Project Objectives

The primary objectives of this project include:

- Design an infrared-based cliff detection system.
- Understand the electrical characteristics of the TCRT5000 sensor.
- Calculate appropriate resistor values for the infrared LED and phototransistor circuit.
- Develop a functional system block diagram.
- Select components based on voltage, current, and power requirements.
- Create a complete bill of materials (BOM).
- Design an electrical schematic using KiCad.
- Document engineering decisions and potential design limitations.

---

## System Components

| Component | Purpose |
|-----------|---------|
| TCRT5000 Sensor | Detects reflected infrared light |
| Infrared LED | Emits infrared light toward the surface |
| Phototransistor | Responds to reflected infrared light |
| Current-Limiting Resistor | Limits current through the infrared LED |
| Load Resistor | Converts phototransistor current into a voltage signal |
| Power Supply | Provides electrical power to the circuit |
| Output Interface | Provides a signal for further processing |

---

## Engineering Design Documentation

The following sections document each stage of the project.

### 01 - Block Diagram
Illustrates the system architecture, component relationships, and signal flow.

### 02 - Component Selection
Explains the selection of electronic components and their electrical specifications.

### 03 - Bill of Materials
Provides component quantities, manufacturer information, and purchasing references.

### 04 - Electrical Schematic
Documents the circuit design and electrical connections using KiCad.

---

## Potential Applications

Infrared cliff detection systems can be used in:

- Autonomous mobile robots
- Robotic vacuum cleaners
- Educational robotics platforms
- Indoor service robots
- Robotic navigation and safety systems

---

## About Me

I am a Robotics Engineering and Autonomous Systems student at **Arizona State University**.

My engineering interests include robotics, autonomous navigation, embedded electronics, mechanical design, and electromechanical systems.

Through this project, I aim to strengthen my understanding of sensor integration, electrical circuit design, and the engineering documentation process.

---

## Project Information

| Category | Details |
|----------|---------|
| University | Arizona State University |
| Course | RAS 304 |
| Semester | Fall 2026 |
| Project | Infrared Cliff Detection |
| Primary Sensor | Vishay TCRT5000 |
| Design Software | KiCad |
| Documentation | GitHub Pages |

---

**Developed by Jaedyn Harris**  
*Arizona State University — Fall 2026*
