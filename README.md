# 🫀 CrowdSense
### Real-time IoT-Based Crowd Health Monitoring System

> *Making Large Gatherings Safer, One Beat at a Time!*
---

## 📋 Overview

**CrowdSense** is a comprehensive IoT-based crowd health monitoring system designed for large events like concerts, festivals, and gatherings. It monitors **heart rate** and **blood oxygen levels (SpO2)** of individuals in real-time, detects anomalies, and provides instant visual feedback through color-coded indicators — while sending data to a central dashboard for crowd management.

---

## 🚨 Problem Statement

In large crowds (music festivals, sporting events, marathons), identifying individuals experiencing medical emergencies is extremely difficult. CrowdSense addresses:

- ❤️ Heart attacks or strokes
- 🌡️ Heat exhaustion and dehydration
- 🫁 Respiratory issues
- 😰 Panic attacks

---

## ✅ Features

- 📡 **Real-time health monitoring** for each individual
- 💡 **Visual alerts** via RGB LED ring & OLED display
- 🗺️ **Centralized dashboard** with live heat map visualization
- ⚠️ **Immediate anomaly detection** and alerts
- 📊 **Historical data tracking** for post-event analysis
- 🔌 **Scalable architecture** — supports unlimited devices

---

## 🏗️ System Architecture

```
ESP8266 (Device 1 - Entrance)  ─┐
ESP8266 (Device 2 - Stage)     ─┼──► WiFi Network ──► Node.js Backend (Port 3000) ──► Web Dashboard
ESP8266 (Device N - Food Court) ─┘
```

---

## 🔧 Hardware Components

| Component | Model | Purpose | Qty |
|-----------|-------|---------|-----|
| Microcontroller | ESP8266 (NodeMCU) | Processing + WiFi | 1 per user |
| Health Sensor | MAX30102 | Measure BPM & SpO2 | 1 per device |
| OLED Display | 0.96" SSD1306 | Local display | 1 per device |
| RGB LED Ring | WS2812B / NeoPixel | Visual status | 1 per device |
| Power Supply | 5V USB | Power source | 1 per device |

---

## 🔌 Wiring (ESP8266 NodeMCU Pin Connections)

| Component | VIN | GND | SCL | SDA | Data |
|-----------|-----|-----|-----|-----|------|
| MAX30102 | 3V3 | GND | D1 (GPIO5) | D2 (GPIO4) | — |
| OLED SSD1306 | 3V3 | GND | D1 (GPIO5) | D2 (GPIO4) | — |
| RGB LED Ring | 5V | GND | — | — | D4 (GPIO2) |

---

## 🎨 Color Code System

### LED Ring

| Color | Status | Meaning |
|-------|--------|---------|
| 🟢 Green | Normal | HR 60–100 BPM, SpO2 95–100% |
| 🟠 Orange | Warning | HR 100–120 BPM or SpO2 90–94% |
| 🔴 Red (Flash) | Critical | HR > 120 BPM or SpO2 < 90% |
| 🟡 Yellow | Low HR | HR < 50 BPM |
| 🔵 Blue | System Ready / Measuring | Waiting or analyzing |
| ⚪ White (Flash) | Heartbeat | Pulse detected |

### Heat Map (Dashboard)

| Color | Risk Level | Health Condition |
|-------|-----------|-----------------|
| 🔵 Blue | No Risk | All normal / no data |
| 🟢 Green | Low Risk | HR 60–100 BPM, SpO2 95–100% |
| 🟡 Yellow | Medium Risk | HR 100–110 BPM or SpO2 92–94% |
| 🟠 Orange | Medium-High | HR 110–120 BPM or SpO2 90–92% |
| 🔴 Red | High Risk | HR > 120 BPM or SpO2 < 90% |
| 🟥 Dark Red | Critical | Multiple critical parameters |

---

## 📊 Health Parameters

| Parameter | Normal | Warning | Critical |
|-----------|--------|---------|----------|
| Heart Rate | 60–100 BPM | 100–120 BPM | > 120 or < 50 BPM |
| SpO2 | 95–100% | 90–94% | < 90% |

### Risk Score Formula

```
Risk Score = (HR Risk Factor + SpO2 Risk Factor) / 2
```

| Condition | HR Risk Factor | SpO2 Risk Factor |
|-----------|---------------|-----------------|
| Normal | 0.0 | 0.0 |
| Elevated | 0.3 | 0.2 |
| Warning | 0.6 | 0.5 |
| Critical | 1.0 | 1.0 |

---

## 🗺️ Event Zones

