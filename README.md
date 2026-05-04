# KR260 Multi-Sensor Streaming Pipeline

## Overview
Real-time sensor data pipeline:

IMU → Pico → KR260 (PL) → AXI DMA → DDR → UDP → Jetson → AI

## Current Status
- AXI DMA pipeline working
- UART bridge (Pico) in progress
- IMU integration underway

## Next Steps
- Validate IMU data streaming
- Add camera input
- Implement timestamp alignment

## Github Structure

<img width="368" height="458" alt="image" src="https://github.com/user-attachments/assets/3b80be9f-5e30-4bf3-ab59-475bf087dae2" />

