# Home-Automation-System-with-IOT

# AIM: 
  To make a Lamp at home (230 V AC) On / Off using ESP8266, IFTT Google Assistance and Blynk IoT mobile application.          
           
# COMPONENTS REQUIRED:
PC with Internet connection
Micro USB cable
Wifi connection for ESP8266 (Use any mobile hotspot or Router)
	ESP8266 Board
	Mobile Phone with Blynk App installed
            IFTT for Google Voice Assistance
	9 W Bulb and Relay control
Arduino software 
Jumper Wires

## Theory: 
Blynk is an IoT platform for iOS or Android smartphones that is used to control Arduino, Raspberry Pi and NodeMCU via the Internet. This application is used to create a graphical interface or human machine interface (HMI) by compiling and providing the appropriate address on the available widgets.In this experiment we use ESP8266 to control a 220-volt lamp from a web server. But you can also use the same procedure to control fans, lights, AC, or other electrical devices that you want to control remotely.
Relay is an electromechanical device that is used as a switch between high current and low current devices. When the coil in the relay gets fully energized, the contact shifts from the normally open position to the normally closed position. Light bulbs usually operate on 120V or 220V AC power supply. We cannot interface these AC loads directly with the ESP8266 development board, or it will damage the board. We have to use a relay between the ESP8266 and the lamp. 
Google Assistant and IFTTT work together to let you control services with voice commands. When you say a set phrase, Google Assistant processes it and sends it to IFTTT as a trigger. If the phrase matches an applet you've created, IFTTT performs the linked action—like turning on a light or sending a message. Everything runs in the cloud, making it easy to automate tasks with just your voice, as long as the command is correctly matched and all services are online.
When we apply an active high signal to the signal pin of the relay module from any microcontroller like ESP8266, the relay contact moves from the normally open to the normally closed position. It makes the circuit complete, and the output load turns on.

# PROCEDURE:

•	Make the circuit connection as per the diagram. In the mobile, download and “Blynq IoT” application using Google play store and Install it. Create log in ID and Password.
•	Connect the IN pin of the Relay module to D1 pin of NodeMCU (ESP8266).
•	Connect VCC of the Relay of NodeMCU. Connect GND of the Relay to GND of NodeMCU. 
•	Connect your AC bulb to the Relay’s switch terminal securely.
•	Install ESP8266 board in Arduino IDE via Board Manager. Select board: NodeMCU 1.0 (ESP-12E Module).
•	Include necessary libraries: ESP8266WiFi and ESP8266WebServer.
•	In the code, configure Wi-Fi SSID and Password.
•	Set up a web server that responds to /on and /off URLs.
•	Upload the code to the ESP8266 using a micro USB cable.
•	Get Local IP Address After uploading, open Serial Monitor to find the local IP address of ESP8266.
•	Create Applets on IFTTT - For "This", select Google Assistant → "Say a simple phrase". Command: "Turn on the light". For "That", choose Webhooks → "Make a web request". 
•	Repeat to create another applet for command with URL.
•	Test the System - Google Assistant triggers IFTTT → sends Webhook to ESP8266 → turns ON the relay (light).
•	Say "Turn off the ligh to switch it OFF, Say "Turn on the light" to switch it ON.

# CIRCUIT DIAGRAM:

<img width="663" height="400" alt="image" src="https://github.com/user-attachments/assets/bfebc70d-25b4-4b4a-a7e1-2a02c09bf423" />
<img width="450" height="570" alt="image" src="https://github.com/user-attachments/assets/38d4c84d-cb3e-4c86-bc19-a29fc13377ac" />


 
# PROGRAM:
```
#define BLYNK_TEMPLATE_ID "TMPL33AOhHpv3" 
#define BLYNK_TEMPLATE_NAME "switch light" 
#define BLYNK_AUTH_TOKEN "oLXGhtWk4j91cXE1q1OG0k_FcZd619vq" 
#include <ESP8266WiFi.h> 
#include <Blynk, SimpleEsp8266.h> 
// WiFi credentials 
char ssid[] = "Allwin"; 
char pass[] = "alwn2009"; 
// Pin definitions for NodeMCU 
const int relayPins[4] = {D1, D2, D3, D5};  // GPIO pins connected to IN1, IN2, IN3, IN4 on relay module 
const int switchPins[4] = {D6, D7, 1, 3};  // GPIO1 (TX) for S3 
// State tracking for switches 
bool lastSwitchState[4] = {0, 0, 0, 0}; 
void setup() { 
Serial.begin(115200); 
Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass); 
// Initialize all relay control pins as outputs and set them HIGH to ensure relays are OFF at startup 
for (int i = 0; i < 4; i++) { 
pinMode(relayPins[i], OUTPUT); 
digitalWrite(relayPins[i], HIGH);  // Set HIGH to turn OFF the relays if they are active low 
pinMode(switchPins[i], INPUT_PULLUP);  // Use internal pull-up resistor 
} 
} 
void loop() { 
Blynk.run(); 
for (int i = 0; i < 4; i++) { 
bool currentSwitchState = !digitalRead(switchPins[i]);  // Read switch state (inverted due to pull-up) 
// Check if switch state has changed 
if (currentSwitchState != lastSwitchState[i]) { 
delay(50);  // Add debounce delay to avoid accidental toggling due to switch noise 
if (currentSwitchState == !digitalRead(switchPins[i])) {  // Re-check the switch state after delay 
if (currentSwitchState) { 
// Toggle relay state 
digitalWrite(relayPins[i], !digitalRead(relayPins[i])); 
Blynk.virtualWrite(V1 + i, digitalRead(relayPins[i]) ? 0 : 255);  // Update Blynk app 
} 
lastSwitchState[i] = currentSwitchState;  // Update last state 
} 
} 
} 
} 
// Blynk app write event handlers for each relay controlled by virtual pins 
BLYNK_WRITE(V1) { toggleRelay(0, param.asInt()); } 
BLYNK_WRITE(V2) { toggleRelay(1, param.asInt()); } 
BLYNK_WRITE(V3) { toggleRelay(2, param.asInt()); } 
BLYNK_WRITE(V4) { toggleRelay(3, param.asInt()); } 
void toggleRelay(int relay, int state) { 
digitalWrite(relayPins[relay], state == 1 ? LOW : HIGH);  // Assume active-low relays 
}
```

 
# Output:

https://github.com/user-attachments/assets/e70943d7-846c-43a6-8aae-f626b46b34d9


## Result:

Thus, Home Automation System with IOT was successfully implemented using ESP8266.

