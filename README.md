# UAV_Board

## STM32 UAV/AGV Control Board

Custom PCB Flight & Navigation Controller for Drones and Autonomous Ground Vehicles

https://opensource.org/licenses/MIT
https://www.st.com/en/development-tools/stm32cubeide.html
## Overview

This repository contains the complete hardware design (schematics, PCB layout, Gerbers) and firmware framework for a custom STM32-based control board designed for Unmanned Aerial Vehicles (UAVs / Drones) and Autonomous Ground Vehicles (AGVs). The board integrates high-precision GNSS/GPS positioning, long-range LoRa telemetry and command links, and heavy-duty DC motor control via PWM, while providing dedicated expansion ports for custom sensor integration.
Built around the STM32 microcontroller ecosystem, this project provides a robust, open-source foundation for robotics applications requiring reliable navigation, long-range communication, and high-power actuation .

## Key Features V1.0

# Feature	Specification
<pre>
MCU	STM32F4 Series (ARM Cortex-M4) — High-performance real-time processing
GNSS/GPS	Multi-constellation GPS module (UART interface) for precise geolocation and waypoint navigation 
Long-Range Comms	LoRa Module (SPI/UART) for low-power, long-distance telemetry and command links (km+ range)
Motor Control	4–8x PWM outputs for heavy-duty DC motors / ESCs; supports standard PWM, OneShot, and DShot protocols 
Sensor Expansion	Free I²C / SPI / UART headers for IMU, barometer, magnetometer, lidar, ultrasonic, camera triggers, etc.
Power Management	Dual-rail power input (Battery + BEC); onboard 3.3V/5V LDO and DC-DC regulators
Debugging	SWD/JTAG interface + USB OTG for firmware upload and serial monitoring
Form Factor	Compact 2-layer PCB optimized for vibration resistance and thermal management
</pre>

## System Architecture

<pre>
┌─────────────────────────────────────────┐
│           STM32F4/F7 MCU                │
│    (Cortex-M4/M7 @ 168-480 MHz)         │
├─────────────────────────────────────────┤
│  • GPS/GNSS Module ────────► UART1     │
│  • LoRa Transceiver ───────► SPI1│
│  • IMU (MPU6050/9250) ─────► I2C1/SPI1 │
│  • Barometer/Mag ──────────► I2C1      │
│  • PWM Motor Outputs ──────► TIM1-TIM4 │
│  • Sensor Headers ─────────► I2C1/SPI1 │
│  • USB OTG / SWD ──────────► Debug     │
└─────────────────────────────────────────┘
</pre>
  
## Hardware Highlights
## 1. GNSS / GPS Integration
NEO-M9N or compatible multi-band GNSS receiver
UART interface with PPS (Pulse Per Second) for time synchronization
Supports GPS, GLONASS, Galileo, and BeiDou for robust global positioning 
## 2. LoRa Communication Module
SX1276/RFM95W or STM32WL integrated transceiver
Frequency: 433/868/915 MHz (region-selectable)
Protocol: Custom lightweight telemetry frame or MAVLink over LoRa
Range: 2–10+ km line-of-sight (depending on antenna and environment)
## 3. Heavy-Duty DC Motor Control (PWM)
4–8 independent PWM channels (16-bit resolution, 50 Hz–2 kHz configurable)
Direct drive of Brushed DC motor drivers (e.g., BTS7960, VNH5019) or Brushless ESCs
Current sensing inputs (optional) for stall detection and overcurrent protection
Isolated gate drivers for high-power MOSFET bridges 
## 4. Sensor Expansion Ports
2× I²C buses (3.3V, 400 kHz Fast Mode) — IMU, barometer, magnetometer, OLED
1× SPI bus (up to 21 Mbit/s) — High-speed sensors, SD card logging
2× UART ports — Lidar, optical flow, companion computer (Raspberry Pi / Jetson)
ADC inputs — Battery voltage monitoring, current sensors, analog payloads
GPIO headers — Interrupt-capable pins for encoders, limit switches, triggers

