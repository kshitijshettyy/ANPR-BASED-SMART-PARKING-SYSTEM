]# ANPR & OCR Based Smart Parking System

## Overview

This project implements a smart parking system using ANPR (Automatic Number Plate Recognition) and OCR (Optical Character Recognition) technology. The system includes an ESP32CAM, IR sensors, servo motor, RFID reader, and a web server for vehicle authentication.

## Table of Contents

1. [Introduction](#introduction)
2. [Components](#components)
3. [Hardware Setup](#hardware-setup)
4. [Software Setup](#software-setup)
5. [System Flow](#system-flow)
6. [Testing](#testing)
7. [References](#references)

## Introduction

In this project, we use ANPR to automate the process of vehicle authentication at parking lot gates. The system detects a car using an IR sensor, captures the image of the license plate with an ESP32CAM, processes the image with ANPR and OCR, and compares the extracted text to a database of authorized vehicles.

## Components

- **EM-18 RFID Reader**
- **ESP32CAM**
- **Servo Motor**
- **IR Sensor**
- **Arduino Uno** (used only for uploading code to ESP32CAM)
- **5V 2A Power Source**

## Hardware Setup

### Connections

#### Arduino Uno
- RST -> Arduino Uno GND
- TX -> ESP32CAM TXD
- RX -> ESP32CAM RXD

#### ESP32CAM
- GPIO0 -> ESP32CAM GND
- TXD -> Arduino Uno TX
- RXD -> Arduino Uno RX
- GPIO12 -> IR Sensor Digital Pin
- GPIO13 -> EM-18 RFID Reader TX Pin
- GPIO14 -> Servo Motor Digital Pin

### Power and Ground
Connect the power and ground of all components to a 5V 2A power source and ground respectively.

## Software Setup

### Step 1: ANPR Setup

Follow the instructions in the resources below to set up ANPR on your device:
- [YouTube Tutorial](https://www.youtube.com/watch?v=0-4p_QgrdbE)
- [GitHub Repository](https://github.com/nicknochnack/TFODCourse)

### Step 2: Load ESP32CAM Configuration

1. Ensure Step 1 is completed.
2. Start your web server by executing `ANPR-OCR-webserver.ipynb`.
3. Load the ESP32CAM configuration from `CameraWebServerServo.ino` located in `CameraWebServerServo.zip`.
4. Use the same WiFi network SSID and password as the web server device in `CameraWebServerServo.ino`.
5. Use Arduino IDE and Arduino Uno to upload the ESP32CAM configuration:
    - Select "Wrover Module" as the board.
    - On upload completion, if a hard reset is required (as mentioned in the Arduino IDE console), disconnect GPIO0 and GND, then press the reset button on ESP32CAM.
    - Disconnect TX, RX pins, and separate Arduino Uno from the circuit.

### Step 3: Final Testing

Ensure your web server device and ESP32CAM are connected to the same WiFi network. Test the entire circuit for functionality.

## System Flow

1. Car approaches gate, triggering the IR sensor placed in front of the gate.
2. The IR sensor signal prompts the ESP32CAM to send a `/capture` request to the web server.
3. The web server accesses the live stream from the ESP32CAM at `http://{esp32cam_ip}:81/stream`.
4. ANPR is performed on the live stream to locate the number plate.
5. OCR extracts text from the located number plate.
6. The extracted text is compared to the authorized number plates list.
7. A `/rotate90` or `/unauthorized` request is sent back to the ESP32CAM to rotate the servo 90 degrees or signal an unauthorized vehicle respectively.

## Testing

- Ensure all components are connected correctly and powered.
- Verify the web server can receive and process images from the ESP32CAM.
- Confirm the servo motor responds correctly to authorized and unauthorized requests.

---

Feel free to contribute to this project by submitting issues or pull requests. Your feedback is valuable!
