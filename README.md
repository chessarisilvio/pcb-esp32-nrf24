# pcb-esp32-nrf24
This project is a modular PCB designed to host an ESP32 microcontroller and multiple NRF24 wireless modules, this board ideal for experimentation in wireless communication, sensor networks, or jamming systems.

how to make it work:
Hardware Requirements
For each node (x2):

1x ESP32 DevKit with nRF24L01 module

1x ESP32 with TFT display (yellow board)

2x 5v to 3.3v lm2596-3.3

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

Step 1: Install Required Libraries
Arduino IDE → Sketch → Include Library → Manage Libraries

Install these libraries:

RF24 by TMRh20

TFT_eSPI by Bodmer

WiFi (built-in)

WebServer (built-in)

HTTPClient (built-in)

Step 2: Configure TFT_eSPI Library
Locate the TFT_eSPI library folder:

Windows: Documents\Arduino\libraries\TFT_eSPI

Mac: ~/Documents/Arduino/libraries/TFT_eSPI

Linux: ~/Arduino/libraries/TFT_eSPI

Open User_Setup_Select.h

Comment out all #include lines and uncomment the one matching your display

For ESP32 yellow board with ILI9341, typically use:

cpp
#include <User_Setups/Setup25_TTGO_T_Display.h>
If your display model is different, edit User_Setup.h manually with correct pins

Step 3: Hardware Assembly
Node A:

ESP32 #1 with nRF24 (GPIO17=CE, GPIO5=CSN, standard SPI pins)

ESP32 Yellow Display #1

Node B:

ESP32 #2 with nRF24 (same pinout as Node A)

ESP32 Yellow Display #2

Step 4: Upload Code to Display Boards
Open Display code in Arduino IDE

Change WiFi credentials:

cpp
const char* ssid = "YourWiFiName";
const char* password = "YourWiFiPassword";
For Display A, set: String nodeID = "Display A";

For Display B, set: String nodeID = "Display B";

Upload to first display board

Open Serial Monitor (115200 baud)

Write down the IP address (e.g., 192.168.1.100)

Repeat for second display board and note its IP (e.g., 192.168.1.101)

Step 5: Upload Code to nRF24 Boards
For Node A:

Open Node A code

Change WiFi credentials (same as displays)

Set Display A IP address:

cpp
const char* myDisplayIP = "192.168.1.100";
Upload to ESP32 with nRF24 (Node A)

Open Serial Monitor to verify connection

For Node B:

Open Node B code

Change WiFi credentials

Set Display B IP address:

cpp
const char* myDisplayIP = "192.168.1.101";
Upload to ESP32 with nRF24 (Node B)

Open Serial Monitor to verify connection

Step 6: Testing the System
Automatic Test:

Node A sends message every 5 seconds

Node B sends message every 7 seconds

Both displays should update automatically

Manual Test via Web Interface:

Find IP of Node A nRF24 board in Serial Monitor

Open browser: http://192.168.1.xxx:8080 (replace with actual IP)

Type message and click "Send via nRF24"

Message should appear on Node B's display

Repeat from Node B to Node A

Step 7: Verify Operation
Expected Serial Monitor Output (Node A):

text
=== Node A: ESP32 with nRF24 ===
nRF24 initialized successfully
Connecting to WiFi.....
WiFi connected!
IP address: 192.168.1.102
Web server started on port 8080
Sending via nRF24: Node A: 5s - SUCCESS
=================================
nRF24 RECEIVED: Node B: 7s
=================================
Display update: HTTP 200
Expected Display Output:

Display should show received messages in large white text

Background updates with each new message

Footer shows IP and uptime

Troubleshooting Guide
Problem: nRF24 initialization failed
Solution:

Check all 7 wire connections (VCC, GND, CE, CSN, SCK, MOSI, MISO)

Ensure nRF24 powered from 3.3V (NOT 5V!)

Add 10µF capacitor between nRF24 VCC and GND

Try different nRF24 module (some are defective)

Problem: WiFi connection timeout
Solution:

Verify SSID and password are correct

Check WiFi router is 2.4GHz (ESP32 doesn't support 5GHz)

Move closer to router

Check router firewall settings

Problem: Display shows "WiFi Failed"
Solution:

Double-check credentials match exactly (case-sensitive)

Verify WiFi network is available

Try hotspot from phone to isolate issue

Problem: Messages not appearing on display
Solution:

Verify display IP matches IP in nRF24 code

Ping display IP from computer to check network connection

Check Serial Monitor for HTTP error codes

Ensure both boards on same WiFi network

Problem: nRF24 receives nothing
Solution:

Verify TX address of sender matches RX address of receiver

Check nRF24 modules are on same channel

Reduce distance between modules (start with 1 meter)

Ensure both using same data rate (RF24_250KBPS)

Problem: Display shows corrupted text
Solution:

Reconfigure TFT_eSPI library for your specific display

Check display wiring

Update TFT_eSPI library to latest version

Problem: Web interface not accessible
Solution:

Verify ESP32 IP address in Serial Monitor

Use correct port (:8080 for nRF24 boards, :80 for displays)

Check firewall on your computer

Advanced Customization
Adjust nRF24 Range:
cpp
radio.setPALevel(RF24_PA_HIGH);  // Use HIGH instead of LOW
Change Message Frequency:
cpp
const unsigned long sendInterval = 10000;  // 10 seconds instead of 5
Add Message History:
Store last 10 messages in array and display as scrolling list

Add Encryption:
Implement simple XOR encryption on messages before transmission

Add Timestamps:
Include RTC module and timestamp each message
