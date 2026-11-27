[Three tier system architecture.pdf](https://github.com/user-attachments/files/35525429/Three.tier.system.architecture.pdf)
# Smart Guardian: Multimodal IoT Vehicular Safeguard

**Design and Implementation of a Multimodal IoT Vehicular Safeguard: Steering-Embedded DUI Prevention, Hybrid-Storage Black Box, and Real-Time Crash Alerting**[cite: 12]

![Platform](https://img.shields.io/badge/Platform-ESP32-blue)
![Language](https://img.shields.io/badge/Language-C%2B%2B%20%2F%20Arduino-00979D)
![Cloud](https://img.shields.io/badge/Cloud-ThingsBoard-orange)
![License](https://img.shields.io/badge/License-MIT-green)

---

## 📖 Overview

**Smart Guardian** is a low-cost, edge-computing Internet of Things (IoT) safety system built on an ESP32 microcontroller to address two major causes of road fatalities: alcohol-impaired driving and delayed emergency response during the post-crash "golden hour". 

Unlike standalone aftermarket trackers or generalized cabin sensors, Smart Guardian combines **pre-ignition impairment prevention**, **in-transit crash detection**, **automated multi-channel SOS alerting**, and **dual-redundant black box logging** into a single autonomous unit[cite: 12, 15].

---

## ✨ Key Features

* **Selective Pre-Ignition Alcohol Interlock:** Uses a steering-embedded **MQ-3** gas sensor to sample the driver's breath directly—minimizing false positives from passengers. On a 12-bit ADC scale (`0–4095`) with a clean-air baseline of `~500`, readings exceeding the `1500–2000` safety threshold actuate a **5V relay** within a `100 ms` control loop to lock the vehicle's ignition circuit[cite: 15, 16].
* **In-Transit Collision & Rollover Detection:** Continuously monitors 6-axis acceleration and angular velocity via an **MPU6050** IMU to detect severe impacts and abnormal vehicle tilt[cite: 15, 16].
* **10-Second False-Alarm Cancellation Window:** Gives the driver a 10-second countdown (with OLED status feedback and active buzzer warning) to cancel false triggers via a tactile push-button[cite: 15, 16, 18].
* **Automated Multi-Channel SOS Alerts:** Upon an uncancelled crash, fetches live coordinates from a **NEO-6M GPS** receiver and dispatches an emergency SMS with a Google Maps link via a **SIM800L GSM** module, alongside an automated **SMTP email** alert[cite: 15, 16].
* **Hybrid Smart Black Box (Local + Cloud):** Mirrors telemetry locally to a **Micro SD card** (`.CSV` format) via SPI and streams live data over HTTPS/HTTP POST to a **ThingsBoard Cloud** dashboard (`0.2 Hz` / every 5 seconds during normal operation, with immediate zero-latency dispatch on alerts)[cite: 15, 18].

---

## 🏗️ System Architecture
<img width="640" height="222" alt="Three tier system architecture" src="https://github.com/user-attachments/assets/b898f9ff-467b-4015-8ecc-74d2fb8ea3b7" />


The firmware follows a three-tier IoT architecture[cite: 15]:

1. **Perception Layer:** MQ-3 Alcohol Sensor, MPU6050 6-Axis IMU, NEO-6M GPS Receiver, Ignition Switch, and Manual Override Push Button.
2. **Processing & Control Layer:** ESP32 Microcontroller running real-time sensing and decision logic (`100 ms` loop), driving a 5V Ignition Relay, Active Buzzer, and `0.96"` I2C OLED Display[cite: 14, 15].
3. **Communication & Storage Layer:** SIM800L GSM Module (cellular SMS), Wi-Fi HTTP/SMTP client (ThingsBoard Cloud & Gmail alerts), and SPI Micro SD Card Module (local event data recorder)[cite: 14, 15, 18].

---

## 🔌 Hardware Pinout (ESP32 DevKit V1)

| Module | Interface | ESP32 Pin(s) | Power | Notes |
| :--- | :--- | :--- | :--- | :--- |
| **MQ-3 Alcohol Sensor** | Analog (ADC) | `GPIO 35` (`A0`)[cite: 14, 18] | `5V (VIN)`[cite: 14, 18] | Steering-mounted for driver breath isolation[cite: 15] |
| **OLED Display (SSD1306)** | I2C | `GPIO 21` (`SDA`), `GPIO 22` (`SCL`)[cite: 14, 18] | `3.3V`[cite: 14, 18] | Displays status, ADC/alcohol metrics, and countdown[cite: 15, 18] |
| **MPU6050 IMU** | I2C | `GPIO 21` (`SDA`), `GPIO 22` (`SCL`)[cite: 14] | `3.3V`[cite: 14] | Shares I2C bus with OLED[cite: 14] |
| **NEO-6M GPS Module** | UART2 | `GPIO 16` (`RX2`), `GPIO 17` (`TX2`)[cite: 14, 21] | `3.3V / 5V`[cite: 14] | Connect GPS `TX` $\rightarrow$ `GPIO 16`, `RX` $\rightarrow$ `GPIO 17`[cite: 14, 21] |
| **SIM800L GSM Module** | UART | `GPIO 26` (`TXD`), `GPIO 27` (`RXD`)[cite: 14] | `3.7V–4.2V` (Dedicated)[cite: 14] | Requires dedicated LiPo/buck supply (`2A` peak)[cite: 14] |
| **Micro SD Card Module** | SPI | `GPIO 23` (`MOSI`), `GPIO 19` (`MISO`), `GPIO 18` (`SCK`), `GPIO 5` (`CS`)[cite: 14] | `5V`[cite: 14] | Local `.CSV` Black Box storage[cite: 14, 15] |
| **5V Single-Channel Relay** | Digital Out | `GPIO 4` (or `GPIO 14`)[cite: 14, 18] | `5V (VIN)`[cite: 14, 18] | Controls vehicle ignition circuit[cite: 14, 15] |
| **Active Buzzer** | Digital Out | `GPIO 2` (or `GPIO 13`)[cite: 14, 18] | `3.3V` | Local audio warning for lockouts and crash countdown[cite: 14, 15] |
| **Cancel Push Button** | Digital In | `GPIO 13` (Internal Pull-Up)[cite: 14, 21] | `GND`[cite: 14, 21] | Cancels false crash alerts within 10 seconds[cite: 14, 15] |

---

## 🛠️ Software & Libraries

### Prerequisites
* [Arduino IDE](https://www.arduino.cc/en/software) with **ESP32 Board Support Package** installed[cite: 14].
* **ThingsBoard Cloud** account (`thingsboard.cloud`) with a configured device access token.
* **Gmail Account** with 2-Step Verification enabled and an **App Password** generated for SMTP alerts[cite: 14].

### Required Arduino Libraries
Install via **Sketch > Include Library > Manage Libraries**[cite: 14, 17]:
* `Adafruit SSD1306` & `Adafruit GFX Library`[cite: 14]
* `Adafruit MPU6050` & `Adafruit Unified Sensor`[cite: 17]
* `TinyGPSPlus` (for NEO-6M NMEA parsing)
* `ESP Mail Client` by Mobizt[cite: 14, 17]

---

## 🚀 Getting Started

Contact = isurujayanada2002@gmail.com
