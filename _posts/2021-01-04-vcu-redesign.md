---
title:  "VCU Redesign"
mathjax: true
layout: post
categories: electronics
---

![VCU](/assets/img/product.png)


During the past months, I have been working on a vehicle control unit (VCU) for the Formula Student Team of my university. This article gives a few practical tips to people that are starting with PCB design in Altium.


## Hierarchical Design

The easy approach to schematic design is "Larger is Better!". In Altium you can scale up your schematics to A3 and larger. This gives you plenty of room to build anything you want. While this might work you probably want to design hardware like software in a modular fashion. I noticed this in particular during this project. Routing a single microcontroller (MCU) was fine, but adding various components like low-dropout regulators (LDOs) started making the schematic confusing. Therefore, I highly recommend isolating your schematic into layers. Hierarchical design is probably the best approach for this. Figure 1 shows the essential components inside my VCU design.

The Main MCU is flashed with the control system like a model predictive control (MPC). The Assist MCU is used to reset and control the Main MCU. Furthermore, it contains fairly good analog-digital converts (ADCs). The supply is realised through multiple LDOs. Analog and digital supplies are separated from each other to increase precision and reduce noise. This also enables easy GND separation for multiple polygons. In terms of connectors, Harwin connectors work great and are reliable. 

![Transformer](/assets/img/blocks.png)
<small><center>Figure 1: Hierarchical Design</center></small>

## LDOs

LDOs are linear voltage regulators that can regulate DC voltages when supply and output voltage are very close to each other. The DC converter has no switching noise and therefore ideal to provide high accuracy voltage levels. Figure 2 shows an LDO within the VCU schematics.

![LDO](/assets/img/ldo.png)
<small><center>Figure 2: LDO</center></small>

## Filters

Good and reliable data can be really helpful. Unfortunately, sensors with analog outputs like pressure sensors and load cells contain noise. A differential filter is shown in Figure 3. Note that this filter also provides access to various source voltages depending on the assembly. Therefore, active or inactive (impedance change) sensors can be used. The capacitors in the schematic are primarily used as decoupling capacitors. Resistors are used as voltage dividers. The BAT54S diodes are used to prevent surge voltages from going to the Assist MCU. Voltage levels at ADC_0_P and ADC_0_N are clamped to levels between 0V and 5V. In terms of PCB routing differential signals should always be routed close to each other. In Altium you can set markers inside the schematics to ensure close routing distances.

![Filter](/assets/img/filter.png)
<small><center>Figure 3: Differential Filter</center></small>

## CAN Transceivers

CAN is one of the most common standards in modern cars. CAN-FD allows higher throughput by creating larger data frames. CAN can be accessed through common connectors in the automotive sector like OBD2. Since signal lines usually experience electromagnetic interference using differential pairs is essential. In the automotive sector these are labelled using CANH and CANL. To convert from the digital CAN controller output to the differential pair a CAN transceiver [2] is used. Debugging of CAN signals can be carried out through CANoe a software package by Vector. Alternatively, comma.ai offers an open source alternative. The structure of a CAN data frame is provided by .dbc files. 

## Testing

Although you can easily add a JTAG connector to your board it is often helpful to create a simple development board. The flash procedure can be carried out through SAM-BA [1] via USB. The procedure needs to be initiated via a control signal to the Assist MCU.

## Routing

Use throug-hole vias since these are usually cheaper than burried vias. Often manufacturers cannot even produce burried vias. In terms of routing try to use directional routes in each layer. For instance, using the second layer for horizontal and the third layer for vertical routes can be really helpful in simplifying the schematic. If you use multiple layers it is helpful to use the outer layers for GND and AGND (if you have a separate analog ground) and the inner layers for voltage levels like 3.3V and 5V. GND polygons can be further separated for debugging purposes. Decoupling capacitors should always be placed closely to the corresponding pins. After finishing most of your PCB design use silkscreens to label test LEDs and test points. 

![Routing](/assets/img/routing-1.png)
<small><center>Figure 4: Routing</center></small>


## References
1. [https://github.com/atmelcorp/sam-ba](https://github.com/atmelcorp/sam-ba)
2. [https://www.analog.com/media/en/technical-documentation/application-notes/AN-1123.pdf](https://www.analog.com/media/en/technical-documentation/application-notes/AN-1123.pdf)