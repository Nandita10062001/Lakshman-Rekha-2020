## LakshmanRekha

**LakshmanRekha** is a web-based geofencing and indoor-positioning system designed for monitoring patients using an IoT wristband called **Jatayu** (ESP8266/ESP-12).  
The system tracks a patient’s movement inside a hospital or facility, ensures they remain within a predefined **geofence**, and raises alerts if a breach occurs.

### 🌍 Problem Context

Since the outbreak of COVID-19, hospitals and authorities have needed a reliable way to monitor infected or at‑risk patients without putting staff at unnecessary risk.  
**LakshmanRekha** provides a low-cost, Wi‑Fi–based solution that:

- **Tracks patients indoors** using Wi‑Fi access-point signal strength (RSSI)
- **Defines safe zones (geofences)** over uploaded floor maps
- **Alerts authorities** when patients move outside their permitted zones

---

## ✨ Main Features

- **Geofencing**: Upload floor maps, draw geofences, and associate them with Wi‑Fi access points.
- **Indoor Positioning**: Use trilateration over Wi‑Fi RSSI values from the Jatayu band to estimate the patient’s live location.
- **Alerting**: Trigger alerts when a patient leaves the geofenced area (server-side alert utilities are provided).
- **Multi‑tenant support**: Each organisation (hospital) has its own account and patients.

---

## 🏗 Project Structure

At a high level the repository is split into:

- **`LakshmanRekha/` (Django project)**  
  - `home/`: Landing pages, authentication (login/register), listing patients.  
  - `geofence/`: Upload floor maps, define geofences, associate Wi‑Fi APs to map coordinates.  
  - `tracking/`: Receive live RSSI data from Jatayu devices, validate patients, perform location computation and streaming.
- **`Jatayu/` (ESP8266 firmware)**  
  - `Jatayu.ino`: Main firmware – loads credentials from EEPROM, connects to Wi‑Fi, scans nearby APs, and periodically sends RSSI data to the Django server.  
  - `wifi_functions.ino`: Wi‑Fi scanning and HTTP POST helpers (`/tracking/recieve/`, `/tracking/stream/`).  
  - `user_validation.ino`: Sends user/device details to `/tracking/validate/` for registration/validation.
- **Media & assets**
  - `media/`: Uploaded maps and geofenced output images.
  - `assets/`: Collected static files (Django admin + app CSS/JS).

---

## 🧰 Tech Stack

- **Backend**: Django 3.2 (Python)
- **Database**: MySQL (via `mysqlclient`)
- **Frontend**: Django templates + static CSS/JS
- **Device**: ESP8266 (NodeMCU / ESP-12) running Arduino firmware
- **Key Python libs**: `numpy`, `opencv-python`, `Pillow`, `Shapely`, `playsound`, `plyer`

All Python dependencies are listed in `requirements.txt`.

---

## 🔧 Setup & Installation

### 1. Clone the repository

```bash
git clone https://github.com/<your-org>/LakshmanRekha.git
cd LakshmanRekha-main
```

### 2. Python & Django environment

Create and activate a virtual environment (recommended), then install dependencies:

```bash
pip install -r requirements.txt
```

### 3. MySQL database configuration

Create a MySQL database named **`lakshmanrekha`** (you can use XAMPP/MySQL + phpMyAdmin or the MySQL CLI).

The default database configuration (see `LakshmanRekha/settings.py`) assumes:

- **ENGINE**: `django.db.backends.mysql`
- **NAME**: `lakshmanrekha`
- **USER**: `root`
- **PASSWORD**: `""` (empty)
- **HOST**: `localhost`

Adjust these values in `LakshmanRekha/settings.py` if your setup is different.

Run database migrations:

```bash
cd LakshmanRekha
python manage.py migrate
```

Optionally create a Django superuser to access the admin panel:

```bash
python manage.py createsuperuser
```

### 4. Collect static files (optional, for production‑like setup)

```bash
python manage.py collectstatic
```

### 5. Run the development server

```bash
python manage.py runserver
```

By default the app is served at `http://127.0.0.1:8000/`.

---

## ⚙️ Hardware (Jatayu – ESP8266) Setup

1. **Adjust server IPs**  
   Update the backend server IP/host in:
   - `Jatayu/user_validation.ino` – `sendUserData(...)` endpoint (`/tracking/validate/`)
   - `Jatayu/wifi_functions.ino` – `sendData()` and `liveData()` endpoints (`/tracking/recieve/`, `/tracking/stream/`)

   Set them to your Django server address, for example:
   - `http://192.168.X.X:8000/tracking/validate/`
   - `http://192.168.X.X:8000/tracking/recieve/`
   - `http://192.168.X.X:8000/tracking/stream/`

2. **Build & flash the firmware**
   - Open the `Jatayu` folder in the Arduino IDE (with ESP8266 board support installed).
   - Select the correct **Board** (e.g. NodeMCU 1.0) and **Port**.
   - Upload the sketch to the ESP8266.

3. **Configure Wi‑Fi & patient details**
   - On first boot (or when Wi‑Fi is invalid) the device switches to **Access Point mode**.  
   - Connect to the ESP8266 AP from your phone/laptop and open `http://192.168.4.1/` in a browser.
   - Enter:
     - **Wi‑Fi SSID and password**
     - **Patient name**
     - **Floor name**
     - **Organisation name**
   - The device stores these values in EEPROM and then connects to the configured Wi‑Fi.

Once connected, the device will:

- Periodically **scan nearby Wi‑Fi access points** and build RSSI samples.
- Post compressed readings to the backend at `/tracking/recieve/` and `/tracking/stream/`.
- Send its identity and context to `/tracking/validate/` during validation.

---

## 💻 Web Application Workflow

### 1. Organisation & users

- Organisations (e.g. hospitals) register via the web UI (`/register/`).
- Staff log in at `/login/` and can see their landing dashboard and patient list.

### 2. Geofence creation

Within the **Geofence** module:

1. Upload a floor map image (`/geofence/`).
2. Draw a geofence polygon on top of the map using the web UI.
3. Associate Wi‑Fi APs and measurement points with map coordinates.
4. Saved geofences and AP coordinates are persisted in MySQL (`geofence_map`, `tracking_wifi` tables).

### 3. Tracking & alerts

- The **Tracking** module receives live RSSI values from one or more Jatayu bands.
- Utility code (in `conversion.py`, `trilateration.py`, `tracking_utils.py`) converts RSSI to distance and estimates a location on the floor plan.
- If the computed position lies **outside** the saved geofence, the alert system (`alert_system.py`, `background_check.py`) can be used to notify authorities (UI/system‑notification behaviour can be customised).

---

## 📺 Demo Assets

The repository includes example videos:

- **`location_setup.mp4`** – Demonstrates how the location and geofence are set up.
- **`working.mp4`** – Shows the system working end‑to‑end with a patient device.

You can open these files locally to understand the expected workflow.

---

## 🚀 Development Notes

- Default `DEBUG = True` and MySQL credentials in `settings.py` are suitable **only for local development** – update them before deploying.
- CSRF middleware has been disabled in `MIDDLEWARE` for simplicity when receiving device POSTs; if you expose this system publicly, review and harden security (HTTPS, authentication, CSRF, origin checks, etc.).
- Many DB operations in the `geofence` and `tracking` apps use raw SQL; if you extend the project, consider adding proper models and admin interfaces where possible.

---

## 📄 License

This project does not currently specify a license. If you intend to use or distribute it, please add an appropriate `LICENSE` file.
