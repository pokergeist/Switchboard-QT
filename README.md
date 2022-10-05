# The Switchboard QT Project

## Overview

The Switchboard QT project allows a user to control (5) DPDT latching relays with either an onboard Adafruit QT Py microcontroller or and external micro via a Qwiic/STEMMA QT cable which providing I2C and power.

The Switchboard QT board also hosts a TI INA219 power monitor (voltage, current, power).

There are two Qwiic/STEMMA QT connectors plus one on the QT Py micro.

A jumper header that accepts 5V power and provides access to unused GPIO lines.

## Lineage

The Switchboard QT project is a remix of the [ATMakers Switchboard](https://github.com/ATMakersOrg/ATMakers-Hardware/tree/master/SwitchBoard) FeatherWing project that met with some fabrication and environmental noise issues.

## Status

Component selection is still underway for the GPIO Expander. Drivers for the new GPIOX will have to be integrated or written and tested. Other components are at risk of supply chain issues.

## Hardware

On-board hardware is detailed [here](./hardware/Switchboard-QT-Hardware.md).

## Software

Software will be loaded once developed and tested with the fabricated boards.

