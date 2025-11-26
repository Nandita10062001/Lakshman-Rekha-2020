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

## 🔧 Installation

Clone the repo and run -
- For windows:
```
pip install -r requirements.txt
```

#### ⚙️ Hardware
Change the ip in `Jatayu/user_validation` on line 4 and `Jatayu/wifi_functions` on line 36 and line 58. Then connect NodeMCU to your pc and upload the code in the folder.<br>
Then your NodeMCU will go into access point mode if it fails to connect to a wifi. When it does, open your browser and type `http://192.168.4.1/` and hit enter.<br>
Then enter the `WiFi` and `Patient/User` credentials and hit submit. This will save the data to the eeprom of the NodeMCU.

#### 💻 Software

Download and install xampp. Start the xampp control panel and start Apache and MySQL. Then open your browser and type`http://localhost/phpmyadmin/`.<br>
Then select `new` and create a new database named `lakshmanrekha`.

Once done, run the following commands to run the website -
```
cd LakshmanRekha/
python manage.py runserver
```

## 📈 How It Works
<br>
Location Setup:<br>

https://github.com/user-attachments/assets/34b80ee4-221d-49b0-914d-79cbc06ad54e


Website Working Demo:<br>

https://github.com/user-attachments/assets/23976915-4135-4c77-b18e-3c2ae0ded346





Working:<br>

https://github.com/user-attachments/assets/1c7ef6a6-81af-4b27-a5cf-9e8ecf95201a











## 🎉 Acknowledgment

This project was created for Deep Blue Season 6, a national-level hackathon by Mastek.
Our team proudly qualified for the semifinals, and this solution was built end-to-end during the event.




