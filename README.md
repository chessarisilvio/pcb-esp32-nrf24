<img width="1224" height="1611" alt="1000138540" src="https://github.com/user-attachments/assets/089c7715-0368-4cbc-8ecf-4815a7639396" />
pcb-esp32-nrf24

This project is made to make two esp32 communicate on the 2.4ghz band with 2 nrf24 ebyte in the range of 50m.
you can then show the text on a esp32 Yellow board or on a 0.96 OLED screen connected to the esp32.
It’s perfect for experimenting with wireless communication.

What You Need (Per Node)
- 1× ESP32 DevKit board (38-pin, with USB Type-C)
- 2× nRF24L01 modules (E-BYTE version recommended)
- 1× ESP32 yellow display board (2.8" TFT with ILI9341 controller) OR a ssd1306 i2c OLED 0.96" screen
- 2× LM2596-3.3V DC-DC step-down converters (5V to 3.3V)
- At least 2 standard breadboards (or if assembling custom, 2 sets of 2.54mm female headers and 1 universal prototype board)
- 20× male-to-male jumper wires (20cm)
- 20× female-to-female jumper wires (20cm)
- Power supply: USB Type-C cable or a 5V battery pack

Wiring: Connect Each ESP32 to nRF24
nRF24L01 #1 Connections:
GND  -> GND
VCC  -> 3.3V
CE   -> GPIO17
CSN  -> GPIO5
SCK  -> GPIO18
MOSI -> GPIO23
MISO -> GPIO19

nRF24L01 #2 Connections:
GND  -> GND
VCC  -> 3.3V
CE   -> GPIO18
CSN  -> GPIO21
SCK  -> GPIO18
MOSI -> GPIO23
MISO -> GPIO19


Step 1: Install Required Libraries
In Arduino IDE, go to Sketch → Include Library → Manage Libraries and install:
- RF24 by TMRh20
- TFT_eSPI by Bodmer

Step 2: Configure TFT_eSPI Library
Find the TFT_eSPI folder:
- Windows: Documents\Arduino\libraries\TFT_eSPI
- Mac: ~/Documents/Arduino/libraries/TFT_eSPI
- Linux: ~/Arduino/libraries/TFT_eSPI
Open User_Setup_Select.h. Comment out all the #include lines except the one for your display. For the ESP32 yellow board with ILI9341, use:

for the c++ code:
#include <User_Setups/Setup25_TTGO_T_Display.h>

Step 3: Assemble Hardware
Node A: ESP32 #1 + nRF24 (with pins as above) + ESP32 Yellow Display #1

Node B: ESP32 #2 + nRF24 (same pins) + ESP32 Yellow Display #2

Step 4: Upload Code to Display Boards
Open display code in Arduino IDE.

Update your WiFi credentials:
On the c++ code find this two variables and give them the name and password of your wifi
const char* ssid = "YourWiFiName";
const char* password = "YourWiFiPassword";

For Display A, set String nodeID = "Display A";
For Display B, set String nodeID = "Display B";

then upload to each display board and open Serial Monitor (115200 baud) and note each display’s IP address.

Step 5:
Upload Code to nRF24 Boards, the update WiFi and set Display A’s IP address:
- const char* myDisplayIP = "192.168.1.100";  // Use your actual IP
- Upload to ESP32 with nRF24 (Node A) and check Serial Monitor for connection status.
- Repeat for Node B, using Display B’s IP address.

Step 6: Test the System
- Automatic test: Node A sends a message every 5 seconds, Node B every 7 seconds, and displays update automatically.

Manual test via web:
- Find Node A’s nRF24 IP from Serial Monitor.
- Open browser: http://[NodeA_IP]:8080
- Type a message, click "Send via nRF24," and see it appear on Node B’s display.
- Repeat from Node B to Node A.

Step 7: Verify Operation
Expected Serial Monitor output on Node A:

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

Expected display behavior:
- Shows received messages in large white text.
- Background refreshes on new messages.
- Footer displays IP address and uptime.

Troubleshooting
nRF24 initialization failed?
- Check all 7 connections (VCC, GND, CE, CSN, SCK, MOSI, MISO).
- Make sure nRF24 is powered from 3.3V, not 5V.
- Add a 10µF capacitor between VCC and GND on the nRF24.
- Try another nRF24 module (some may be defective).

WiFi connection timeout?
- Confirm SSID and password are correct.
- Router must be 2.4GHz (ESP32 doesn’t support 5GHz).
- Move closer to the router.
- Check router firewall settings.

Display shows "WiFi Failed"?
- Double-check credentials (case-sensitive).
- Confirm WiFi network is available.
- Try connecting via a phone hotspot.

Messages don’t appear on display?
- Verify display IP matches IP in nRF24 code.
- Ping display IP from a computer.
- Check Serial Monitor for HTTP errors.

nRF24 receives no data?
- Verify matching TX and RX addresses.
- Ensure modules are on the same channel.
- Reduce distance (start close, ~1 meter).
- Use the same data rate (RF24_250KBPS).

Display text looks corrupted?
- Reconfigure TFT_eSPI for your display.
- Check display wiring.
- Update TFT_eSPI library.

Web interface inaccessible?
- Check ESP32 IP in Serial Monitor.
- Use correct ports (:8080 for nRF24, :80 for displays).
- Disable or adjust computer firewall.
