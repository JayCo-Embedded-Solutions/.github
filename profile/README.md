# JayCo Embedded Solutions
![Range Recon Logo](https://drive.google.com/uc?id=1eECACmVXNv3WCFBJkXMfIDpm5NHQGud7)

## Abstract
Many students have experience building systems without understanding the underlying drivers actuating the device. 
In this project, we explore the process of building device drivers for 95% of system components of a quadcopter drone. 
We achieved higher-level functionality including a flight control algorithm, image capturing mechanism, and manual remote control (RC), 
while following a ground-up approach to interfacing with each peripheral device. 
In developing these drivers, we gained significant experience with several hardware communication protocols such as I2C, UART, SPI, and became deeply familiar with the inner workings of each peripheral device. 
This allowed us to optimize our quadcopter at lower levels than what would be typically achievable using existing libraries, improving on battery life, communication speed, and memory efficiency.

## The Team
### Cody Rupp
- 3rd year computer science major
- Primarily responsible for firmware development
- [LinkedIn](https://www.linkedin.com/in/codyprupp/)

### Jeremy Lautoa-Capelle 
- 4th year electrical engineering major
- Primarily responsible for hardware development
- [LinkedIn](https://www.linkedin.com/in/jlautoa/)

## Project Overview

As previously mentioned, the goal of our project is to develop our own aerial photography drone while building our own software drivers and custom hardware. Most of our work in this project went into developing firmware for all devices (IMU, Barometer, etc.) from scratch, as well as architecting our own flight controller (both algorithm and PCB). The (vast amount of) other time was spent on building, troubleshooting, and repairing our prototypes, as well as testing and tuning our flight control algorithm.

A high-level overview of our work is shown below:

![rangeReconOverview](https://github.com/JayCo-Embedded-Solutions/.github/assets/121917210/0975abe5-5dd7-471d-8a61-373c708f3b3c)

Our objectives for the project were to develop hardware drivers for each of our sensors and actuators to be able to interface with them using our microcontroller.

Hardware Component List:
- MCU (Flight Controller): STM32F446RET6
- IMU: MPU6500
- Barometer: BMP390
- RFIC: NRF24L01
- Magnetometer: LIS3MDL
- MCU (Remote Controller): STM32F103C8T6

After developing our hardware drivers and establishing reliable communication between our flight controller and remote controller, we worked on developing our flight control algorithm.

Shown below is one of the early prototypes of our drone:

![rangeReconprototype](https://github.com/JayCo-Embedded-Solutions/.github/assets/121917210/2ec54037-ead5-4695-ac14-9c08d44776cf)

## Setup Instructions
To get started developing this device, follow these steps:
1. Clone the [Range-Recon-Flight-Controller](https://github.com/JayCo-Embedded-Solutions/Range-Recon-Flight-Controller) repository and the [Range-Recon-Remote-Controller](https://github.com/JayCo-Embedded-Solutions/Range-Recon-Remote-Controller) repository.
2. Purchase the hardware (list of necessary components is in the system diagram and listed below).
3. Install the STM32CubeIDE.
4. Import the .cproject files into your STM32CubeIDE workspace (there is one in each repository).
5. To check if your hardware setup is correct, refer to the pinouts available in the respective repositories' `.ioc` files.
6. Start developing! For explanations of the important files, see the "Firmware Overview" section below.

## Safety Guidelines
Safety is a critical part of this project, given that nearly every single component is a potential point of failure, and the high speeds and altitudes reached can cause serious damage. Thus, we have developed a multitude of safety features and adopted a set of guidelines to follow when testing or operating the quadcopter.

Firstly, our flight control firmware has two main ways of detecting and countering a hazard:
- Independent Watchdog Timer (IWDG)
    - The IWDG is a hardware timer built into the STM32F446 microcontroller that will reset the device if the timer is not refreshed before a specific time period. In our case, there is a 125ms timer that runs after the initial setup, and will only be refreshed if the RF module receives data. This way, if the drone loses connection while it is in the middle of a command to accelerate upwards, it does not become a rocket. It also enables us to easily have an "arm" stage when testing, which is mentioned below.

- Unsafe Angle Shutoff
    - The flight controller has a built in check before actuating the motors in each iteration of the control loop that will ensure that the current angle of the craft is not greater than 45 degrees. If the angle is greater than 45 degrees, the motors are shut off. This does limit the lateral speeds, but the number of times it has saved potentially out of control situations far outweighs such a benefit.

We also have a variety of on-site safety guidelines to ensure that no incidents occur:
- Aerodrome
    - UCSD has a facility called the "Aerodrome" which is a large netted area that can be used to test aircraft in a safe environment, out of the way of people and fragile objects. We have made extensive use of this facility, and we intend to continue to do as much testing as possible in it while we have time.
- Power Management
    - On the STM32F446, the 3.3V input pin is directly adjacent to the 5V input pin, and neither are labeled on the board. Since we supply power with 5V, the processor and its peripherals can be easily and badly damaged by incorrectly plugging our 5V power supply into the 3.3V pin. We have a system for double checking that this does not occur by first unplugging the battery power, then connecting the microcontroller to the unplugged battery, checking that the pin is correct, and then finally reconnecting the battery.
- Arming the device
    - The IWDG mentioned above allows us to prevent the aircraft from actuating any motors until we are ready. All we have to do is simply leave the RF module for the remote controller unplugged, and the watchdog timer will continuously reset the flight controller as it is not detecting any RF signal in the 125ms window. Then, when we are ready to fly, we can plug the RF module back in and the motors will be actuated on command.

## Firmware Overview
The [Range-Recon-Flight-Controller](https://github.com/JayCo-Embedded-Solutions/Range-Recon-Flight-Controller) repository hosts all the firmware written for the quadcopter's flight controller, including various sensor drivers, filters, picture-taking functions, and of course the flight control algorithm.

File Layout (currently refers to f446prototype branch, will be updated):

    root
    ├── firmware
    |   ├── Core
    |   |   ├──Inc                      <-- Header files for user code
    |   |   |   ├──main.h
    |   |   |   ├──flightController.h
    |   |   |   └──...
    |   |   └──Src                      <-- Contains all user code
    |   |       ├──main.c
    |   |       ├──flightController.c
    |   |       └──...
    |   ├── .cproject                   <-- Import project with this
    |   └── f446testprototype.ioc       <-- View/configure pinout
    └── hardware
        ├── gerb1.zip                   <-- Downloadable PCB files
        └── rangeReconMk1SCHPages.pdf   <-- Viewable schematic

At a high level, the flight controller takes in sensor data from its IMU, barometer, and magnetometer, and then fuses and filters them to get coherent pitch, roll, yaw, vertical velocity, altitude, and heading values. The specifics of how they are fused and filtered can be seen within the files. Then, when data is received over the RF transceiver module, the flight controller compares those "desired" movement values to the "measured" sensor values, and actuates the motors based on a PID control algorithm (available in `flightController.c` and `pidControl.c`). If the RF sends the signal to take a picture, the camera is given the command to take a picture, and that image data is stored in a micro SD card.

The [Range-Recon-Remote-Controller](https://github.com/JayCo-Embedded-Solutions/Range-Recon-Remote-Controller) repository contains the firmware for the quadcopter's companion remote controller, including RF transceiver drivers, analog joystick drivers, and OLED drivers.

File Layout:

    root
    ├── Core
    |   ├── Inc                     <-- Header files for user code
    |   |   ├── main.h
    |   |   └── nRF24l01.h
    |   └── Src                     <-- Contains all user code
    |       ├── main.c
    |       └── nRF24l01.c          <-- nRF24 module drivers
    ├── .cproject
    └── bluepill_nrf24l01_controller.ioc

The remote controller is much simpler than the flight controller. It simply reads the analog joystick values with a DMA, and then sends those raw values over the RF transceiver to the flight controller. We are working to implement an OLED display to show some in flight telemetry.

## Hardware Overview

For this project, we developed our own custom flight controller using Altium Designer. This is the main hardware development portion of the project (excluding prototyping). A picture of the final product is shown below (3D View):

![rangeReconFC_PCB](https://github.com/JayCo-Embedded-Solutions/.github/assets/121917210/91ea2fff-f13b-461d-a891-54ffbbd63994)

And top copper below:

![rangeReconTopCopper](https://github.com/JayCo-Embedded-Solutions/.github/assets/121917210/d6ad4111-7f27-4ff7-b971-2837fe5b873e)

As indicated in the above pictures, the hardware design is split into multiple different sections, each containing different sensors/peripherals that we developed firmware to interface with. This includes our IMU (MPU6500), Magnetometer (LIS3MDL), Barometer (BMP390), and others. 

The other hardware design files, including schematic pages and gerber files, are located in the [Range-Recon-Flight-Controller/Hardware](https://github.com/JayCo-Embedded-Solutions/Range-Recon-Flight-Controller/tree/f446prototype/hardware) directory.

Now, for some general features of the hardware design:

- The sensors and peripherals selected for the hardware design were selected to be the same modules that were used for prototyping. This is to reduce uncertainty and boost confidence in the board working on the first try, since they are components that we are familiar with.
- The placement of (most of) the components on the board is according to our STM32 IOC file, which determines which ports on the chip are used for which devices. Placing the components based on their pin locations is best to maximize signal integrity (not having to run many long traces), and make it easier for routing as well. Of course there are exceptions to this rule since board space is limited.
- The stackup for this design is a 4-layer controlled-impedance stackup. There are signal layers on the bottom and top layers, with two inner GND layers. We need controlled impedance for this design for routing the RF section.

## Other Documentation
- [Project Specification](https://drive.google.com/file/d/12DUhvl7pq2MYKs3B_Zq3_H3mgaKwSOg2/view?usp=sharing)
- [Milestone Report](https://drive.google.com/file/d/1jUV10IAVyD7B5ZanoJeC3_bdeJAXxO_Y/view?usp=sharing)
- Final Report (Coming Soon!)
