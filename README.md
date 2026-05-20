# KR260 Multi-Sensor AXI-DMA Streaming Pipeline

## Overview
Real-time sensor data pipeline:

IMU → Mega → KR260 (PL) → AXI DMA → DDR → UDP → Jetson → AI

## Current Status
- AXI DMA pipeline working
- UART bridge (Arduino Mega) achieved
- IMU integration complete

## Next Steps
- Validate IMU data streaming - completed
- Add camera input
- Implement timestamp alignment

## Github Structure

<img width="368" height="458" alt="image" src="https://github.com/user-attachments/assets/3b80be9f-5e30-4bf3-ab59-475bf087dae2" />

