# The Switchboard QT Project

## Overview

The Switchboard QT project allows a user to control (5) DPDT latching relays with either an onboard Adafruit QT Py microcontroller or and external micro via a Qwiic/STEMMA QT cable which providing I2C and power.

The Switchboard QT board also hosts a TI INA219 power monitor (voltage, current, power).

There are two Qwiic/STEMMA QT connectors plus one on the QT Py micro.

A jumper header that accepts 5V power and provides access to unused GPIO lines.

## Lineage

The Switchboard QT project is a remix of the [ATMakers Switchboard](https://github.com/ATMakersOrg/ATMakers-Hardware/tree/master/SwitchBoard) FeatherWing project that met with some fabrication and environmental noise issues.

## Status

A tested driver for the new GPIOX has been [located](https://github.com/sumotoy/gpio_expander) though I think a few tweaks are needed.  Hardware components are at risk of supply chain issues.

Production is awaiting feedback expressing interest in the product as-is.

Other variants may use different number or type of relay, perhaps solid state versions, and utilize SDA & SCL for an extended addressing scheme.

## Hardware

On-board hardware is detailed [here](./hardware/Switchboard-QT-Hardware.md).

## Software

Software will be uploaded once developed and tested with the fabricated boards.

## Options Under Consideration

* Expand the address range of the GPIOX and Power Monitor ICs. Allow SDA and SCL to be connected to A0 & A1 (increasing board addresses from 4 to 16).
