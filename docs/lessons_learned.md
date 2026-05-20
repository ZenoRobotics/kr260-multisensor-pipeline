## Initial Test using Verilog Counter Gen

- First 3 AXI words dropped → DMA not ready at start
- TLAST alignment critical for packet integrity
- Cache invalidate required before CPU read

## AMD Kria vs Vitis

The Kria is notoriously tricky to debug in Vitis due to its hard-coded QSPI boot mode. This often causes inconsistent behavior, hanging at boot around 69% of the time, or memory write errors when Vitis and the SoM conflict over control. So, I must do the following after each board power down/up or pushing the reset button. 
```
>	Connect targets -set -filter {name =~ "PSU"} 
>	mwr 0xffca0010 0x0 
>	mwr 0xff5e0200 0x0100 
>	rst -system 
```

This allows JTAG to take over the boot on rst -system. This is working for me on the IMU Golden vivado and vitis debug now.(Source: AMD).

What I was running into with default QSPI boot:

- .text at valid address but memory write errors
- A53 runaway / EDITR not ready
- debug working one night and failing the next
- Ubuntu/boot image interactions
- golden design intermittently working

Status: 

KR260 JTAG debug prep after every power cycle/reset:
disable/neutralize QSPI boot path so JTAG can take control cleanly.
