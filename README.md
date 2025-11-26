<h1 align="center">🚧 LakshmanRekha – Indoor Positioning & Geofencing System</h1>

<p align="center">
  <b>Built for Deep Blue Season 6 – National Hackathon • Semifinalist</b>
</p>

<p>
<b>LakshmanRekha</b> is a web-based indoor-positioning and geofencing platform paired with an IoT wristband called <b>Jatayu</b> (ESP8266/ESP-12).
The system estimates a patient’s real-time indoor location using Wi-Fi RSSI signals, visualizes them on hospital floor maps, and triggers alerts on geofence breaches.
</p>

---

## 🌍 Problem Context

During the COVID-19 pandemic, hospitals needed a low-cost way to track quarantined or at-risk patients indoors without relying on cameras or constant physical supervision.

<b>LakshmanRekha</b> addresses this by:

- Tracking patients indoors using Wi-Fi RSSI trilateration  
- Allowing hospitals to map floors and define geofenced zones  
- Using a low-cost ESP8266 wristband  
- Triggering alerts when patients leave safe zones  
- Supporting multiple hospitals with isolated accounts  

---

## ✨ Key Features

- 🗺️ **Upload floor maps** and draw custom geofences  
- 📡 **Indoor Positioning** using Wi-Fi RSSI trilateration  
- 🚨 **Alert system** for boundary violations  
- 👥 **Multi-tenant architecture** for hospitals/organisations  
- 📊 **Live tracking dashboard**  
- 📱 **Low-cost IoT wristband** built on ESP8266  
- 🧭 **Access-point mapping & coordinate calibration**  

---

## 📺 Demo Videos

### 🔧 Geofence & Location Setup  


### 📡 End-to-End Tracking Demo  
https://github.com/Nandita10062001/Lakshman-Rekha-2020/issues/1#issue-3668545099

---

## 🧱 System Architecture

### 🏥 1. LakshmanRekha (Django Web App)
Handles:
- Map uploads & polygon drawing  
- RSSI ingestion endpoints  
- Trilateration & coordinate conversion  
- Alerting logic  
- User & organisation accounts  

### 🎛 2. Jatayu (ESP8266 Wristband)
Handles:
- Wi-Fi scanning  
- RSSI sampling  
- Posting data to backend  
- Storing patient/floor/org data in EEPROM  
- AP-mode configuration portal  

---
## 🧰 Tech Stack

- **Backend:** Django 3.2  
- **Database:** MySQL  
- **Frontend:** Django Templates + JS  
- **IoT Hardware:** ESP8266 (NodeMCU / ESP-12E)  
- **Python Libraries:** `numpy`, `opencv-python`, `Shapely`, `Pillow`, `playsound`, `plyer`

---

## 🎉 Acknowledgment

This project was created for Deep Blue Season 6, a national-level hackathon by Mastek.
Our team proudly qualified for the semifinals, and this solution was built end-to-end during the event.




