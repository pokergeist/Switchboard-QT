# Switchboard QT Hardware

## Component Overview

* Processing, Control & Comms

  * footprint for optional QT Py [microcontroller](#microcontroller)
  * external control via Qwiic/Stemma-QT connectors
  * Jumper Block
  * Reset switch
* Relay Hardware
  * [GPIO Expander](#gpio-expander) IC
  * [Relay Driver](#relay-driver) ICs
  * DPDT latching [relays](#relays)
* [Power Monitor](#power-monitor)
* Discrete Components
  * [Resistors](#resistors)
  * [Capacitors](#capacitors)
  * Reset Switch
* Interconnects
  * Qwiic/Stemma-QT connectors
  * Terminal Blocks for Relays & Power Sensor
  * Header for select pin access
    * 5V & GND
    * GPA5-GPA7, GPB5-GPB-7
    * under consideration:
      * /RESET
      * /INT
      * QT Py A0-A2, SPIx3, TX.RX
  * Cut/solder jumpers for:
    * I2C addresses (A0, A1). Grounded, cut to pull high.
    * I2C Vcc connection to QT Py MCU. Cut to isolate QT Py voltage regulator from Qwiic Vcc.

---

## Control Elements

### Microcontroller

The board is prepared to accept an Adafruit [QT Py microcontroller](https://www.adafruit.com/product/4600), power via USB-C or the 5V jumper.

Alternatively, external control and power can be provided via the (2) Qwiic/Stemma-QT connectors. Additional sensor modules can be connected as well.

QT Py Notes:

* 5V power can be provided via USB-C cable or header JP1.
  * The 5V input is only used to power the QT Py's 3.3V voltage regulator.
* The QT Py provides a third Qwiic connector.
* All components and the Qwiic Vcc line are power by the QT Py's voltage regulator (up to ~500mA) by default.
* The /RESET line is not exposed off-board. A SAMD21 (#4600) QT Py's SWD and /RESET pads are accessible via through-hole solder pads. Therefore the RESET button will reset the GPIO Expander IC and perhaps the MCU. The state of the Relays will not be changed, and cannot be examined. The MCU may choose to reset the relays to a known state or preserve the current state.

### Switches

| Location |            Type             | Uses                                                         |
| :------: | :-------------------------: | ------------------------------------------------------------ |
|  RESET   | Momentary pushbutton switch | Resets the GPIO Expander, and the MCU if the RST pad is connected. **Note: the relays latch so they do not reset by default. There is no way to determine their state unless it is saved to EEPROM or NVRAM.** |

### I2C Notes

* I2C pullup resistors (1kΩ) are provided but can be changed or removed depending on other connected I2C modules.
* If I2C Vcc power is provided off-board and there is a QT Py MCU in place, the Vcc cut jumper **should be cut** to decouple that voltage from the QT Py's 3.3V voltage regulator output.
* Address values for the I2C connected ICs can altered using the A0 & A1 cut jumpers such that (4) Switchboard QT boards could be connected via Qwiic cabling.

## GPIO Expander

**TI [PCA9539DW](https://www.digikey.com/short/zd08w70j)**

The 16-channel GPIO Expander adds (10) GPIO lines to control the Set and Reset coils of the 5 double-pole relays.

* Access via I2C
* Extra lines:
  * (6) GPIO lines are accessible via the header JP1
  * the /INT line is routed to a QT Py input. **FIXME** - route to header?

**GPIOX Notes**:

* The PCA9539 does not provide pull-up or pull-down resistors for lines designated as inputs.
* The GPIO lines to the Relay Drivers are only held high long (~15ms) to latch the relay in the desired state, then dropped. Therefore the state of the relays cannot be assessed after resetart.
* The relay coils draw about 51mA so energizing all 5 relays at once will draw ~257mA of the 500mA available from the QT Py. (The driver should prevent concurrent use of the SET and RESET coils for any one relay.)
* MCP20017 comparison:
  * larger package, 28-pin vs. 24
  * GPx7 pins are output only
  * has /INTA & B pins
  * only available in DIP package for now

## Relay Driver

**ST Micro [ULN2003D1013TR](https://www.digikey.com/short/wzfmzvjh)**

The 7-channel relay drivers provide low-side switching of up to 500mA per channel via a Darlington pair (NPN) arrangement. They also provide snubber diodes to eliminate inductive flyback voltage. Replaces a TI part.

## Relays

**[EE2-3TNUH(-L)](https://www.digikey.com/short/3hfz4vbc)**

There are (5) double-pole, double-throw (DPDT) latching relays. The Set or Reset control activates both relays in the device to switch the connection between the common (CO) contact and the normally open NO or normally closed (NC) contacts respectively.

## Power Monitor

**[INA219BIDR](https://www.digikey.com/short/9qj5h54z)**

This is a slight upgrade from the INA219A. It monitors voltage, current, and power through the V+ and V- contacts. Here's the [writeup](https://www.adafruit.com/product/904) on Adafruit's module.

## Resistors

All 1206 (3216 metric) size.

| Value | Tolerance | Power | Location | Notes                     |
| :---: | :-------: | :---: | :------: | ------------------------- |
| 0.1Ω  |    1%     | 1/2W  |   R11    | Current sense             |
| 3.3kΩ |    5%     | 1/4W  | R21 R22  | A0 & A1 address pull-ups  |
| 10kΩ  |    5%     | 1/4W  |   R23    | /INT pull-up              |
| 1.0kΩ |    5%     | 1/4W  | R31 R32  | I2C clock & data pull-ups |
| 100k  |    5%     | 1/4W  |   R41    | /RESET pull-up            |
|  100  |    1%     | 1/4W  |   R42    | Reset cap current limiter |
|  75   |    1%     | 1/4W  |   R43    | LED current limiter       |

##  Capacitors

All 1206 (3216 metric) size, X7R rated.

| Value | Tolerance | Location | Notes            |
| :---: | :-------: | :------: | ---------------- |
| 0.1µF |    10%    | C11 C12  | IC filter caps   |
| 0.1µF |    10%    |   C41    | Reset timing cap |

