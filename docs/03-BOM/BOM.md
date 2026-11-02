---
title: Bill of Materials
tags:
- tag1
- tag2
---


## 2. Bill of Materials and Pricing

**Project:** RAS 304 – Cliff Detection  
**Supplier:** DigiKey / Other Distributors  
**Currency:** USD

| Reference | Component | Manufacturer Part Number | Qty | Unit Price | Total |
|---|---|---|---:|---:|---:|
| U1, U2 | TCRT5000 IR Sensor | TCRT5000 | 2 | $1.33* | $2.66 |
| R1, R3 | 220 Ohm Resistor, 0805 | CRCW0805220RFKEA | 2 | $0.10 | $0.20 |
| R2, R4 | 10k Ohm Resistor, 0805 | CRCW080510K0JNEA | 2 | $0.10 | $0.20 |
| **TOTAL** | | | **6** | | **$3.06** |

*The TCRT5000 price is a reference distributor
price, not a confirmed current DigiKey USD quote.*

### Supplier Links

- [TCRT5000 Sensor – DigiKey](https://www.digikey.com/en/products/detail/vishay-semiconductor-opto-division/TCRT5000/1681167)
- [220 Ohm Resistor – DigiKey](https://www.digikey.com/en/products/detail/vishay-dale/CRCW0805220RFKEA/1175559)
- [10k Ohm Resistor – DigiKey](https://www.digikey.com/en/products/detail/CRCW080510K0JNEA/541-10KACT-ND/1180364)

## 3. Cost Summary

| Category | Cost |
|---|---:|
| Infrared Sensors | $2.66 |
| 220 Ohm Resistors | $0.20 |
| 10k Ohm Resistors | $0.20 |
| **Estimated Subtotal** | **$3.06** |

Shipping, taxes, PCB fabrication, and any
additional connectors are not included.

## 4. Hardware Ordering Notes

The design requires two complete sensing
circuits, each containing one TCRT5000,
one 220 Ohm resistor, and one 10k Ohm resistor.

The quantities above represent components
installed in the final circuit. Additional
components may be purchased as spares.

Resistor package size and footprints must
match the final KiCad PCB design.
