# ZC-ESP32-CAM-Automated-Intrusion-Snapshot-System
ESP32-CAM Automated Intrusion Snapshot System
Project Overview

The ESP32-CAM Automated Intrusion Snapshot System is a simple cybersecurity monitoring project that uses an ESP32-CAM module to simulate an automated surveillance device. The system captures image snapshots every few seconds and logs monitoring events through the Arduino IDE serial monitor.

The purpose of the project is to demonstrate how IoT devices can be used for basic monitoring and surveillance operations in a cybersecurity environment. Instead of creating a complex networking system, this project focuses on camera initialization, automated monitoring, and event logging using embedded hardware.

This project demonstrates:

IoT device deployment
Automated monitoring
Camera-based surveillance concepts
Event logging
Embedded systems programming
Hardware Used
ESP32-CAM (AI Thinker)
USB cable / USB-to-Serial connection
Laptop or PC
Software Used
Arduino IDE
ESP32 Board Package by Espressif Systems
Resources
Arduino IDE

Arduino IDE Download

ESP32 Board Package URL
https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
ESP32 Arduino Repository

ESP32 Arduino GitHub Repository

How the Project Works

The ESP32-CAM continuously captures image frames and prints monitoring information to the serial monitor every 5 seconds.

The system:

Initializes the camera
Starts monitoring
Captures snapshots
Logs event information
Repeats automatically

This simulates a lightweight intrusion monitoring camera system.

System Flow
System Boot
    ↓
Camera Initialization
    ↓
Monitoring Activated
    ↓
Capture Snapshot
    ↓
Log Event Information
    ↓
Repeat Every 5 Seconds
Project Code
#include "esp_camera.h"

#define PWDN_GPIO_NUM     32
#define RESET_GPIO_NUM    -1
#define XCLK_GPIO_NUM      0
#define SIOD_GPIO_NUM     26
#define SIOC_GPIO_NUM     27

#define Y9_GPIO_NUM       35
#define Y8_GPIO_NUM       34
#define Y7_GPIO_NUM       39
#define Y6_GPIO_NUM       36
#define Y5_GPIO_NUM       21
#define Y4_GPIO_NUM       19
#define Y3_GPIO_NUM       18
#define Y2_GPIO_NUM        5

#define VSYNC_GPIO_NUM    25
#define HREF_GPIO_NUM     23
#define PCLK_GPIO_NUM     22

int snapshotCount = 0;

void setup() {

  Serial.begin(115200);

  Serial.println();
  Serial.println("ESP32-CAM Intrusion Monitor");

  camera_config_t config;

  config.ledc_channel = LEDC_CHANNEL_0;
  config.ledc_timer = LEDC_TIMER_0;

  config.pin_d0 = Y2_GPIO_NUM;
  config.pin_d1 = Y3_GPIO_NUM;
  config.pin_d2 = Y4_GPIO_NUM;
  config.pin_d3 = Y5_GPIO_NUM;
  config.pin_d4 = Y6_GPIO_NUM;
  config.pin_d5 = Y7_GPIO_NUM;
  config.pin_d6 = Y8_GPIO_NUM;
  config.pin_d7 = Y9_GPIO_NUM;

  config.pin_xclk = XCLK_GPIO_NUM;
  config.pin_pclk = PCLK_GPIO_NUM;
  config.pin_vsync = VSYNC_GPIO_NUM;
  config.pin_href = HREF_GPIO_NUM;

  config.pin_sscb_sda = SIOD_GPIO_NUM;
  config.pin_sscb_scl = SIOC_GPIO_NUM;

  config.pin_pwdn = PWDN_GPIO_NUM;
  config.pin_reset = RESET_GPIO_NUM;

  config.xclk_freq_hz = 20000000;
  config.pixel_format = PIXFORMAT_JPEG;

  config.frame_size = FRAMESIZE_QVGA;
  config.jpeg_quality = 12;
  config.fb_count = 1;

  esp_err_t err = esp_camera_init(&config);

  if (err != ESP_OK) {
    Serial.println("Camera initialization failed");
    return;
  }

  Serial.println("Camera initialized successfully");
}

void loop() {

  Serial.println("---------------------------------");

  Serial.print("Intrusion Event ID: ");
  Serial.println(snapshotCount);

  camera_fb_t * fb = esp_camera_fb_get();

  if (!fb) {
    Serial.println("Snapshot capture failed");
    return;
  }

  Serial.println("Snapshot captured successfully");

  Serial.print("Image Size (bytes): ");
  Serial.println(fb->len);

  esp_camera_fb_return(fb);

  snapshotCount++;

  delay(5000);
}
Installation and Setup
Step 1 — Install Arduino IDE

