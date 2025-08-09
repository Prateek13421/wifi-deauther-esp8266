# ESP8266 Web Wi-Fi Deauther

## 📜 Description
This project turns an **ESP8266** into a **web-based Wi-Fi deauther** for **authorized security testing**.  
It allows you to:
- Scan for nearby Wi-Fi networks.
- Select a target network from a browser interface.
- Start/Stop sending Wi-Fi deauthentication frames.
- View live packet counts.

⚠ **Important:**  
Deauthentication attacks are illegal on networks you do not own or have explicit permission to test.  
This tool is for **educational and authorized penetration testing only**.

---

## 🚀 Features
- Web interface accessible from any device (phone/laptop).
- Network scan with SSID, channel, and BSSID display.
- One-click start and stop attack.
- Live packet counter.
- Runs standalone — no computer required after boot.

---

## 🛠 Hardware Requirements
- **ESP8266** board (e.g., NodeMCU, Wemos D1 mini, ESP-12E)
- Micro USB cable
- Arduino IDE installed on your computer

---

## 📦 Software Requirements
- **Arduino IDE** (latest version recommended)
- **ESP8266 Board Package** installed:
  1. Go to **File → Preferences**
  2. Add this URL in **Additional Board Manager URLs**:
     ```
     http://arduino.esp8266.com/stable/package_esp8266com_index.json
     ```
  3. Go to **Tools → Board → Board Manager** and install **ESP8266 by ESP8266 Community**

---

## 📋 Installation
1. Clone or download this repository.
2. Open `esp8266_web_deauther.ino` in Arduino IDE.
3. Select your board under **Tools → Board** (e.g., NodeMCU 1.0).
4. Select the correct COM port.
5. Click **Upload**.

---

## 🔌 Usage
1. Power up the ESP8266.
2. It will create a Wi-Fi AP:
   ```
   SSID: ESP8266_Deauther
   Password: 12345678
   ```
3. Connect your phone or laptop to this network.
4. Open a browser and go to:
   ```
   http://192.168.4.1/
   ```
5. Use the **Scan Networks** button to list nearby Wi-Fi networks.
6. Click **Attack** next to your target (your own network for testing).
7. Click **Stop Attack** when done.

---

## 📷 Screenshots
<img width="1102" height="427" alt="image" src="https://github.com/user-attachments/assets/43ec27a7-1bc8-4309-a1e5-4e2214d31555" />


---

## ⚠ Disclaimer
This project is for **educational purposes only**.  
Do **not** use it on networks without **explicit permission** from the owner.  
The author is **not responsible** for any misuse of this tool.

---

## 📄 License
This project is open-source and free to modify under the MIT License.
