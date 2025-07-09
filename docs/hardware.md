# Hardware
Our collaboration partners in Sydney, Australia, designed the hardware. Team Turtle Rabbit competed in the 2024 RoboCup in Eindhoven as a first-time team. The team description paper for the 2024 RoboCup can be found [here](https://arxiv.org/abs/2402.08205).

## Robot Design

### Wheel Design
---
In the design the motor is located within the wheel (yellow part in the middle). The light blue part is the 3D printed wheel chassis printed with PETG. The gray parts in the front and the back are the front and rear covers which are milled from aluminum. The blue part in the back of the wheel is the motor controller. Inbetween the motor controller and the wheel is the motor mount which is also milled from aluminum. 

*Image*

## Electronics Components

### Raspberry Pi 4b
---
A small single-board computer used as the main processing unit for controlling the robot and handling communication with other components.

*Image*

**Company**: Raspberry Pi\
**Model**: Raspberry Pi 4 B, 4x 1,5 GHz, 4 GB RAM, WLAN, BT

**Resources**:
- [Github](https://github.com/raspberrypi)
- [Getting started](https://www.raspberrypi.com/documentation/computers/getting-started.html)
- [Hardware reference](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html)

### Motor Controllers
---
These boards are used to control the BLDC drone motors, providing Field-Oriented Control (FOC) for efficient motor operation, a STM32G4 Microcontroller for processing motor commands, an absolute magnetic encoder for precise position sensing and a CAN-FD interface for communication.

*Image*

**Company**: MJBOTS

**Model**: moteus r4 controller ([link](https://mjbots.com/products/moteus-r4-11))

**Specifications**: 

- 3 phase brushless FOC based control
- Voltage Input: 10-44V (<=10S)
- Temperature: -40-85C
- Peak Electrical Power: 500W
- Mass: 14.2g
- Control rate: 15-30kHz
- PWM switching rate: 15-60kHz
- 170 MHz 32 bit STM32G4 microprocessor
- Peak phase current: 100A
- Continuous phase current: 11A / 22A (w/o and w/ thermal management)
- Max electrical frequency: 3kHz
- Dimensions: 46x53mm - 2D CAD drawing / STEP file
- Communications: 5Mbps CAN-FD
- Power Connector: 2x XT30PW-M
- Data Connector: 2x JST PH-3
- Open source firmware (Apache 2.0): https://github.com/mjbots/moteus

**Resources**:
- [Getting started](https://github.com/mjbots/moteus/blob/main/docs/getting_started.md)
- [Reference manual](https://github.com/mjbots/moteus/blob/main/docs/reference.md)
- [Video tutorial for using the developers kit](https://youtu.be/6prAv9hbmeM)

### Pi3Hats
---
A board that connects to the Raspberry Pi via GPIO, providing 5 CAN-FD interfaces for communication with controllers, power supply for the Raspberry Pi and inertial Measurement Unit (IMU) for motion tracking.

*Image*

**Company**: MJBOTS\
**Model**: ph3hat r4.5 ([link](https://mjbots.com/products/mjbots-pi3hat-r4-5))

**Specifications**:

- Voltage Input: 8V-54V
- 5x 5Mbps independent CAN-FD interfaces
- 1kHz attitude reference system
- Connectors: 
- Daisy chained XT30-M power connector
- JST-PH3 for all CAN-FD ports
- 40 pin Raspberry Pi GPIO
- 0.1" connectors for I2C, UART, and 3.3V/5V power output
- Provides up to 2.5A at 5V for the Raspberry Pi
- Open source firmware: https://github.com/mjbots/pi3hat

The mjbots pi3hat is a daughterboard for a Raspberry Pi 3 or Raspberry Pi 4, which provides the following:

- Power input from 8-54V [2], provides 5V for Raspberry Pi
- 5x independent 5Mbps CAN-FD buses
- A 1kHz IMU and attitude reference system
- A spread spectrum nrf24l01 interface

**Resources**:
- [Reference manual](https://github.com/mjbots/pi3hat/blob/master/docs/reference.md)
- [Video](https://youtu.be/aJKB5TGMtuI)


### Power Distribution Boards
---
Used for distributing power from a 6S1P Lithium Polymer (LiPo) battery, pre-charging high capacitance loads (to protect controllers) and software-controlled shutdowns.

*Image*

**Company**: MJBOTS\
**Model**: power_dist r4.5b ([link](https://mjbots.com/products/mjbots-power-dist-r4-5b?_pos=1&_sid=bde79f53b&_ss=r))

**Specifications**:

- Voltage input: 10-44V (<= 10S), XT90-M connector
- Current (continuous/peak): 45A/80A
- Data:
- 5 Mbps CAN-FD
- Power and energy monitoring
- Soft switch control
- Max load capacitance: 4000uF
- Quiescent Current: 300uA
- Output: 6x XT30-F
- Dimensions: 50mm x 80mm
- Mass: 36.0g
- Open source firmware: https://github.com/mjbots/power_dist

Resources:
- [Reference manual](https://github.com/mjbots/power_dist/blob/main/docs/reference.md)


### Motors
---
Brushless motors used for driving.

*Image*

**Brand**: Tarot Martin\
**Model**: 4008 MT 330kv TL2955 Brushless Multicopter Motor ([link](https://www.premium-modellbau.de/tarot-4008-mt-330kv-tl2955-brushless-multicopter-motor-6s-85g-tl2955)) 

**Specifications**:

- Support lithium section number: 6S
- Propeller installation diameter: 10mm/12mm/34mm
- Recommend Propeller: 15 inch
- Stator diameter: 38mm
- No load current: 0.53a 20V
- Stator end thickness: 8.0
- Stator end number: 18N
- Motor pole number: 24P
- Speed: 330KV ± 5%
- Motor external diameter: 44.5mm
- Axis diameter: 4mm
- Motor height: 22mm
- Maximum continuous current (1): 30A
- Maximum continuous power: 497W
- Motor weight: 85g (including the propeller holder)

The motors are designed for hobby drones which makes them more affordable than the motors most of the other teams use. 

*Link to description of motor calibration.*

### Cables
---
There are different types of custom cables which are needed. 

**XT30 Cables**

XT30 cables refer to cables that use XT30 connectors, which are small, high-current electrical connectors commonly used in RC (radio-controlled) drones, airplanes, cars, and other battery-powered electronics. The XT30 connector is designed for applications requiring up to 30A of continuous current and is a smaller version of the more common XT60 connector.

*Image*

A youtube tutorial from MJBOTS on how to manufacture those cables can be found [here](https://www.youtube.com/watch?v=f6WtDFWuxuQ).

Needed materials:

- XT30 connectors (male or female)
- 16-18 American wire gauge (AWG) silicon wire - 18 AWG ~ 0.75mm² (Lautsprecherkabel)
- separate flux (optional but recommended) 
- Heat shrink tubing (1/8inch ~ 3.2mm)
- Lead free solder

Needed tools:

- Soldering helping hands
- Wire cutter
- Wire stripper
- Hot air gun
- Soldering iron

### Batteries
What type of batteries are used? How do they have to be charged? How should they be stored? What safety measure should be taken? ...

6S1P Lithium Polymer (LiPo) batteries

**Charging**
- Never leave charging batteries unattented!
- The safest way to charge a lipo battery and the one that puts the least amount of strain on your battery is to charge at a rate of "1C" or 1 times capacity.  A 1C charge rate means that the current will charge the entire battery in 1 hour ( assuming you are starting with a fully discharged battery around 3.2v ).  For example, if you had a 1000mAh lipo, to charge at 1C you would set your charger for 1 A. -> This in not the current setting, but it is apparently the safest way.
- Once a lipo battery is charged, it is best to use it "soon" and then return the battery to storage voltage once done.  That is because a battery not at storage voltage is constantly degrading over time and that damage is cumulative. ... So if you find yourself with some fully charged batteries and no plans to use them in the next day or two, it would be best to discharge 
them down to storage voltage.

How to charge the batteries:
...

**Temperatures**

Heat and cold are the enemies of lipo batteries. Allowing lipo batteries to get hot either during use or especially during charging will damage them.  And on the other side, cold temperatures will decrease the performance of a lipo battery. 

**Life Span**

LiPo batteries have a limited lifespan.  Eventually, after 300 or so charge cycles, you will find that most LiPo batteries have lost a lot of performance and it will be time to retire them.  You will notice that you get less and less flight time and not as much "punch" as you did when the battery was new.  Another indicator of a battery being ready for disposal is "puffing".  Worn out or abused batteries will expand or puff up as components inside the battery turn into a gas.  Finally, if your battery charger can measure the batteries internal resistance, keep an eye on those numbers.  A sudden jump, or having one cell have an internal resistance that is much higher than the others is an indication that the battery should be retired.

## Robot Assembly