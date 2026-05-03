
## 🏗️ Arduino Firmware Structure
The following professional template is used to ensure high-speed data processing and modular logic.

```cpp
/* * Project: Automotive Data Gateway
 * Description: Reads CAN data and sends to Serial for Raspberry Pi
 */

// --- 1. GLOBAL SPACE ---
#include <SPI.h>           // Required for most CAN-HATs
const int CAN_CS_PIN = 10; // Chip Select for CAN module
int vehicleSpeed = 0;      // Variable to store speed

// --- 2. SETUP (Runs Once) ---
void setup() {
  Serial.begin(115200);    // High speed for automotive data
  pinMode(LED_BUILTIN, OUTPUT);
  
  // Initialize CAN bus hardware here
  Serial.println("System Initializing...");
}

// --- 3. LOOP (Runs Forever) ---
void loop() {
  readVehicleData();       // Call custom function to get data
  checkSafetyLimits();     // Call custom function for logic
  
  delay(100);              // Small stability delay
}

// --- 4. CUSTOM FUNCTIONS ---
void readVehicleData() {
  // Logic to read from CAN bus would go here
  vehicleSpeed = random(0, 120); // Simulating data
  Serial.print("SPEED:"); 
  Serial.println(vehicleSpeed);  // Output for Raspberry Pi Parser
}

void checkSafetyLimits() {
  if (vehicleSpeed > 100) {
    digitalWrite(LED_BUILTIN, HIGH); // Alarm/Visual Feedback
  } else {
    digitalWrite(LED_BUILTIN, LOW);
  }
}
```

---

## 🛠 Troubleshooting
| Error | Cause | Solution |
| :--- | :--- | :--- |
| `TypeError: (reading 'trim')` | Node-RED msg.topic is undefined | Add a **Change Node** to set `msg.topic` before the MQTT Out node. |
| **Connection Refused (Win 11)** | Pi Firewall or Config | Run `ufw allow 1883` and check the `listener 1883 0.0.0.0` setting. |
| **Garbage Serial Data** | Baud rate mismatch | Ensure both Arduino and Pi are set to `115200`. |