## Hardware Design Files

File	Description
Schematics/	Full schematic sheets (MCU, power, comms, motor drivers, connectors)
Altium PCB layout (2-layer stackup, impedance-controlled traces)
Gerbers/	Production-ready Gerber files + drill files
BOM/	Bill of Materials with LCSC/JLCPCB part numbers
3D_Models/	STEP files for enclosure design and mechanical integration

PCB Specifications:
Layers: 2-layer 
Dimensions: 126 mm × 91 mm 
Connectors: JST-GH, XT60 (power), SMA (LoRa/GPS antenna)
Mounting: 30.5 mm × 30.5 mm M3 holes (compatible with standard drone frames)

## Firmware
The firmware is developed in STM32CubeIDE using HAL/LL libraries with a modular architecture:
<pre>
// Example: Initialize PWM for motor control
HAL_TIM_PWM_Start(&htim3, TIM_CHANNEL_1);  // Motor 1
HAL_TIM_PWM_Start(&htim3, TIM_CHANNEL_2);  // Motor 2
// Set duty cycle (0-1000 maps to 0-100%)
__HAL_TIM_SET_COMPARE(&htim3, TIM_CHANNEL_1, 750);  // 75% throttle
Implemented Modules:
✅ GPS NMEA/UBX parser
✅ LoRa packet radio driver (TX/RX)
✅ PWM motor control with failsafe
✅ I²C/SPI sensor abstraction layer
✅ ADC battery monitoring
✅ UART command interface
🔄 MAVLink protocol integration (in progress)
🔄 PID attitude control loop (in progress)
</pre>
## Getting Started
# Hardware Requirements
 <pre>
This custom PCB (fabricated via JLCPCB/PCBWay)
GPS module (NEO-M9N or u-blox compatible)
LoRa module (RFM95W / Ebyte E22)
Heavy-duty DC motors + motor drivers or BLDC ESCs
2S–6S LiPo battery (7.4V–22.2V)
ST-Link V2 debugger
Software Requirements
STM32CubeIDE
STM32CubeMX (for peripheral configuration)
Git
Flashing Firmware
  
git clone https://github.com/ (Full firmware will be added ASAP)
cd stm32-uav-agv-controller/Firmware
</pre>

# Open in STM32CubeIDE → Build → Debug via ST-Link

# Applications
<pre>
Agricultural Drones — GPS waypoint spraying/monitoring with LoRa telemetry backhaul
Surveillance UAVs — Long-range communication for beyond-line-of-sight operations
AGVs / Robots — Indoor/outdoor navigation with sensor fusion and heavy payload motors
Search & Rescue — Autonomous GPS-guided missions with real-time status updates
Custom Robotics — Expandable platform for research and development 
</pre>
  
## Board Preview
![PCB](https://github.com/mazen-daghari/UAV_Board/blob/61bc43917c9f1d2c9e153b42a39d661d16fb4d6a/3D%202.png)
![PCB](https://github.com/mazen-daghari/UAV_Board/blob/61bc43917c9f1d2c9e153b42a39d661d16fb4d6a/3D%201.png)

## Contributing
Contributions are welcome! Whether it's hardware improvements, firmware features, or documentation ( feel free to open an issue or submit a pull request ).

## License
This project is licensed under the MIT License — see the LICENSE file for details.

## Authors

- [@mazen-daghari](https://www.github.com/mazen-daghari)

  
## Acknowledgements
Inspired by open-source flight controller projects like INAV, Betaflight, and ArduPilot 
STM32 drone development community and STMicroelectronics application notes .

## Disclaimer
This is an advanced electronics project involving high-current DC motors and LiPo batteries. Ensure proper safety precautions (fuses, fireproof bags, insulated wiring) when testing. The authors are not responsible for damage or injury resulting from the use of this hardware.
