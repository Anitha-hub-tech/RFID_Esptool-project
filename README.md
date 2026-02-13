#  RFID Card Top-Up System  
### IoT Architecture Project – Team its_ace

---

##  Project Overview

This project implements a complete **RFID Card Top-Up System** using a real-world IoT architecture model.

The system allows users to:

- Scan RFID cards  
- View card balance  
- Send top-up requests  
- Receive real-time updates  

The architecture strictly follows the required rule:

> **Devices speak MQTT.**  
> **Users speak HTTP & WebSocket.**  
> **The backend translates between them.**

---

#  System Architecture

The system consists of three main components:

---

##  ESP8266 (Edge Controller)

- Reads RFID card UID using MFRC522
- Publishes card UID and balance via MQTT
- Subscribes to top-up commands via MQTT
- Updates card balance
-  Does NOT use HTTP
-  Does NOT use WebSocket

---

## 2️ Backend API Service (VPS)

- Built with **Node.js + Express**
- Exposes:
  ```
  POST /topup
  ```
- Communicates with ESP8266 via MQTT
- Pushes real-time updates to browsers via WebSocket
- Acts as protocol translator

---

## 3️ Web Dashboard (Frontend)

- Sends top-up requests via HTTP
- Receives real-time updates via WebSocket
- Does NOT connect directly to MQTT
- Displays:
  - Current card UID
  - Current balance
  - Update history

---

#  System Communication Flow

## Card Scan Flow

1. RFID card is scanned
2. ESP8266 reads UID
3. ESP publishes to:
   ```
   rfid/batidao/card/status
   ```
4. Backend receives MQTT message
5. Backend broadcasts via WebSocket
6. Dashboard updates instantly

---

##  Top-Up Flow

1. User submits top-up form
2. Browser sends:
   ```
   POST /topup
   ```
3. Backend publishes:
   ```
   rfid/batidao/card/topup
   ```
4. ESP updates card balance
5. ESP publishes:
   ```
   rfid/batidao/card/balance
   ```
6. Dashboard updates in real-time

---

#  MQTT Topic Isolation

All teams share the same MQTT broker.

To prevent interference, this project uses a unique namespace:

```
Team ID: batidao
Base Topic: rfid/batidao/
```

### 📤 Card Status
```
rfid/its_ace/card/status
```

```json
{
  "uid": "A1B2C3D4",
  "balance": 3000
}
```

---
#  Hardware Connections

## ESP8266 ↔ MFRC522 RFID Module

| MFRC522 Pin | ESP8266 Pin | GPIO |
|-------------|-------------|------|
| SDA (SS)    | D2          | GPIO15 |
| SCK         | D5          | GPIO14 |
| MOSI        | D7          | GPIO13 |
| MISO        | D6          | GPIO12 |
| RST         | D3          | GPIO0  |
| GND         | GND         | — |
| 3.3V        | 3.3V        | — |


#  Technologies Used

### Embedded Side
- ESP8266
- MicroPython
- MFRC522 RFID Module
- MQTT Protocol

### Backend
- Node.js
- Express.js
- WebSocket (ws)
- MQTT (npm mqtt package)

### Frontend
- HTML
- Tailwind CSS
- JavaScript (Fetch + WebSocket)

---

#  Installation & Setup

## Backend Setup

```bash
npm init -y
npm install express ws mqtt
node server.js
```

Backend runs at:
http://157.173.101.159:9225
## ESP8266 Setup

1. Flash MicroPython firmware
2. Upload:
   - `main.py`
   - `mfrc522.py`
3. Configure WiFi credentials
4. Ensure:

```python
TEAM_ID = "batidao"
```
5. Run `main.py`
 # Project Structure

RFID/
│
├── server.js
├── package.json
├── package-lock.json
├── index.html
│
├── main.py
├── mfrc522.py

# Conclusion

This project demonstrates a practical implementation of a real-world IoT system architecture using:

- Edge computing (ESP8266)
- Cloud-based backend services
- MQTT messaging
- Real-time web communication

Team ID: **its_ace**  
RFID IoT Architecture Project  
