# JayCo Embedded Solutions
![Range Recon Logo](https://drive.google.com/uc?id=1eECACmVXNv3WCFBJkXMfIDpm5NHQGud7)

## Brief Overview (Abstract)
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

## Setup Instructions

## Safety Guidelines
Safety is a critical part of this project, given that nearly every single component is a potential point of failure, and the high speeds and altitudes reached can cause serious damage. Thus, we have developed a multitude of safety features and adopted a set of guidelines to follow when testing or operating the quadcopter.

Firstly, our flight control firmware has two main ways of detecting and countering a hazard:
- Independent Watchdog Timer (IWDG)
    - The IWDG is a hardware timer built into the STM32F446 microcontroller that will reset the device if the timer is not refreshed before a specific time period. In our case, there is a 125ms timer that runs after the initial setup, and will only be refreshed if the RF module receives data. This way, 
- Unsafe Angle Shutoff

We also have a variety of on-site safety guidelines ee

## Firmware Overview
The [Range-Recon-Flight-Controller](https://github.com/JayCo-Embedded-Solutions/Range-Recon-Flight-Controller) repository hosts all the firmware written for the quadcopter's flight controller, including various sensor drivers, filters, picture-taking functions, and of course the flight control algorithm.

The [Range-Recon-Remote-Controller](https://github.com/JayCo-Embedded-Solutions/Range-Recon-Remote-Controller) repository contains the firmware for the quadcopter's companion remote controller, including RF transceiver drivers, analog joystick drivers, and OLED drivers.

## Hardware Overview

## Other Documentation
- Project Specification
- Milestone Report
- Final Report (Coming Soon!)