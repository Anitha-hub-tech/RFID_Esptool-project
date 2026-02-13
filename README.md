# RFID Card Top-Up System (IoT Architecture)

## 📌 Overview

This project implements a complete RFID Card Top-Up System using:

- ESP8266 (Edge Controller)
- MQTT (Publish–Subscribe Messaging)
- Cloud Backend API (Node.js on VPS)
- Real-Time Web Dashboard (WebSocket + HTTP)

The system allows users to scan RFID cards, check balances, and perform top-ups in real-time through a web dashboard.

This implementation strictly follows the required IoT architecture:

> Devices speak MQTT.  
> Users speak HTTP and WebSocket.  
> The backend translates between them.

---

## 🏗 System Architecture

### 1️⃣ ESP8266 (Edge Controller)
- Reads RFID card UID
- Publishes card UID and balance via MQTT
- Subscribes to top-up commands via MQTT
- Does NOT use HTTP or WebSocket

### 2️⃣ Backend API (Node.js on VPS)
- Receives top-up commands via HTTP (`POST /topup`)
- Communicates with ESP8266 via MQTT
- Pushes real-time updates to dashboard via WebSocket

### 3️⃣ Web Dashboard (Frontend)
- Sends top-up requests via HTTP
- Receives real-time balance updates via WebSocket
- Does NOT communicate directly with MQTT

---

## 🔐 MQTT Topic Isolation

All teams share the same MQTT broker.

To prevent interference, this project uses a unique namespace:

