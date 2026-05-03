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
4.  Click the **Install** tab and search for: `node-red-dashboard`.
5.  Click **Install**.

---

## 🔌 Connecting to the Raspberry Pi MQTT Broker
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

## 🛠 Troubleshooting
* **Command not found:** If `npm` isn't recognized, restart your computer after the Node.js installation.
* **Firewall Prompt:** If Windows asks for permission to let Node.js communicate on the network, select **Private Networks** and click **Allow**.
* **Connection Refused:** Ensure your laptop and the Raspberry Pi are on the **same Wi-Fi network**.
