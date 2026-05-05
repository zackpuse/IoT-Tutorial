# Tutorial: Installing Node-RED on Windows 10/11
### Project: IoT Autonomous Vehicle Control Center
**Target:** Individual student laptops (Windows) connecting to a central Raspberry Pi MQTT Broker.

---

## 📋 Prerequisites
Before installing Node-RED, you must have **Node.js** installed. It acts as the engine that runs Node-RED.

1.  **Download Node.js:** Visit [nodejs.org](https://nodejs.org/).
2.  **Select LTS Version:** Download the **LTS (Long Term Support)** installer.
3.  **Run Installer:** Follow the prompts. When asked about "Tools for Native Modules," you can leave the box **unchecked** to save time/space for this specific workshop.
4.  **Verify:** Open **Command Prompt** (cmd) and type:
    ```bash
    node -v
    npm -v
    ```
    *(You should see version numbers appear).*

---

## 🚀 Installation Steps

### 1. Install Node-RED via NPM
Open **Command Prompt** or **PowerShell** (Running as Administrator is recommended) and run:

```bash
npm install -g --unsafe-perm node-red
```

> [!NOTE]
> The `-g` flag stands for "global," allowing you to start Node-RED from any folder on your computer.

### 2. Start Node-RED
In your terminal, type:
```bash
node-red
```

Wait for the message: `[info] Server now running at http://127.0.0.1:1880/`.
**Keep this terminal window open!** If you close it, the dashboard will go offline.



---

## 🎨 Setting up the Dashboard UI
By default, Node-RED is just a logic editor. To create the "Cockpit" for your autonomous vehicle, you need the Dashboard nodes.

1.  Open your browser to `http://localhost:1880`.
2.  Click the **Menu** (three horizontal lines) in the top right corner.
3.  Select **Manage Palette**.
4.  Click the **Install** tab and search for: `@flowfuse/node-red-dashboard`.
5.  Click **Install**.

---
# 🔌  Connecting to the Raspberry Pi MQTT Broker

Instead:

## ✔ Use built-in MQTT nodes OR enable via palette

If missing, install:

```bash
node-red-contrib-mqtt
```

To receive data from your ESP32 via the Raspberry Pi, follow these steps in the Node-RED editor:

1.  **Drag an `mqtt in` node** from the network section onto the flow.
2.  **Double-click the node** and click the **Pencil Icon** next to "Add new mqtt-broker."
3.  **Connection Settings:**
    * **Name:** `RPi_Broker`
    * **Server:** `[INSERT_PI_IP_HERE]` (e.g., `192.168.1.50`)
    * **Port:** `1883`
4.  **Topic Settings:**
    * Set Topic to: `iot/class/group[X]/#` (Replace `[X]` with your assigned number).
5.  **Deploy:** Click the red **Deploy** button in the top right.



---

storage data local.

Data saved:

* temperature
* humidity
* throttle
* timestamp

---

# ✅ STEP 1 — Install Required Node

In Node-RED:

### Menu → Manage palette → Install

Search:

```text id="8j3m9x"
node-red-node-file
```

Install it.

👉 This gives you the **file write node**

---

# ✅ STEP 2 — Create MQTT Inputs

Add **3 MQTT IN nodes**:

| Node   | Topic              |
| ------ | ------------------ |
| MQTT 1 | sensor/temperature |
| MQTT 2 | sensor/humidity    |
| MQTT 3 | sensor/throttle    |

---

# ✅ STEP 3 — Convert Each Topic (Function Nodes)

Attach a **function node after each MQTT input**

---

## 🌡 Temperature Function

```js id="kq9p1a"
msg.topic = "temperature";
msg.payload = parseFloat(msg.payload);
return msg;
```

---

## 💧 Humidity Function

```js id="h3xq7n"
msg.topic = "humidity";
msg.payload = parseFloat(msg.payload);
return msg;
```

---

## 🎚 Throttle Function

```js id="v9m2kd"
msg.topic = "throttle";
msg.payload = parseInt(msg.payload);
return msg;
```

---

# ✅ STEP 4 — JOIN DATA (VERY IMPORTANT)

Add **JOIN node**

### Configure it:

* Mode: **manual**
* Combine: **key/value object**
* After receiving: **3 message parts**
* Timeout: 2–5 seconds

---

### Output will become:

```json id="z8kq2v"
{
  "temperature": 28,
  "humidity": 65,
  "throttle": 40
}
```

---

# ✅ STEP 5 — FORMAT TO CSV

Add a **function node after JOIN**

```js id="m4n7pq"
let now = new Date().toISOString();

let temp = msg.payload.temperature;
let hum = msg.payload.humidity;
let thr = msg.payload.throttle;

msg.payload = `${now},${temp},${hum},${thr}\n`;

return msg;
```

---

# ✅ STEP 6 — FILE NODE (SAVE DATA)

Add **file node**

### Configure:

* Action: **append to file**
* Filename:

### Windows:

```text id="b7q2lm"
D:/node-red/iot_data.csv
```

### Raspberry Pi / Linux:

```text id="n9v4xq"
/home/pi/iot_data.csv
```

---

# ✅ STEP 7 — ADD HEADER (ONLY ONCE)

Add **inject node (manual trigger)**

Connect to a function:

```js id="t3m8zx"
msg.payload = "timestamp,temperature,humidity,throttle\n";
return msg;
```

Then connect to SAME file node (append mode)

👉 Click inject ONCE only at start

---

# 🟢 FINAL FLOW STRUCTURE

```text id="l2q8pn"
MQTT(temp) ─┐
MQTT(hum)  ─┼→ Function → JOIN → CSV FORMAT → FILE
MQTT(thr)  ─┘

Inject (header) → FILE (once)
```

---

# 📁 RESULTING FILE

Your CSV will look like:

```csv id="r8v3mk"
timestamp,temperature,humidity,throttle
2026-05-05T10:00:00Z,28,65,40
2026-05-05T10:00:05Z,29,66,42
2026-05-05T10:00:10Z,30,67,45
```

---

# ⚠️ IMPORTANT PRACTICAL TIPS

## 1. Avoid data flooding

Use ESP32 interval:

* 2–5 seconds minimum

---

## 2. Ensure folder exists

Node-RED WILL NOT create folders automatically.

---

## 3. Safe shutdown

Do not power off while writing heavily (risk file corruption)

---

## 4. Recommended upgrade

Use:

* Raspberry Pi (best logging stability)
* or Windows PC Node-RED service

---

# 🧠 ENGINEERING VIEW

You just built:

✔ Real-time acquisition system
✔ Data logger (blackbox style)
✔ Multi-sensor fusion pipeline

This is similar to:

* Automotive ECU logging
* Industrial PLC historian
* Flight recorder system concept

---

# 🚀 NEXT LEVEL (OPTIONAL UPGRADE)

If you want, I can upgrade this to:

✔ Graph dashboard from CSV
✔ Auto file rotation (daily logs)
✔ Excel auto-report generator
✔ SD card logging on ESP32 (offline system)
✔ Cloud sync backup system

Just tell 👍