Download and install:
Arduino IDE Official Website

Step 2 — Add ESP32 Board Support

Open Arduino IDE and go to:

File → Preferences

Paste this into “Additional Boards Manager URLs”:

https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json

Click OK.

Step 3 — Install ESP32 Boards

Go to:

Tools → Board → Boards Manager

Search for:

ESP32

Install:

esp32 by Espressif Systems
Step 4 — Connect ESP32-CAM

Connect the ESP32-CAM to the computer using the USB connection.

Step 5 — Configure Board Settings

Use these settings inside Arduino IDE:

Setting	Value
Board	AI Thinker ESP32-CAM
Upload Speed	115200
PSRAM	Enabled
Partition Scheme	Huge APP
Port	Correct COM Port
Step 6 — Upload the Code
Create a new sketch
Paste the project code
Click Upload

If upload pauses on:

Connecting...

Press the RESET button on the ESP32-CAM once.

Step 7 — Open Serial Monitor

Go to:

Tools → Serial Monitor

Set the baud rate to:

115200
Example Output
ESP32-CAM Intrusion Monitor

Camera initialized successfully

---------------------------------
Intrusion Event ID: 0
Snapshot captured successfully
Image Size (bytes): 18234
---------------------------------

The monitoring process repeats automatically every 5 seconds.

Important Parts of the Code
Camera Initialization
esp_err_t err = esp_camera_init(&config);

This initializes the ESP32 camera hardware using the selected camera settings.

Snapshot Capture
camera_fb_t * fb = esp_camera_fb_get();

This captures an image frame from the camera module.

Event Logging
Serial.println("Snapshot captured successfully");

This logs monitoring activity through the serial monitor.

Troubleshooting
Upload Gets Stuck on “Connecting…”
Fix

Press the RESET button on the ESP32-CAM during upload.

Camera Initialization Failed
Possible Causes
Wrong board selected
PSRAM disabled
Incorrect COM port
Fix

Make sure:

AI Thinker ESP32-CAM is selected
PSRAM is enabled
Correct COM port is selected
Serial Monitor Shows Random Symbols
Fix

Set the baud rate to:

115200
Challenges Encountered

One challenge during the project was configuring the ESP32 board correctly in the Arduino IDE. Selecting the wrong board or COM port caused upload and camera initialization issues.

Another issue occurred during uploads when the ESP32-CAM became stuck on the “Connecting…” screen. This was solved by pressing the reset button during the upload process.

Since the project uses built-in ESP32 camera libraries and minimal hardware, troubleshooting was relatively manageable.


Screenshots and Photos 

ESP32-CAM hardware setup
<img width="1404" height="1599" alt="WhatsApp Image 2026-05-23 at 11 14 03 PM" src="https://github.com/user-attachments/assets/a1436992-d628-4be7-be2e-a4a5bd70fa0f" />


Arduino IDE with the project code
<img width="1125" height="894" alt="image" src="https://github.com/user-attachments/assets/54ef3175-e6ec-4dd2-bb84-5a9391848d45" />


Successful upload screen
<img width="1140" height="352" alt="image" src="https://github.com/user-attachments/assets/4267acbd-549b-478d-a629-a8b0638f6ba5" />


Serial monitor showing monitoring logs
<img width="1155" height="1005" alt="Screenshot 2026-05-23 223147" src="https://github.com/user-attachments/assets/51085a0f-814a-4998-a3d3-da7da2bf10ae" />

ESP32-CAM connected to laptop

# Summary

The ESP32-CAM Automated Intrusion Snapshot System successfully demonstrates how an IoT camera device can be used for basic cybersecurity monitoring and surveillance operations. Using the ESP32-CAM and Arduino IDE, the system automatically captures image snapshots and generates monitoring logs through the serial monitor.

The project highlights important cybersecurity concepts such as:

* Automated monitoring
* Event logging
* Embedded surveillance systems
* IoT device deployment

One of the main goals of the project was to create a simple and reliable monitoring system without requiring complex networking or additional hardware components. Throughout development, challenges such as ESP32 board configuration, upload issues, and camera initialization errors were encountered and resolved through proper setup and troubleshooting.

Overall, the project provides a practical example of how embedded IoT devices can support security monitoring functions in real-world environments while remaining affordable, lightweight, and easy to deploy.
