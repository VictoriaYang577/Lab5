# Lab 5: Gesture Recognition with Edge Impulse and ESP32

## Overview

This project implements a gesture recognition system using an ESP32 microcontroller and an MPU6050 accelerometer. The system captures accelerometer data corresponding to specific hand gestures, trains a machine learning model using Edge Impulse, and deploys the model back onto the ESP32 for real-time inference. Recognized gestures trigger LED indicators and send the results to a local server.

## Objectives

- Collect and preprocess accelerometer data for predefined gestures.
- Train a machine learning model using Edge Impulse.
- Deploy the trained model onto the ESP32 microcontroller.
- Implement real-time gesture recognition with LED feedback.
- Send recognized gestures to a local server via HTTP POST requests.

## Hardware Components

- ESP32 Development Board
- MPU6050 Accelerometer and Gyroscope Module
- Red, Green, and Blue LEDs
- Push Button
- Resistors (appropriate values for LEDs and button)
- Breadboard and Jumper Wires

## Software Components

- Arduino IDE with libraries:
  - Adafruit MPU6050
  - Adafruit Unified Sensor
  - WiFi
  - HTTPClient
  - ArduinoJson
- Edge Impulse Studio
- Python 3.x with:
  - Flask
  - pandas
  - numpy
  - scikit-learn
  - matplotlib

## Repository Structure

Lab5/
├── ESP32_to_cloud/        # Arduino code for ESP32
├── app/                   # Flask server application
├── data/                  # Collected gesture data
├── trainer_scripts/       # Python scripts for data preprocessing and model training
└── README.md              # Project documentation

## Setup and Implementation

### 1. Hardware Configuration

**MPU6050 Connections:**
- VCC to 3.3V
- GND to GND
- SDA to GPIO21
- SCL to GPIO22

**LED Connections:**
- Red LED to GPIO23
- Green LED to GPIO22
- Blue LED to GPIO21

**Button Connection:**
- Button to GPIO4 with a pull-up resistor

### 2. Data Collection

- Upload the `ESP32_to_cloud` Arduino sketch to the ESP32.
- Press the button to start data capture for a specific gesture.
- Perform the gesture during the capture duration (1 second).
- Repeat for each gesture (`O`, `V`, `Z`) to collect sufficient samples.
- Save the collected data as CSV files in the `data/` directory, organized by gesture class.

### 3. Model Training with Edge Impulse

- Create a new project in Edge Impulse Studio.
- Upload the collected CSV files to the respective classes.
- Design an impulse with appropriate preprocessing (e.g., spectral features) and a neural network classifier.
- Train the model and evaluate its performance.
- Deploy the model as an Arduino library and include it in the `ESP32_to_cloud` sketch.

### 4. Real-Time Inference and Server Communication

- The ESP32 captures accelerometer data upon button press.
- The onboard model processes the data and predicts the gesture.
- Corresponding LED lights up based on the recognized gesture:
  - `O`: Red LED
  - `V`: Red and Blue LEDs
  - `Z`: Blue LED
- The ESP32 sends a JSON payload with the gesture and confidence score to the Flask server running locally.

### 5. Flask Server Setup

- Navigate to the `app/` directory.
- Install the required Python packages using:

  ```bash
  pip install -r requirements.txt

	•	Run the server:

python app.py


	•	The server listens for POST requests at http://127.0.0.1:8000/predict and logs the received data.

Results
	•	The system successfully recognizes predefined gestures with high accuracy.
	•	LED indicators provide immediate visual feedback for the recognized gestures.
	•	The Flask server receives and logs the gesture data sent from the ESP32.

Challenges and Solutions

WiFi Connectivity Issues:
	•	Challenge: Intermittent WiFi connections caused data transmission failures.
	•	Solution: Implemented retry logic and ensured a stable power supply to the ESP32.

Gesture Misclassification:
	•	Challenge: Similar gestures (V and Z) were occasionally misclassified.
	•	Solution: Collected more training data and fine-tuned the model parameters in Edge Impulse.

Conclusion

This project demonstrates the integration of embedded systems with machine learning for real-time gesture recognition. By leveraging Edge Impulse for model training and deploying the model onto an ESP32, we achieved an efficient and responsive system capable of recognizing hand gestures and communicating the results to a server.

References
	•	Edge Impulse Documentation
	•	Adafruit MPU6050 Library
	•	ArduinoJson Library
	•	ESP32 WiFi Library

![Prototype 1](https://drive.google.com/uc?export=view&id=1FYEpmoGtxOOSCjtihYlTiN4M1DHAi0sd)
![Prototype 2](https://drive.google.com/uc?export=view&id=1EFkFkxHk0lvWunT2gCfgu-0g9MP13dCf)



Let me know if you’d like to add any images, GIFs of the prototype, or model accuracy graphs!
