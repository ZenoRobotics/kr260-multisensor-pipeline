# Low-Latency Multi-Sensor Edge AI Pipeline (Jetson + KR260)
You can find setup instructions here for the the course "Low-Latency Multi-Sensor Edge AI Pipeline (Jetson + KR260)".

This makes a great central location for finding steps to be followed for system setup and running.

## Overview
Real-time sensor data pipeline:

IMU → Mega → KR260 (PL) → AXI DMA → DDR → UDP → Jetson → AI

## Current Status
- AXI DMA pipeline working
- UART bridge (Arduino Mega) achieved
- IMU integration complete
- Validate IMU data streaming - completed

## Next Steps
- Implement timestamp alignment

## Github Structure

<img width="368" height="458" alt="image" src="https://github.com/user-attachments/assets/3b80be9f-5e30-4bf3-ab59-475bf087dae2" />

## Issues

1) There are major issues with trying to use both Vivado ILA for HW debug and Vitis for running software to test the hardware. They both share the same JTAG port and for this board, that is an issue.

2) Vitis hangs on the Axi-DMA code:
   ```
   Status = XAxiDma_CfgInitialize(&AxiDma, CfgPtr);
   ```
   when the board is powered up or reset in Bare-Metal mode to its default QSPI boot mode. So, one has to switch boot to JTAG mode.

3) Put Vitis into JTAG boot mode:

   Follow these instructions to enable the XSDB Console (maybe there is a simpler way to do this?) so that Vitis can be put in the JTAG mode using TCL commands given below. 

   First press DEBUG symbol in the left most column pane (just left of Vitis Explorer. You can then see the play, stop, step over, etc. symbols shown in the top left of the VITIS Explorer window. Press the green arrow to start Debug. Once it starts, there will be a blue triangle button on left, just under the green triangle you just pressed.

   The code will start to run but then hang at the XAxiDMA_CfgInitialize() function. This can be seen by using the step over button instead of the run button. Press the square in the DEBUG menu display to exit that simulation.

   This process then allows you to use the XSDB Console so that you can type in the commands needed to put Vitis in JTAG mode. Type the following series of TCL commands at the prompt in the XSDB Console:
   ```
   connect
   targets -set -filter {name =~ "PSU"}
   mwr 0xffca0010 0x0
   mwr 0xff5e0200 0x0100
   rst -system
   ```
   This allows JTAG to take over the boot on rst -system.

   This is the list of errors I was running into with default QSPI boot:
   ```
   .text at valid address but memory write errors
   A53 runaway / EDITR not ready
   debug working one night and failing the next
   Ubuntu/boot image interactions
   golden design intermittently working
   ```
### Boot Image Notes

I was changing the boot versions back and forth, and forgot which one I had for image A. I thought it was the newer 2025 version, but I hadn't booted the Kria since I performed the change to get the VIVADO ILA and Vitis debug working at the same time. 

I tried booting it tonight (6/1/2026) and it wouldn't boot. So, I was fairly sure I had the new version installed for images A & B, but I went ahead and installed version BOOT-k26-starter-kit-20230516185703.bin in Image A. The Kria was then able to boot after that.

Image B is still the newer boot image. The Xilinx Boot Image Recovery Tool has Image A as the "Requested Boot Image".

## System Startup Steps

1) Boot Ubuntu on Kria and Jetson.
2) Connect ethernet cord to Kria (top right eithernet port ) and the Jetson.

    
