> **A note on RX/TX inversion:** Serial signals (like those used for the SiK radio and GPS) send signals from TX and listen to signals on RX. This means, for two devices to communicate properly, their RX and TX lines must be crossed (TX to RX and RX to TX instead of TX to TX and RX to RX). Some devices flip the labels on their RX and TX in a misguided attempt to simplify assembly. Fortunately, the devices will not be damaged by getting the wires wrong, so trying one configuration and then the other is not an issue. 

## Radio

##### RCIN Plug - Handheld radio controller

> Note: This should be handled by a 3-wire Dupont servo connector

| Pin                        | Signal  | Volt | Connected to      |
|----------------------------|---------|------|-------------------|
| RCIN GND (brown/black)     | GND     | GND  | Receiver SBUS GND |
| RCIN VCC (red)             | VCC +5V | +5V  | Receiver SBUS VCC |
| RCIN Signal (yellow/white) |         |      | Receiver SBUS GND |

##### Telem 1 Plug - SiK radio connection

| Pin     | Signal   | Volt  | Connected to |
|---------|----------|-------|--------------|
| 1 (red) | VCC      | +5V   | SiK VCC      |
| 2 (blk) | TX (OUT) | +3.3V | SiK RX       |
| 3 (blk) | RX (IN)  | +3.3V | SiK TX       |
| 4 (blk) | CTS      | +3.3V | None         |
| 5 (blk) | RTS      | +3.3V | None         |
| 6 (blk) | GND      | GND   | SiK GND      |

## GPS

##### GPS 1 Plug - Global Positioning

> Note: Depending on the exact model of GPS and Pixhawk, there may be a ready made plug.  
> Additionally, the GPS may have an I2C cable broken out

| Pin     | Signal   | Volt  | Connected to    |
|---------|----------|-------|-----------------|
| 1 (red) | VCC      | +5V   | GPS VCC (red)   |
| 2 (blk) | TX (OUT) | +3.3V | GPS RX (orange) |
| 3 (blk) | RX (IN)  | +3.3V | GPS TX (yellow) |
| 4 (blk) | CAN2 TX  | +3.3V | None            |
| 5 (blk) | CAN2 RX  | +3.3V | None            |
| 6 (blk) | GND      | GND   | GPS GND (black) |

##### I2C Plug - Compass heading

> Note: Power is not needed because the compass gets power from the GPS port

| Pin     | Signal | Volt           | Connected to            |
|---------|--------|----------------|-------------------------|
| 1 (red) | VCC    | +5V            | None                    |
| 2 (blk) | SCL    | +3.3 (pullups) | GPS Compass SCL (green) |
| 3 (blk) | SDA    | +3.3 (pullups) | GPS Compass SDA (black) |
| 4 (blk) | GND    | GND            | None                    |

## Power

##### Power Plug

> Note: Power should be handled by a single plug that comes from the power converter. This is provided only for reference.

| Pin     | Signal  | Volt        |
|---------|---------|-------------|
| 1 (red) | VCC     | +5V         |
| 2 (blk) | VCC     | +5V         |
| 3 (blk) | CURRENT | up to +3.3V |
| 4 (blk) | VOLTAGE | up to +3.3V |
| 5 (blk) | GND     | GND         |
| 6 (blk) | GND     | GND         |

## Chassis and VBat

The S550 platform uses a PCB as the frame of the drone. This means that the primary power leads are soldered directly to the frame itself. Power then flows to pads on the surface of the frame. The following items are electrically connected to the battery through the frame of the drone. Each has just a positive (\~16V) connection and a ground:

- 6X Motor controllers (ESCs)
- 1 pair Dupont connectors for VTX quick disconnect
- XT60 lead

## Motors

Standard brushless motors have three wires. The motors included in the kit connect to their respective speed controllers with bullet connectors. During configuration, if any of the motors run in the wrong direction,  two of the 3 motor cables should be swapped to reverse the motor direction. The power leads of the ESCs can then be soldered directly to the VBat pads on the frame.

The ESCs receive commands in the form of PWM from the Pixhawk motor outputs. It is important that each motor is plugged into its designated input (1-6) per the Arducopter diagram. For control, the ESCs only require a signal wire and a ground connection. Dupont style servo cables are ideal for this use case because they fit perfectly into most Pixhawk flight controllers. 