# AutoCoop Mini

**A Small-Scale Modular IoT Poultry Automation and Environmental Monitoring System**

AutoCoop Mini is a low-cost, scalable, and modular telemetry ecosystem built specifically for small-scale poultry pens, backyard coops, and hobbyist farmers. Designed to alleviate the heavy physical labor and constant manual supervision required in traditional poultry keeping, AutoCoop Mini automates microclimate control, feeding/watering alerts, and coop security while delivering real-time telemetry and cloud/local control capabilities via an **ESP32 → MQTT Broker → Django VPS** architecture.

---

## Problem Statement

Small-scale and backyard poultry farmers in Sri Lanka face significant challenges in maintaining optimal coop conditions manually. Lack of continuous monitoring can lead to critical failures such as:
* Irregular feeding and watering schedules.
* Rapidly deteriorating air quality (ammonia buildup) and extreme temperature/humidity fluctuations.
* High labor intensity and requirement for constant physical presence on-site.

Existing commercial automation systems target large-scale industrial poultry operations, making them far too expensive, overly complex, and impractical for small coops or micro-scale operations. **AutoCoop Mini** bridges this gap by offering a scalable, affordable, and easy-to-deploy hardware/software solution tailored for micro-farms.

---

## Key Features & Architecture

* **Environmental & Air Quality Telemetry:** Continuous tracking of ambient temperature, humidity, and hazardous gas/ammonia concentration levels.
* **Automated Door Management:** Servo-driven coop door control triggered by dusk/dawn ambient light transitions or custom scheduled timers.
* **Smart Ventilation & Lighting:** Relay-controlled 12V DC ventilation fans and RTC-scheduled artificial lighting loops to optimize bird growth and egg laying cycles.
* **Feed & Water Level Monitoring:** Ultrasonic range sensors track food and water reservoir levels, triggering real-time alerts when levels drop below critical thresholds.
* **Offline-First Local Fail-Safe:** Smart local C++ threshold loops on the ESP32 main controller ensure critical actuators (fans, heaters, doors) continue running safely even during local network or Wi-Fi outages.
* **Modular Slave Expansion:** Wireless low-cost ESP32 slave nodes can be added to expand monitoring across multiple pens using ESP-NOW or MQTT relaying without requiring infrastructure redesigns.

---

## Tech Stack & Architecture

### **Hardware & Sensors**
* **Primary Gateway / Core MCU:** ESP32 Microcontroller
* **Temperature & Humidity:** DHT22 & DS18B20 (Waterproof digital probe)
* **Air Quality & Gas Sensing:** MQ-135 Gas Sensor (Ammonia / Air Quality)
* **Level Measurement:** HC-SR04 Ultrasonic Distance Sensors
* **Timekeeping:** DS3231 RTC (Real-Time Clock) Module
* **Actuators & Controls:** 5V/12V Relay Modules, 12V DC High-CFM Fans, High-Torque Servo Motor (Door Actuator)

### **Firmware & Embedded Software**
* **Language:** C++ / C
* **Framework / IDE:** Arduino IDE / PlatformIO
* **Telemetry Protocol:** MQTT (Message Queuing Telemetry Transport) over Wi-Fi
* **Embedded Libraries:** `PubSubClient`, `WiFi.h`, `DHT sensor library`, `OneWire`, `DallasTemperature`

### **Backend, Cloud & Dashboard Stack**
* **MQTT Broker:** Eclipse Mosquitto (Hosted on Cloud VPS)
* **Backend Framework:** Django / Django REST Framework (DRF)
* **Database:** SQLite (Historical Telemetry & Logs)
* **Real-Time Data Streaming:** Django Channels
* **Web Dashboard:** HTML5,  Bootstrap, Chart.js 
* **Deployment / Server:** Linux VPS (Ubuntu)

---

## System Architecture & Flow

```
+-------------------------------------------------------------------------+
|                            AutoCoop Hardware                            |
|                                                                         |
|  +--------------------+   +-------------------+  +-------------------+  |
|  | DHT22 / DS18B20    |   |  MQ-135 Gas Sensor|  | HC-SR04 Sensors   |  |
|  | Temp & Humidity    |   |  Ammonia / AQI    |  | Feed & Water      |  |
|  +---------+----------+   +---------+---------+  +---------+---------+  |
|            |                        |                      |            |
|            +-------------------+    |    +-----------------+            |
|                                v    v    v                              |
|                       +------------------------+                        |
|                       | ESP32 Master Controller|                        |
|                       |  (Local Logic Loops)   |                        |
|                       +-----------+------------+                        |
|                                   |                                     |
|       +---------------------------+---------------------------+         |
|       |                           |                           |         |
|       v                           v                           v         |
|  +----+------------------+   +----+------------------+   +----+---+----+|
|  | Relay Module (Fans)   |   | Servo Motor (Door)    |   | RTC Light   ||
|  +-----------------------+   +-----------------------+   +-------------+|
+-----------------------------------|-------------------------------------+
                                    |
                            MQTT over Wi-Fi
                                    v
+-------------------------------------------------------------------------+
|                             Cloud VPS Server                            |
|                                                                         |
|                       +-----------------------+                         |
|                       | Eclipse Mosquitto     |                         |
|                       | (MQTT Broker)         |                         |
|                       +-----------+-----------+                         |
|                                   |                                     |
|                                   v                                     |
|                       +-----------------------+                         |
|                       | Django Backend        |                         |
|                       | (MQTT Worker / DRF)   |                         |
|                       +-----------+-----------+                         |
|                                   |                                     |
|                 +-----------------+-----------------+                   |
|                 v                                   v                   |
|      +--------------------+               +--------------------+        |
|      | PostgreSQL DB      |               | Django Channels    |        |
|      | (Historical Logs)  |               | (WebSockets Stream)|        |
|      +--------------------+               +---------+----------+        |
+-----------------------------------------------------|-------------------+
                                                      v
                                        +----------------------------+
                                        | Real-time Web Dashboard    |
                                        | (Custom UI & Controls)     |
                                        +----------------------------+
```

---

## 📊 System Objectives & Project Roadmap

* [ ] **Hardware Prototyping:** Design and integrate temperature, humidity, and ammonia gas sensor nodes.
* [ ] **Firmware Development:** Implement robust embedded C++ local threshold control loops for safe, cloud-independent offline execution.
* [ ] **MQTT & Cloud Pipeline:** Configure Mosquitto MQTT broker on Cloud VPS for real-time telemetry passing into Django application services.
* [ ] **Dashboard Development:** Create dynamic web dashboard using Django and WebSockets for telemetry visualization and remote actuation.
* [ ] **Stress & Field Testing:** Conduct a 7-day operational evaluation in a live small-pen farm environment to assess stability and reliability.

---

## Project Team

**Special Term Team Engineering Project**  
*Department of Electronic and Telecommunication Engineering*  
*University of Moratuwa, Sri Lanka*  
*Date: September 07, 2026*

| Index No. | Name |
| :--- | :--- |
| **250412N** | R. M. Minadith |
| **250423A** | G. L. B. G. S. Nanayakkara |
| **250428T** | L. G. H. M. Nawanjana |
| **250429X** | M. N. Naweed (Project Concept) |

---
