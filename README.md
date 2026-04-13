# Table of Contents
- [Project Description](#project-description)
- [User Interface Specification](#user-interface-specification)
- [Test Plan and Results](#test-plan-and-results)
- [User Manual](#user-manual)
- [Spring Presentation](#spring-presentation)
- [Expo Poster](#expo-poster)
- [Assessments](#assessments)
- [Summary of Hours](#summary-of-hours)
- [Summary of Expenses](#summary-of-expenses)
- [Appendix](#appendix)

---

## Project Description

Plantir is an IoT solution for monitoring and optimizing plant health. It features both embedded systems, a cloud VPS for data processing and storage, and a native Android mobile app for user interface. The embedded system and mobile app solution is a good way to connect the disciplines of our group members -- Computer Engineering and Computer Science. The goal of this project is to show technical depth and build a real-world product with everything that comes with it, such as a user interface, testing, data security, fault tolerance, modularity, and documentation.

The embedded system consists of a Nano ESP32 microcontroller connected to various sensors such as soil moisture, temperature, humidity, and light sensors. Optionally, actuators can also be added, and we currently fully support a water pump, allowing for remote watering of plants. The microcontroller collects data at a user defined interval (default 10 seconds) and sends it to the cloud VPS for processing and storage. Depending on the user's preferences, the plant can be automatically watered when the soil moisture level drops out of an optimal range tailored to the specific plant species. 

The mobile app allows users to view real-time metrics of their plant's health and environment, search for over 2,000+ plant species and their optimal growing conditions, and setup their plant microcontoller with one-touch provisioning. The app also provides notifications and alerts for plant care, such as watering reminders or warnings about unfavorable conditions. The app leverages Bluetooth Low Energy to enable easy setup and provisioning of the microcontroller, allowing the user to connect their plant to the app with a single tap. This provisioning process also enabled the ability for Over-the-air (OTA) updates, allowing us to push firmware updates to the microcontroller without needing physical access to it. Each plant has a soil moisture histogram that allows users to track the moisture levels of their plant over the past 24 hours, identifying when the plant was watered (soil moisture jump $\ge$ 15%). This adds another layer of robustness to our system, helping the user identify if the remote watering is working.

At the press of a button, the user can send water to their plant from anywhere with an internet connection. The user can also set up automatic watering (as previously mentioned), and the system will keep the plant watered without drowning it (system cooldown).

The cloud VPS's main function is to translate between MQTT messages (to and from microcntroller) and PostgreSQL queries (to and from the database). The automatic watering, when enabled, is also handled by the VPS. The VPS is also responsible for user authentication and data security, ensuring that only authorized users can access their plant's data and control the watering system. The VPS is built using Python and Mosquitto MQTT broker, and it uses PostgreSQL (Supabase) for data storage. The VPS is designed to be scalable and can handle multiple users and plants simultaneously, and offers the cloud as a single source of truth.

---

## User Interface Specification

The user interface for Plantir is primarily a native Android mobile app that allows users to interact with their plant's data, control the watering system, view various plant species, and easily provision their plants. The app features a clean and intuitive design, making it easy for users to navigate and access the various features.

Different screens for:
- Home Screen: Display an overview of the user's plants, including their current health status and any notifications or alerts.
- Plant Detail Screen: Show detailed information about a specific plant, including real-time sensor data, historical data, and care recommendations.
- Plant Species Screen: Allow users to search for and view information about different plant species, including their optimal growing conditions and care requirements.
- Provisioning/Add Plant Screen: Guide users through the process of connecting their plant's microcontroller to the app using Bluetooth Low Energy (BLE) for easy setup and provisioning.

Additionally, use Android's navigation back stack to allow users to easily navigate between screens and return to previous screens without losing their place. The app will also include a Floating Action Button (FAB) for quick access to adding new plants, and it will use notifications to alert users about important events such as watering reminders or unfavorable conditions.

---

## Test Plan and Results



---

## User Manual

This section is about getting started with Plantir, including setting up the hardware, installing the mobile app, and using the features of the system.

Alternative setups are possible, such as using a different microcontroller or cloud service, but the following instructions are based on our specific implementation and are the recommended way to get started with Plantir.


### Hardware Setup

The hardware setup uses the following components:
- Arduino Nano ESP32 microcontroller
- Capacitive soil moisture sensor
- DHT22 temperature and humidity sensor
- BH1750 light sensor
- PWM water pump

1. Connect the sensors to the Nano ESP32 microcontroller in the following way:
   - Soil moisture sensor: Connect the VCC pin to 3.3V, GND pin to GND, and the output pin to A1 (analog input pin).
   - Temperature and humidity sensor: Connect the VCC pin to 3.3V, GND pin to GND, and the data pins to A3 and A4 (analog input pins using I2C communication).
   - Light sensor: Connect the VCC pin to 3.3V, GND pin to GND, and the output pin to D2 (digital input pin).
   - Water pump: Connect the VCC pin to 5V, GND pin to GND, and the control pin to a digital output pin (e.g., D4) through a relay module.
2. Plug the microcontroller into a desktop environment and flash the inital provisioning firmware. This can be done many ways; we recommend PlatformIO, WebFlash, or the Arduino IDE.
3. Reboot the microcontroller, and it should now be in provisioning mode, broadcasting a Bluetooth Low Energy (BLE) signal that the mobile app can detect.

### App Setup

1. Download and install the Plantir mobile app from the Google Play Store.
2. Open the app and create an account or log in if you already have one.
3. Press the Floating Action Button (FAB) in the bottom right to add a new plant.
    - Inout the network SSID and credentials the microcontroller will be able to use reliably. This is required for the microcontroller to connect to the internet and communicate with the cloud VPS.
    - Select the plant species from the dropdown menu. This will allow the system to tailor the optimal growing conditions and watering schedule for the specific plant.
    - Toggle the added sensors and pumps that you have connected to the microcontroller. This will allow the app to display the correct data and control the correct actuators.
4. Press the "Provision Plant" button to start the provisioning process. The app will scan for nearby BLE devices and should detect the microcontroller broadcasting its signal.

---

## Spring Presentation
![Spring Presentation](./Documents/Spring%20Final%20Presentation.pdf)
<iframe src="./Documents/Spring Final Presentation.pdf" width="100%" height="600px"></iframe>
// TODO: embed the PDF in the README

## Expo Poster
![Poster](./Documents/2026%20Plantir%20Expo%20Poster%20FINAL.png)

## Assessments
- Final self assessment

## Summary of Hours

// TODO: Give both semester summaries for each team member (hours and amount); give a total for each team member for the year (hours and amount); give a total for the project (hours and amount); a paragraph of justification of the activities for each team member associated with the hours should be given. Provide links to project notebooks or to meeting notes, which provides evidence of hours.


## Summary of Expenses
Hardware and services we'd view as required:
- Nano ESP32: $10.99
- Soil moisture sensor: $5.99
- VPS: $5-10/month

Optional hardware and services:
- Temperature and humidity sensor: $5.99
- Light sensor: $5.99
- 3D printed enclosure: $20-50
- Water pump: $10-20

## Appendix

// TODO: links to repo, notion, and any other relevant project documentation or resources.