| Zone ID | Name | Default Color |
|---------|------|--------------|
| ENTRANCE A | Main Entrance | 🔵 Blue |
| ZONE A | Stage Area | 🔴 Red |
| ZONE B | Food Court | 🟠 Orange |
| ZONE C | Rest Area | 🟢 Green |
| VIP ZONE | VIP Section | 🟣 Purple |

---

## 💻 Software & Technologies

### Firmware (Arduino IDE)
- **Language:** C++
- **Libraries:** `Wire.h`, `MAX30105.h`, `Adafruit GFX`, `Adafruit SSD1306`, `ESP8266WiFi`, `Adafruit NeoPixel`

### Backend (Node.js)
- **Framework:** Express.js
- **Dependencies:** `express`, `cors`, `fs`

### Frontend (Web Dashboard)
- HTML5, CSS3, JavaScript
- **Leaflet.js** — Heat map visualization
- **Chart.js** — Data visualization

---

## 🌐 API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/heartrate` | POST | Receive health data from ESP8266 |
| `/api/latest` | GET | Get latest 20 readings |
| `/api/all` | GET | Get all stored readings |
| `/api/stats` | GET | Get statistics (avg, min, max) |
| `/api/alerts` | GET | Get active alerts |
| `/api/alerts` | DELETE | Clear all alerts |

### POST Data Format

```json
{
  "user_id": "CROWD_USER_001",
  "location": "ZONE_A",
  "heart_rate": 85,
  "spo2": 96,
  "signal_quality": 50,
  "total_beats": 42
}
```

---

## 🚀 Getting Started

### 1. Install Arduino Libraries
Open Arduino Library Manager and install:
- MAX30105 by SparkFun
- Adafruit GFX Library
- Adafruit SSD1306
- Adafruit NeoPixel

### 2. Flash Firmware
Open `ESP8266 Code/crowdsense.ino` in Arduino IDE, set your WiFi credentials, and flash to the ESP8266.

### 3. Start Backend Server
```bash
cd crowdsense-backend
npm install express cors
npm start
```

### 4. Open Dashboard
Navigate to `http://localhost:3000` in your browser.

---

## 📁 File Structure

```
crowdsense-backend/
├── public/
│   └── index.html        # Dashboard with color-coded UI
├── server.js             # Backend server with color logic
├── package.json          # Node dependencies
├── data.json             # Stored readings (auto-generated)
└── node_modules/         # Dependencies (auto-generated)

ESP8266 Code/
└── crowdsense.ino        # Main firmware with LED color control
```

---

## ⚡ Performance Metrics

| Metric | Value |
|--------|-------|
| Heart Rate Accuracy | ±5 BPM |
| SpO2 Accuracy | ±2% |
| Data Transmission Delay | < 1 second |
| Dashboard Update Rate | Every 2 seconds |
| Battery Life (2000mAh) | ~8 hours |
| Alert Response Time | < 3 seconds |
| Data Storage | 1000 readings/session |

---

## 🔍 Troubleshooting

| Issue | Possible Cause | Solution |
|-------|---------------|---------|
| Sensor not detected | Wrong wiring | Check 3.3V power, SCL/SDA |
| No BPM readings | Finger placement | Use index finger, gentle pressure |
| Low SpO2 readings | Cold hands | Warm hands before testing |
| WiFi fails | Wrong credentials | Verify SSID and password |
| Data not sending | Backend not running | Run `npm start` |
| LED ring not lighting | Wrong pin | Connect NI to D4 (GPIO2) |

---

## 🔮 Future Enhancements

- 📱 Mobile App (iOS/Android) with color alerts
- 📍 GPS integration for precise location tracking
- 🤖 AI-powered predictive analytics for risk prediction
- ☁️ Cloud deployment (AWS/Azure)
- 🗄️ Database integration (MongoDB/MySQL)
- 📧 Real-time color-coded email/SMS notifications
- ♿ Color-blind friendly mode
- 📄 Export reports (PDF/CSV) with color graphs

---

## 🏟️ Applications

- 🎵 Music festivals (e.g., Sunburn)
- 🏆 Sporting events & marathons
- 🏥 Healthcare facilities
- 👴 Elderly care centers
- 🎪 Large public gatherings

---

## 📚 References

1. MAX30102 Datasheet — Analog Devices
2. ESP8266 Technical Reference — Espressif Systems
3. Node.js Documentation — Node.js Foundation
4. Arduino Reference — Arduino LLC
5. IEEE Standards for IoT Healthcare Systems

---

## 🙏 Acknowledgments

Special thanks to the open-source community for providing excellent libraries and tools that made this project possible.

---
Team

Hariduthram PS
SD Kowshik Raj
J Subhi
S Hareehareen Heidenreich
---
*CrowdSense Development Team | March 2026 | [github.com/crowdsense](https://github.com/crowdsense)*
