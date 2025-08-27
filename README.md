# Qingping_Barometer_ProS_MQTT

**BLE-to-MQTT bridge for Qingping Temp & RH Monitor (Barometer) Pro S (CGP23W) using ESP32-C3, with full Home Assistant integration.**

- 📌 For **Qingping CO₂ Temp RH (CGP22C)** setup, please refer to my other repository:  
- 👉 [Qingping_CO2_Temp_RH_MQTT](https://github.com/markhsieh2020/Qingping_CO2_Temp_RH_MQTT)  

---

## Project Overview
This project uses an **ESP32-C3 mini** as a BLE-to-MQTT bridge for the **Qingping Temp & RH Monitor (Barometer) Pro S**.  
All sensor data is published via **MQTT Autodiscovery** for seamless integration with **Home Assistant**.

- The Barometer Pro S continuously broadcasts data over BLE.  
- In this example, the ESP32-C3 is configured to **publish to MQTT every 5 seconds**.  
- The publish interval can be modified in the source code.  

---

## Features
- **BLE Scanning & Parsing** (UUID `0xFDCD`)  
- **Sensor Data**:  
  - Temperature (°C)  
  - Humidity (%)  
  - Pressure (kPa)  
  - Battery (%)  
  - BLE RSSI (dBm)  
- **Diagnostics**: Boot Count, LAN IP, WAN IP, MAC address, MQTT error count, Reset Reason, Wi-Fi BSSID, Wi-Fi Channel, Wi-Fi Reconnects, Wi-Fi RSSI Proxy, Wi-Fi SSID, Uptime.  
- **Controls**:  
  - Restart Button (via MQTT in Home Assistant)  
  - On-board LED Switch (blinks 3x on boot, defaults OFF)  
- **Home Assistant Autodiscovery**: automatically creates entities.  
- **Reliability**: Auto-reconnect Wi-Fi & MQTT, WAN IP refresh, persistent boot counter stored in NVS.  

---

## Hardware Requirements
- **Board**: ESP32-C3 mini (4 MB Flash, Wi-Fi + BLE 5)  
- **Sensor**: Qingping Temp & RH Monitor (Barometer) Pro S  
- **On-board LED**: GPIO8 (active LOW, blinks 3x at boot, default OFF)  

---

## Software Requirements
- **Arduino IDE** version 2.3.6 or higher  
- **ESP32 Arduino Core** version 2.0 or higher  
- **Required Libraries**:  
  1. **NimBLE-Arduino** v2.3.4  
  2. **PubSubClient** v2.8  
  3. **ArduinoJson** v7.4.2  

---

## Installation
1. Open this file in your browser:  
   👉 [esp32c3_qingping_Barometer_mqtt.txt](https://github.com/markhsieh2020/Qingping_CO2_Temp_RH_MQTT/blob/main/esp32c3_qingping_co2_mqtt.txt)  
2. Copy all the code inside the `.txt` file.  
3. Update Wi-Fi and MQTT credentials:  
   ```cpp
   const char* ssid        = "YOUR_WIFI_SSID";
   const char* password    = "YOUR_WIFI_PASSWORD";
   const char* mqtt_server = "YOUR_MQTT_SERVER";
   const int   mqtt_port   = 1883;
   const char* mqtt_user   = "mqtt_user";
   const char* mqtt_pass   = "mqtt_pass";
   ```
4. Replace with your **Barometer Pro S BLE MAC address**:  
   ```cpp
   static const char* TARGET_BLE_ADDR = "58:2d:34:xx:xx:xx";
   ```
   You can find the MAC address in the **Qingping+ App → Device Info**.  
5. Compile and flash to ESP32-C3.  

---

## MQTT Configuration
- **Discovery Prefix**: `homeassistant`  
- **MQTT Topics Example**:  
  - State: `ESP32_C3_BLE_xxxxxx/state`  
  - Availability: `ESP32_C3_BLE_xxxxxx/availability`  
  - Commands:  
    - Restart → `ESP32_C3_BLE_xxxxxx/cmd/restart`  
    - LED → `ESP32_C3_BLE_xxxxxx/cmd/led`  

---

## BLE Advertisement Parsing
Example BLE broadcast packet from Barometer Pro S:  

```
== ADV == name="Qingping Barometer Pro S" addr=58:2d:34:83:e1:f0 rssi=-52
FDCD SD : 88 33 0C 54 84 34 2D 58 01 04 23 01 D2 01 02 01 64 07 02 58 27
```

### Decoded Values
- Temperature: 29.1 °C  
- Humidity: 46.6 %  
- Battery: 100 %  
- Pressure: 100.72 kPa  
- RSSI: –52 dBm  

---

## Tested Environment
- **Development**: Arduino IDE 2.3.6  
- **ESP32 Core**: 2.0+  
- **Libraries**: NimBLE-Arduino v2.3.4, PubSubClient v2.8, ArduinoJson v7.4.2  
- **Hardware**: ESP32-C3 mini + Qingping Barometer Pro S  
- **Home Assistant**: OS 16.0, Supervisor 2025.08.1, HA 2025.7.4  

---

## Tested Devices

| Model                                | Firmware | Tested | Notes                |
|--------------------------------------|----------|--------|----------------------|
| Qingping Temp & RH Monitor Pro S     | 2.0.6   | Yes    | BLE parsing verified |

---

## Troubleshooting
- **No entities in Home Assistant?** → Ensure `homeassistant/` MQTT discovery is enabled.  
- **Wi-Fi disconnects?** → Use a stable 2.4GHz Wi-Fi network.  
- **Incorrect values?** → Verify your sensor’s MAC address and firmware version.  

---

## License
This project is licensed under the **MIT License**. See [LICENSE](LICENSE).  

---

## Acknowledgements
- [NimBLE-Arduino](https://github.com/h2zero/NimBLE-Arduino)  
- [ArduinoJson](https://arduinojson.org/)  
- [PubSubClient](https://pubsubclient.knolleary.net/)  
