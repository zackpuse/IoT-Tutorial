# 🚗 Raspberry Pi 4: MQTT & CAN Bus Gateway

This guide covers the installation of an MQTT broker and its application in both standard IoT and specialized automotive (ADAS) environments.

## ⚙️ Phase 1: Installation & Configuration
Before setting up the architecture, you must prepare the Raspberry Pi to act as the Broker.

### 1. System Update
```bash
sudo apt update && sudo apt upgrade -y
```

### 2. Install Mosquitto Broker
```bash
sudo apt install mosquitto mosquitto-clients -y
```

### 3. Enable & Start Service
```bash
sudo systemctl enable mosquitto
sudo systemctl start mosquitto
```

### 4. Security Setup (Recommended)
By default, Mosquitto denies external access. To secure and allow connections:
1. Create a password: `sudo mosquitto_passwd -c /etc/mosquitto/passwd pi`
2. Configure the listener: `sudo nano /etc/mosquitto/conf.d/default.conf`
3. Paste the following:
   ```text
   listener 1883
   allow_anonymous false
   password_file /etc/mosquitto/passwd
   ```
4. Restart: `sudo systemctl restart mosquitto`

---

### 5. 🧠 Testing Mosquitto
1. Check Mosquitto version

```bash id="v3kq1a"
mosquitto -v
```

👉 Output example:

```
mosquitto version 2.0.11
```

---

2. 🟢 Check if Mosquitto is active (running service)

### ✅ System service status

```bash id="s8n2vc"
sudo systemctl status mosquitto
```

---

### 🔎 What you should see:

If active:

```
Active: active (running)
```

If not running:

```
Active: inactive (dead)
```

---

3. ⚡Quick lightweight check

### Just check running state:

```bash id="k4d7lm"
systemctl is-active mosquitto
```

Output:

* `active` → running
* `inactive` → stopped
* `failed` → error

---
4.📊 Check MQTT port (1883)

To confirm broker is listening:

```bash id="t9xqwo"
sudo ss -tulnp | grep 1883
```

Expected:

```
LISTEN 0  ... :1883
```

---
5.🧪 Test Message

Try real publish/subscribe:

### Terminal 1 (subscriber)

```bash id="sub001"
mosquitto_sub -h localhost -t test/topic -u pi -P YOUR_PASSWORD
```

### Terminal 2 (publisher)

```bash id="pub001"
mosquitto_pub -h localhost -t test/topic -m "Raspberry Pi MQTT OK" -u pi -P YOUR_PASSWORD
```

---


## 🧠 Phase 2: Basic MQTT Architecture
This is the standard setup for general IoT projects (sensors, home automation, etc.).

### 📊 Basic Flow Diagram

```text
                ┌────────────────────┐
                │    Sensor Node     │
                │ (ESP32 / Arduino)  │
                └─────────┬──────────┘
                          │ Publish (Topic: /home/temp)
                          ▼
                ┌────────────────────┐
                │   Raspberry Pi 4   │
                │  Mosquitto Broker  │
                └─────────┬──────────┘
                          │
        ┌─────────────────┼─────────────────┐
        │                 │                 │
        ▼                 ▼                 ▼
┌──────────────┐  ┌──────────────┐  ┌──────────────┐
│   Laptop     │  │  Dashboard   │  │   Cloud      │
│ (Subscriber) │  │ (Node-RED)   │  │ (AWS/Azure)  │
└──────────────┘  └──────────────┘  └──────────────┘
```

---

## 🚘 Phase 3: Automotive CAN Bus Architecture
This setup is specific to **ADAS and Vehicle Telemetry**, where the Pi acts as a translator between the car's hardware and the network.

### 📡 Real CAN Bus → MQTT Pipeline

```text
                    ┌──────────────────────────────┐
                    │        Vehicle ECU           │
                    │ (Engine, Brake, Battery, ADAS)│
                    └─────────────┬────────────────┘
                                  │ Physical CAN High/Low
                                  ▼
                    ┌──────────────────────────────┐
                    │   CAN Interface Module       │
                    │ (SocketCAN / CAN-HAT / USB)  │
                    └─────────────┬────────────────┘
                                  │ Hexadecimal Frames
                                  ▼
                    ┌──────────────────────────────┐
                    │   Raspberry Pi 4 (Edge)      │
                    │  - CAN-to-MQTT Parser        │
                    │  - Data Processing Layer     │
                    └─────────────┬────────────────┘
                                  │ Decoded JSON (Port 1883)
                                  ▼
                    ┌──────────────────────────────┐
                    │ Eclipse Mosquitto Broker     │
                    └─────────────┬────────────────┘
                                  │
         ┌────────────────────────┼────────────────────────┐
         ▼                        ▼                        ▼
┌──────────────┐       ┌─────────────────┐      ┌──────────────────┐
│ Instrument   │       │ Remote Fleet    │      │ ADAS Diagnostic  │
│ Cluster (UI) │       │ Management      │      │ Engineering Tool │
└──────────────┘       └─────────────────┘      └──────────────────┘
```

---

## 🧪 Verification & Testing
To verify that your installation and architecture are working correctly, use two terminal windows:

* **The Listener (Subscriber):**
    `mosquitto_sub -h localhost -t "/vehicle/#" -u pi -P yourpassword`
* **The Data Source (Publisher):**
    `mosquitto_pub -h localhost -t "/vehicle/speed" -m "85" -u pi -P yourpassword`

---


## 🧩 Key Differences

| Feature | Basic Architecture | CAN Bus Architecture |
| :--- | :--- | :--- |
| **Data Source** | Simple Sensors (DHT11, LDR) | Vehicle Network (ECU, ADAS) |
| **Complexity** | Direct Publish | Requires Parsing/Decoding (DBC files) |
| **Connectivity** | Wi-Fi / Bluetooth | Physical Shield (MCP2515 / SN65HVD230) |
| **Use Case** | Smart Home / Education | Fleet Telemetry / Autonomous Testing |

---

## 📈 Future Improvements
* **Node-RED Visualization:** Create a live speedometer dashboard.
* **TLS/SSL:** Add certificates for secure vehicle-to-cloud communication.
* **DBC Integration:** Automate the decoding of raw CAN signals using Python libraries.
