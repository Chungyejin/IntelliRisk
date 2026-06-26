# IntelliRisk

This project is available in multiple languages.
- [한국어](README.ko.md)
- [Português](README.pt.md)

# IntelliRisk: Smart Infectious Disease Forecasting Platform

**IntelliRisk** is an end-to-end smart infectious disease forecasting platform that integrates an IoT hardware system with a web-based dashboard to monitor dengue fever outbreak risks in real time.

By analyzing environmental data (temperature and humidity) collected at the edge device (ESP32) level, the system predicts the likelihood of dengue outbreaks, archives the results in a database, and visualizes them on a web dashboard. Concurrently, it controls physical actuators (LEDs, LCD) at the edge to provide intuitive, real-time warning indicators locally.

---

## System Components & Tech Stack

* **Hardware & Sensors:** `ESP32` mainboard, `DHT11` temperature & humidity sensor
* **Actuators:** Tri-color alert `LED`s, `LCD Display` for real-time status updates
* **Backend Architecture:** `Python (Flask)`-based REST API server
* **Data Storage:** `MySQL` (stores time-series environmental data and risk logs)

---

## Key Workflows & Data Pipeline

### 1. Real-Time Data Ingestion
* The DHT11 sensor embedded in the ESP32 periodically measures precise temperature and humidity data.
* The collected environmental data is transmitted over Wi-Fi to the REST API endpoints of the Flask backend server.

### 2. Risk Analysis & Data Storage
* The Flask server runs a custom risk assessment algorithm based on the received data to calculate the probability of a dengue fever outbreak.
* The computed risk level, along with timestamped logs, is securely stored (`INSERT`) in the MySQL database.

### 3. Real-Time Bidirectional Feedback
* **Hardware Control:** Based on the analyzed risk level, the text on the ESP32-connected LCD screen updates dynamically, and the corresponding warning LEDs toggle in real time.
* **Web Monitoring Dashboard:** Administrators and users can track regional temperature/humidity trends and dengue fever risk statistics at a glance via the web dashboard.

---

## Dashboard & System Screenshots

<table width="100%">
  <tr>
    <td width="50%"><img src="https://github.com/user-attachments/assets/638825a6-7b35-4d36-8c6a-6f54e289bc61" width="100%"/></td>
    <td width="50%"><img src="https://github.com/user-attachments/assets/0316fda4-4dcb-4a0d-9499-e9d91f2c8fae" width="100%"/></td>
  </tr>
  <tr>
    <td width="50%"><img src="https://github.com/user-attachments/assets/61cd375b-c995-4298-864b-a6bbff0a2424" width="100%"/></td>
    <td width="50%"><img src="https://github.com/user-attachments/assets/18bcc3c1-5de0-4238-be01-ae33d8f90cfe" width="100%"/></td>
  </tr>
  <tr>
    <td width="50%"><img src="https://github.com/user-attachments/assets/653192c4-7066-4d67-b99a-13b575ae5b11" width="100%"/></td>
    <td width="50%"><img src="https://github.com/user-attachments/assets/57bde822-8a70-4d15-8af4-6a15fda5df34" width="100%"/></td>
  </tr>
  <tr>
    <td width="50%"><img src="https://github.com/user-attachments/assets/e80a4598-8b3c-47c6-9b3b-41142cf1510f" width="100%"/></td>
    <td width="50%"><img src="https://github.com/user-attachments/assets/6d0c5161-9b97-46a8-bef3-6ca369ec0bb0" width="100%"/></td>
  </tr>
</table>

---

## Team Members

* **Yejin Chung**
* **Ana Flávia**
* **Isabella Berkembrock**
* **Michele Cristina Otta**

