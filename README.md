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
### Description
Plantir has unit testing for the mobile app, embedded system, and cloud VPS components. These tests cover, but are not limited to, the following:
- Mobile App: UI tests for navigation and user interactions, unit tests for data processing and state management, and integration tests for API calls to the cloud VPS.
- Embedded System: Unit tests for sensor data collection and processing, integration tests for MQTT communication with the cloud VPS, and end-to-end tests for the watering system.
- Cloud VPS: Unit tests for MQTT message handling and database queries, integration tests for user authentication and data security, and end-to-end tests for the automatic watering system.

As for informal testing, we have been testing the system throughout the development process by setting up a test plant with the microcontroller and sensors, and using the mobile app to monitor the plant's health and control the watering system. This has allowed us to identify and fix issues in real-time, ensuring that the system is robust and reliable. We have also been testing the system with different plant species and under various conditions to ensure that it can handle a wide range of scenarios and provide accurate data and recommendations to the user.

We have also been testing the provisioning process extensively, ensuring that the Bluetooth Low Energy (BLE) connection is stable and that the microcontroller can reliably connect to the cloud VPS and mobile app. This has involved testing with different Android devices and under various environmental conditions to ensure that the provisioning process is seamless for all users.

We also have done general workflow testing: setting up the hardware, provisioning the plant with the app, monitoring the plant's health, and controlling the watering system -- acting as a user. This has allowed us to identify any issues in the overall user experience and make improvements as needed.

### Test Cases

| Identifier | Purpose | Description | Expected Output | Pass/Fail |
| --- | --- | --- | --- | --- |
| TC1 | Mobile App UI Test | Test navigation from Home Screen to Plant Detail Screen | Plant Detail Screen is displayed with correct plant information | Pass |
| TC2 | Mobile App API Test | Test API call to cloud VPS for plant data | Cloud VPS returns correct plant data | Pass |
| TC3 | Embedded System Sensor Test | Test soil moisture sensor data collection | Data is collected and processed correctly | Pass |
| TC4 | Embedded System MQTT Test | Test MQTT communication between microcontroller and cloud VPS | Cloud VPS receives and processes MQTT message correctly | Pass |
| TC5 | Cloud VPS Authentication Test | Test user authentication process | User is authenticated successfully and can access their plant data | Pass |
| TC6 | Delete Plant Test | Test the process of deleting a plant from the app | The plant is removed from the user's account and all associated data is deleted from the cloud VPS | Pass |
| TC7 | Automatic Watering Test | Test the automatic watering system when soil moisture drops below optimal range | | Soil moisture level drops below the user-defined threshold | The system automatically activates the water pump to water the plant, and a notification is sent to the user | Functional | End-to-End | Pass |
| TC8 | Provisioning Test | Test the Bluetooth Low Energy (BLE) provisioning process | | User initiates provisioning process in the mobile app | The microcontroller is detected via BLE, and the user can successfully connect it to the app and cloud VPS | Functional | End-to-End | Pass |
| TC9 | OTA Update Test | Test the Over-the-air (OTA) update process for the microcontroller | | A new firmware update is available for the microcontroller | The user can initiate the OTA update from the mobile app, and the microcontroller successfully updates its firmware without needing physical access | Functional | End-to-End | Pass |
| TC10 | Add Plant Test | Test the process of adding a new plant to the app | The user can successfully input the necessary information, connect the microcontroller via BLE, and the new plant is added to their account with all associated data stored in the cloud VPS | Functional | End-to-End | Pass |
| TC11 | Data Security Test | Test the security of user data in the cloud VPS | Access is denied, and the user's data remains secure | Functional | Unit | Pass |
| TC12 | Performance Test | Test the response time of the mobile app when fetching plant data from the cloud VPS | Plant data is fetched within 3 seconds of home screen load | Pass |
| TC13 | RLS Test | Test Row-Level Security (RLS) in the cloud VPS to ensure users can only access their own plant data | User 1 cannot see, update, add to, or delete User 2's plant data | Pass |

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
[Spring Presentation (limited to people within the University of Cincinnati)](https://mailuc-my.sharepoint.com/:p:/g/personal/foxlc_mail_uc_edu/IQDPgdCk3MSSS5DOcwlFFipPAZViJdn_0Lyr5vD9pFRWi5E?e=3hMGCd)
[Spring Presentation (public pdf)](./Documents/Spring%20Final%20Presentation.png)

## Expo Poster
![Poster](./Documents/2026%20Plantir%20Expo%20Poster.png)

## Assessments
[Spring 2026 Self Assessment](./Documents/Self%20Assessment.pdf)

## Summary of Hours

Both team members worked full-time jobs alongside school during both semesters, so time was limited to weekends and evenings. That work time equaled out to ~ 5-6 hours per week per person, which is a reasonable amount of time to dedicate to a senior design project while balancing other responsibilities. We were able to make significant progress on the project each week, and we consistently met our milestones and deadlines. Fall semester had less hours as we were still in the early stages and mainly planning, ideating, and then setting up. Spring semester had more hours as we were in the thick of development, testing, and iteration. We also had the Expo poster and presentation to prepare for at the end of the semester, which required additional time and effort. Our first visable product was available in January 2026, and we spent the rest of the semester improving and iterating on that product, as well as preparing for the Expo. This VP allowed for more time invested in debugging, testing, and development.

Fall 2025 Hours Summary:
- Lucas Fox: 45 hours
- Mason Trippel: 45 hours

Spring 2026 Hours Summary:
- Lucas Fox: 80 hours
- Mason Trippel: 80 hours

Total Hours Summary:
- Lucas Fox: 125 hours
- Mason Trippel: 125 hours
- **Project Total: 250 hours**

## Summary of Expenses
Hardware and services we'd view as required:
- Nano ESP32: $7
- VPS: $5-10/month
- Miscellaneous hardware (wires, resistors, board, relay module, etc.): $25

Optional hardware and services:
- All 4 sensors (soil moisture, temperature, humidity, light): $6.50
- 3D printed enclosure: $20-50
- Water pump: $6

## Appendix

[Plantir GitHub Repository](https://github.com/lukestonefox/Plantir)