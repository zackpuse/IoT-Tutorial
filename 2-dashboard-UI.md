# 🚗 IoT Autonomous Vehicle Dashboard (Node-RED + MQTT)

A real-time IoT dashboard system using **Node-RED**, **MQTT (Mosquitto)**, and Raspberry Pi as a broker for vehicle telemetry and ADAS simulation.

---

## 📌 Project Overview

This project demonstrates a full IoT pipeline:

* Sensor / Simulation Data Source
* MQTT Communication Layer (Raspberry Pi Broker)
* Node-RED Dashboard UI (Windows Laptop)
* Real-time visualization (Speed, RPM, Battery, Alerts)

---

## 🧠 System Architecture

```text
[ Sensors / ESP32 / Inject Nodes ]
                ↓
     MQTT Publish (topic/data)
                ↓
 [ Raspberry Pi 4 - Mosquitto Broker ]
                ↓ MQTT Subscribe
     [ Node-RED Dashboard (Windows) ]
                ↓
     UI: Charts / Gauges / Status
```

---

## ⚙️ Technologies Used

* Node-RED
* Eclipse Mosquitto
* MQTT Protocol
* Raspberry Pi 4
* Windows 10/11

---

## 🖥️ Dashboard Features

✔ Real-time telemetry display
✔ MQTT live data streaming
✔ Vehicle-style UI (cockpit view)
✔ Speed / RPM / Battery simulation
✔ Status indicators (Engine / ADAS alerts)


## 📊 Install Dashboard

Install modern dashboard:

```bash
npm install @flowfuse/node-red-dashboard
```

OR via Node-RED UI:

* Menu → Manage Palette → Install
* Search:

```
@flowfuse/node-red-dashboard
```

---



### MQTT Broker Settings:

| Field    | Value           |
| -------- | --------------- |
| Server   | xxx.xxx.xxx.xxx   |
| Port     | 1883            |
| Username | pi              |
| Password | (xxxx) |

---

## 📡 Example MQTT Topics

| Data Type | Topic          |
| --------- | -------------- |
| Speed     | /vehicle/speed |
| RPM       | /engine/rpm    |
| Battery   | /battery/soc   |
| ADAS      | /adas/status   |

---

## 🧪 Example Flow

```text
[inject] → [mqtt out]
```

Payload:

```text
Engine OK
```

Topic:

```text
test/topic
```

---

## 📊 Dashboard Flow Example

```text
[mqtt in] → [ui_gauge]
[mqtt in] → [ui_chart]
[mqtt in] → [ui_text]
```

---

## 🚗 Real-Time Use Case

This system simulates:

* Vehicle telemetry streaming
* ECU data broadcasting
* ADAS sensor feedback
* Fleet monitoring system

---

---

### loading Dashboard 


```text
http://localhost:1880/dashboard
```

---

## 📌 Future Improvements

* CAN bus integration (SocketCAN)
* Real vehicle ECU simulation
* Cloud MQTT bridge (AWS / Azure)
* AI-based ADAS detection system
* Web-based fleet dashboard

---

## 👨‍💻 Mohd Fadli

IoT Automotive Lab Project
Node-RED + MQTT Vehicle Telemetry System

