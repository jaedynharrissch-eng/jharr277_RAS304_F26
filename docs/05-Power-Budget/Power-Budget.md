
# 05 - Power Budget

## Overview

The purpose of this power budget is to estimate the electrical power required to operate my TCRT5000 infrared cliff detection circuit.

A power budget is important because it verifies that the selected power supply can deliver sufficient current to all components without exceeding their electrical ratings.

My cliff detection system uses a TCRT5000 reflective optical sensor, consisting of an infrared LED and a phototransistor.

The calculations below estimate current consumption and power requirements for a single sensor circuit operating from a regulated 5V DC supply.

---

## Electrical Design Parameters

| Parameter | Value |
|-----------|-------|
| Supply Voltage | 5.0 V DC |
| Sensor | Vishay TCRT5000 |
| Infrared LED Forward Voltage (Assumed) | 1.25 V |
| Target LED Current | 20 mA |
| LED Series Resistor | 220 ohms |
| Phototransistor Load Resistor (Example) | 10 kohms |
| Number of Sensors | 1 |

**Note:** The resistor values and operating currents must be verified against the final KiCad schematic.

---

## Power Calculations

Electrical power is calculated using:

**P = V x I**

Where:

- P = Power (Watts)
- V = Voltage (Volts)
- I = Current (Amperes)

### 1. Infrared LED Current

The infrared LED requires a series resistor to limit current.

Using Ohm's Law:

**I = (Vs - Vf) / R**

Where:

- Vs = 5.0 V
- Vf = 1.25 V
- R = 220 ohms

**I = (5.0 - 1.25) / 220**

**I = 0.01705 A = 17.05 mA**

### 2. Infrared LED Power

**P = Vf x I**

P = 1.25 x 0.01705

**P = 0.0213 W = 21.3 mW**

### 3. LED Series Resistor Power

**P = I² x R**

P = (0.01705)² x 220

**P = 0.0639 W = 63.9 mW**

A standard 0.25 W resistor provides adequate power dissipation capacity for this example.

### 4. Phototransistor Circuit

The phototransistor current depends on the amount of infrared light reflected from the surface.

Assuming a 10 kohm pull-up resistor:

**Imax = V / R**

Imax = 5 / 10000

**Imax = 0.0005 A = 0.5 mA**

This is the maximum current through the load resistor under the simplified assumption that the phototransistor is fully conducting.

### 5. Maximum Phototransistor Load Resistor Power

**P = V² / R**

P = 5² / 10000

**P = 0.0025 W = 2.5 mW**

---

## Estimated Power Budget

| Component / Branch | Voltage | Current | Power |
|--------------------|---------|---------|-------|
| Infrared LED | 1.25 V | 17.05 mA | 21.3 mW |
| LED Series Resistor | 3.75 V | 17.05 mA | 63.9 mW |
| Phototransistor Load Branch (Maximum) | 5 V | 0.5 mA | 2.5 mW |
| **Total Supply (Maximum)** | **5 V** | **17.55 mA** | **87.8 mW** |

The LED and its series resistor form one current path. Their currents must not be added together when calculating total supply current.

The estimated maximum current is:

**Itotal = 17.05 + 0.5 = 17.55 mA**

The estimated maximum supply power is:

**Ptotal = 5 x 0.01755 = 0.0878 W**

**Total Estimated Power = 87.8 mW**

---

## Design Margin

To provide additional capacity, a 25% design margin is applied.

**Irequired = 17.55 mA x 1.25**

**Irequired = 21.94 mA**

The 5V supply should therefore provide at least approximately 22 mA for the sensor circuit under these assumptions.

This estimate does not include any microcontroller, voltage regulator losses, indicator LEDs, or other external components.

---

## Conclusions

Based on the example calculations, a single TCRT5000 cliff detection circuit is expected to consume a maximum of approximately 87.8 mW from a regulated 5V supply.

The infrared LED branch accounts for most of the power consumption, while the phototransistor sensing branch requires comparatively little current.

The calculated resistor power dissipation is within the rating of a standard 0.25 W resistor.

These results provide a preliminary estimate for power supply selection. Final values must be recalculated using the actual components and verified through circuit testing.

---

## Resources

- [Vishay TCRT5000 - DigiKey](https://www.digikey.com/en/products/detail/vishay-semiconductor-opto-division/TCRT5000/1681167)
- [TCRT5000 Manufacturer Datasheet](https://www.vishay.com/docs/83760/tcrt5000.pdf)

---

**RAS 304 | Arizona State University | Fall 2026**
