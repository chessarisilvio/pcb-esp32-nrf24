# pcb-esp32-nrf24
This project is a modular PCB designed to host an ESP32 microcontroller and multiple NRF24 wireless modules, this board ideal for experimentation in wireless communication, sensor networks, or jamming systems.

how to make it work:
Hardware Requirements
For each node (x2):

1x ESP32 DevKit with nRF24L01 module

1x ESP32 with TFT display (yellow board)

Power supply (USB or battery)

Connections (each ESP32 with nRF24):

text
nRF24L01    ESP32
GND      →  GND
VCC      →  3.3V
CE       →  GPIO17
CSN      →  GPIO5
SCK      →  GPIO18
MOSI     →  GPIO23
MISO     →  GPIO19
IRQ      →  Not connected (optional)
