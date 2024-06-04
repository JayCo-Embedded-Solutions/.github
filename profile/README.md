## Hi there 👋

<!--

**Here are some ideas to get you started:**

🙋‍♀️ A short introduction - what is your organization all about?
🌈 Contribution guidelines - how can the community get involved?
👩‍💻 Useful resources - where can the community find your docs? Is there anything else the community should know?
🍿 Fun facts - what does your team eat for breakfast?
🧙 Remember, you can do mighty things with the power of [Markdown](https://docs.github.com/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
-->
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

### Jeremy Lautoa-Capelle 
- 4th year electrical engineering major
- Primarily responsible for hardware development

## Setup Instructions

## Safety Guidelines

## Software Overview
The [Range-Recon-Flight-Controller](https://github.com/JayCo-Embedded-Solutions/Range-Recon-Flight-Controller) repository hosts all the firmware written for the quadcopter's flight controller, including sensor drivers, picture-taking functions, and of course the flight control algorithm.

The [Range-Recon-Remote-Controller](https://github.com/JayCo-Embedded-Solutions/Range-Recon-Remote-Controller) repository contains the firmware for the quadcopter companion remote controller.

## Hardware Overview

For this project, we developed our own custom flight controller using Altium Designer. This is the main hardware development portion of the project (excluding prototyping). A picture of the final product is shown below (3D View):

![rangeReconFC_PCB](https://github.com/JayCo-Embedded-Solutions/.github/assets/121917210/91ea2fff-f13b-461d-a891-54ffbbd63994)

And top copper below:

![rangeReconTopCopper](https://github.com/JayCo-Embedded-Solutions/.github/assets/121917210/d6ad4111-7f27-4ff7-b971-2837fe5b873e)

As indicated in the above pictures, the hardware design is split into multiple different sections, each containing different sensors/peripherals that we developed firmware to interface with. This includes our IMU (MPU6500), Magnetometer (LIS3MDL), Barometer (BMP390), and others. 

The other hardware design files, including schematic pages and gerber files, are located in the [Range-Recon-Flight-Controller/Hardware](https://github.com/JayCo-Embedded-Solutions/Range-Recon-Flight-Controller/tree/f446prototype/hardware) directory.

## Other Documentation
- Project Specification
- Milestone Report
- Final Report (Coming Soon!)
