# V2V-communication-prototype-over-ESP-NOW
Simple V2V communication prototype using an ESP-12E as the sender and an ESP32 as the receiver cum vehicle dashboard controller. 

Concepts covered in this prototype are:
1. Wireless communication over the ESP-NOW network
2. Task scheduling using FreeRTOS

This project assumes a car-bus-car scenario: a car is present in front of a heavy vehicle, and the heavy vehicle is fitted with a proximity sensor that detects sudden braking of the car. This sensor then communicates its findings with the car behind it, making it a V2V prototype. In this project, an ESP 12-E microcontroller is used as the sender, and an ESP32 as the receiver. The "ESP8266_MacAddressFinder.cpp" code programs the ESP8266 to find the MAC address of the ESP32 receiver to ensure message passing. Subsequently, the "SenderCode_ESP8266" code programs it to send messages based on the readings of an ultrasonic sensor, and the "ESP32_Receiver_SuperLoopConstruction.cpp" code programs the ESP32 receiver to receive these messages and act on certain actuators (here, a buzzer+LED+OLED Display setup) that constitute the receiver dashboard. All communication is done via ESP-NOW.
Additionally, a FreeRTOS code is present for the receiver, that does the same tasks using FreeRTOS that is natively present on ESP32, vis-a-vis the super-loop construct that the ordinary receiver code follows. FreeRTOS is not a prerequisite here, but it can be used for learning purposes. 

Please note that the FreeRTOS code is still a work in progress. You may use it for reference, but don't upload it to your hardware yet! Check this space for updates on it.

All code was written within the PlatformIO extension on Microsoft VS Code. If you are using Arduino IDE, please remove the "#include <Arduino.h>" line from the code. 
