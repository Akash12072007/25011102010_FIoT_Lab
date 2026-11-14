# Foundations of Internet of Things (25CS2741) – Lab Repository

**Name:** Akash  
**Registration Number:** 25011102010  
**Course:** 25CS2741 – Foundations of Internet of Things + Lab  

---

## Repository Overview

This repository contains embedded firmware, circuit diagrams, Node-RED flows, and documentation for Continuous Assessments (**CAT2 – CAT15**) implemented across **8051 (AT89S52)** and **STM32 (STM32F103C8T6 Blue Pill & STM32 Nucleo-F401RE)** microcontroller platforms.

---

## Folder Structure

```text
25011102010_FIoT_Lab/
├── README.md
├── 8051/
│   ├── CAT2/   # Bare-metal GPIO LED blinking
│   ├── CAT3/   # Timer-based LED patterns & interrupt control
│   ├── CAT4/   # 4x4 Matrix keypad & 7-segment display interfacing
│   ├── CAT5/   # Timer 0 & Timer 1 hardware interrupts
│   ├── CAT6/   # UART serial transmission & echo
│   └── CAT7/   # 16x2 LCD 4-bit interfacing, DDRAM cursor & keypad navigation
└── STM32/
    ├── CAT8/   # STM32F103 GPIO push-button control & software debouncing
    ├── CAT9/   # Short-press vs. long-press duration classification
    ├── CAT10/  # UART command-controlled LED parser & ESP8266 AT setup
    ├── CAT11/  # Embedded HTTP server on STM32 + ESP8266 & Node-RED client
    ├── CAT12/  # Nucleo-F401RE GPIO bring-up, state latch & press classification
    ├── CAT13/  # BME280 I2C sensor interfacing + HTTP telemetry + Node-RED analytics
    ├── CAT14/  # BME280 + MQTT telemetry publishing via custom ESP8266 API
    └── CAT15/  # Real-time BME280 + MQTT + OpenWeather API comparison dashboard
