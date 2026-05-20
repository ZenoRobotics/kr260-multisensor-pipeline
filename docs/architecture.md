## Data Flow

Pico (UART)
→ PL UART RX
→ Packet Parser
→ AXI-Stream
→ AXI DMA (S2MM)
→ DDR
→ CPU
→ UDP
→ Jetson

## Key Design Decisions

- Used Pico to avoid RTL I2C complexity
- AXI DMA for high-throughput buffering
- CPU handles packetization (flexibility)



