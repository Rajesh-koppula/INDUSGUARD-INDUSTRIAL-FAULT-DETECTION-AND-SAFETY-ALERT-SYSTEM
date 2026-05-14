📌 **Overview:**

INDUSGUARD is an embedded industrial monitoring and safety alert system designed to detect abnormal environmental conditions in industrial 
environments and notify responsible personnel through GSM-based SMS alerts.

The system continuously monitors temperature and humidity using sensors connected to the LPC2148
ARM7 microcontroller. When the sensed values exceed predefined threshold limits, the system generates
alerts using LEDs/Buzzer and sends SMS notifications with timestamps.

**This project improves:**

Industrial safety
Fault detection reliability
Remote monitoring capability
Real-time alert communication

🚀** Features:**
🌡️ Real-time temperature monitoring
💧 Humidity monitoring using DHT11
📟 LCD display for live sensor data
📲 GSM-based SMS alerts
🔐 Password-protected configuration
💾 EEPROM storage for settings
⌨️ Local configuration using keypad
⚠️ Fault indication using LEDs/Buzzer
📡 Remote parameter update via SMS
🕒 RTC-based timestamp support

🛠 **Hardware Components:**

| Component          | Description                   |
| ------------------ | ----------------------------- |
| LPC2148            | ARM7 Microcontroller          |
| DHT11              | Temperature & Humidity Sensor |
| GSM Module (M660A) | SMS Communication             |
| AT24C256           | EEPROM Memory                 |
| LCD Display        | System Status Display         |
| 4x4 Matrix Keypad  | User Input                    |
| LEDs/Buzzer        | Fault Indication              |
| RTC                | Timestamp Generation          |


💻 **Software Requirements**
Embedded C
Keil uVision IDE
Flash Magic
UART Terminal / HyperTerminal

INDUSGUARD/
│
├── lcd.c
├── lcd.h
├── delay.c
├── delay.h
├── keypad.c
├── keypad.h
├── uart.c
├── uart.h
├── i2c.c
├── i2c.h
├── adc.c
├── adc.h
├── dht11.c
├── dht11.h
├── gsm.c
├── gsm.h
├── eeprom.c
├── eeprom.h
├── rtc.c
├── rtc.h
├── projectmain.c

**Project Flow:**

                           ┌─────────────────────┐
                           │      START          │
                           └─────────┬───────────┘
                                     │
                                     ▼
                    ┌────────────────────────────────┐
                    │ Initialize All Peripherals     │
                    │ LCD, GSM, EEPROM, RTC, DHT11  │
                    └────────────────┬───────────────┘
                                     │
                                     ▼
                    ┌────────────────────────────────┐
                    │ Store Default Values in EEPROM │
                    │ • Temp Set Point               │
                    │ • Humidity Set Point           │
                    │ • Password                     │
                    │ • Mobile Number                │
                    └────────────────┬───────────────┘
                                     │
                                     ▼
                    ┌────────────────────────────────┐
                    │ Read Temperature & Humidity    │
                    │ from DHT11 Sensor              │
                    └────────────────┬───────────────┘
                                     │
                                     ▼
                    ┌────────────────────────────────┐
                    │ Display Sensor Values on LCD   │
                    └────────────────┬───────────────┘
                                     │
                                     ▼
                    ┌────────────────────────────────┐
                    │ Read Set Points from EEPROM    │
                    └────────────────┬───────────────┘
                                     │
                                     ▼
                    ┌────────────────────────────────┐
                    │ Compare Current Values with    │
                    │ Stored Set Points              │
                    └────────────────┬───────────────┘
                                     │
                 ┌───────────────────┴───────────────────┐
                 │                                       │
                 ▼                                       ▼
      ┌─────────────────────┐               ┌─────────────────────┐
      │ Temp/Humidity       │      NO       │ Continue Monitoring │
      │ Exceeds Limit ?     │──────────────▶│ Sensor Values       │
      └──────────┬──────────┘               └─────────────────────┘
                 │ YES
                 ▼
      ┌─────────────────────────────────────┐
      │ Turn ON Red LED / Buzzer            │
      └────────────────┬────────────────────┘
                       │
                       ▼
      ┌─────────────────────────────────────┐
      │ Get Current Time from RTC           │
      └────────────────┬────────────────────┘
                       │
                       ▼
      ┌─────────────────────────────────────┐
      │ Send SMS Alert through GSM Module   │
      └────────────────┬────────────────────┘
                       │
                       ▼
      ┌─────────────────────────────────────┐
      │ Check for Incoming SMS              │
      └────────────────┬────────────────────┘
                       │
            ┌──────────┴──────────┐
            │ SMS Received ?      │
            └───────┬─────────────┘
                    │ YES
                    ▼
      ┌─────────────────────────────────────┐
      │ Verify Sender Mobile Number         │
      └────────────────┬────────────────────┘
                       │
            ┌──────────┴──────────┐
            │ Authorized User ?   │
            └───────┬─────────────┘
                    │ YES
                    ▼
      ┌─────────────────────────────────────┐
      │ Validate SMS Syntax                 │
      │ XXXXCDDDD....$                      │
      └────────────────┬────────────────────┘
                       │
            ┌──────────┴──────────┐
            │ Valid Syntax ?      │
            └───────┬─────────────┘
                    │ YES
                    ▼
      ┌─────────────────────────────────────┐
      │ Execute Requested Operation         │
      │                                     │
      │ T → Change Temperature Set Point    │
      │ M → Change Mobile Number            │
      │ I → Send Sensor Information         │
      └────────────────┬────────────────────┘
                       │
                       ▼
      ┌─────────────────────────────────────┐
      │ Update EEPROM / Send SMS Response   │
      └────────────────┬────────────────────┘
                       │
                       ▼
                ┌───────────────┐
                │ Continue Loop │
                └──────┬────────┘
                       │
                       ▼
      ┌─────────────────────────────────────┐
      │ External Interrupt Triggered ?      │
      └────────────────┬────────────────────┘
                       │ YES
                       ▼
      ┌─────────────────────────────────────┐
      │ Display Menu on LCD                 │
      │ 1. Set Point Change                 │
      │ 2. Password Change                  │
      └────────────────┬────────────────────┘
                       │
                       ▼
      ┌─────────────────────────────────────┐
      │ Enter Current Password              │
      └────────────────┬────────────────────┘
                       │
            ┌──────────┴──────────┐
            │ Password Correct ?  │
            └───────┬─────────────┘
                    │ YES
                    ▼
      ┌─────────────────────────────────────┐
      │ Allow User to Modify Settings       │
      │ Save Updated Values in EEPROM       │
      └────────────────┬────────────────────┘
                       │
                       ▼
                ┌───────────────┐
                │ Return to Main│
                │ Monitoring    │
                └───────────────┘
