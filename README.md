<div align="center">

# 🍳 Smart Cook AI : RFID Tangible User Interface

[![Python](https://img.shields.io/badge/Python-3.12+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Flask](https://img.shields.io/badge/Flask-2.x-000000?style=for-the-badge&logo=flask&logoColor=white)](https://flask.palletsprojects.com)
[![Google Gemini](https://img.shields.io/badge/Google%20Gemini-AI-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://ai.google.dev)
[![Arduino](https://img.shields.io/badge/Arduino-ESP32-00979D?style=for-the-badge&logo=arduino&logoColor=white)](https://www.arduino.cc)
[![Platform](https://img.shields.io/badge/Platform-Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white)](https://microsoft.com/windows)

**A hands-free, AI-powered cooking assistant that bridges physical IoT hardware with Generative AI : scan an RFID card and let the AI guide you through a full recipe, step by step, with voice instructions and live timers.**

</div>

---

## 📌 About The Project

An interactive Human-Computer Interaction (HCI) project that combines:

- Google Gemini AI (recipe generation)
- Text-to-Speech (gTTS + pygame)
- Automated step timers
- Flask backend API
- Tkinter desktop UI
- Designed for RFID-triggered interaction

This system generates dynamic, step-by-step cooking recipes with automatic timers and voice guidance, creating a hands-free smart cooking experience.
---

## 📋 Project Overview

The Smart RFID Cooking Assistant allows users to:

- **Scan a Recipe Card** → AI generates a new recipe
- **Scan a Next Step Card** → Moves to next cooking step (or scan other recipe cards), recipe cards can be modified according to user interest like fish, mutton recipe card)
- **Automatic timer** per step
- **Voice instruction** playback
- **Live UI** with countdown + progress bar

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        USER                                 │
│                    (Scans RFID Card)                        │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────┐
│         ESP32 RFID Module      │
│  • Reads card UID                        │
│  • Maps UID → Recipe Type                │
│  • Beeps buzzer for feedback             │
│  • Sends HTTP POST over WiFi             │
└──────────────────────────┬───────────────┘
                           │ HTTP POST /recipe
                           │ (Local WiFi Network)
                           ▼
┌──────────────────────────────────────────┐
│         Flask API Backend (PC)           │
│  • Receives recipe type from ESP32       │
│  • Calls Google Gemini 2.5 Flash         │
│  • Parses JSON recipe response           │
│  • Manages step state & timers           │
│  • Triggers TTS voice output             │
└──────────┬────────────────────┬──────────┘
           │                    │
           ▼                    ▼
┌──────────────────┐  ┌─────────────────────┐
│  Tkinter Desktop │  │        TTS          │
│  UI Dashboard    │  │  (Voice Playback)   │
│  (Live Updates)  │  │                     │
└──────────────────┘  └─────────────────────┘
```

---

## 🔄 How It Works

1. **Scan a Recipe Card** → ESP32 reads the RFID card UID and maps it to a recipe type (e.g., `chicken`, `fish`, `vegetarian`)
2. **ESP32 sends HTTP POST** to the Flask server running on the PC over local WiFi
3. **Flask backend calls Google Gemini AI** with the recipe type and receives a structured JSON recipe with step-by-step instructions and durations
4. **Step 1 is spoken aloud** via gTTS + pygame and displayed on the Tkinter UI
5. **Countdown timer starts** automatically for that step's duration
6. **Scan the next card** → Flask advances to the next step, reads it aloud, and starts the next timer
7. **Repeat** until all steps are complete — Gemini generates a unique recipe every time

---

## 🛠️ Technologies Used

**Hardware & Embedded**

| Component | Details |
|-----------|---------|
| ESP32 Microcontroller | Main IoT controller |
| Arduino IDE | For ESP32 programming |
| RFID Module | 
| RFID Cards | Physical tangible input |
| WiFi Communication | ESP32 ↔ PC over local network |
| Buzzer | Scan feedback |
| Jumper Wires | Hardware connections |
| Breadboard | Prototyping |

**Software**

| Technology | Role |
|-----------|------|
| Google Gemini AI (`gemini-2.5-flash`) | Recipe generation |
| Flask | Backend API |
| gTTS + pygame | Text-to-Speech & audio playback |
| Tkinter | Desktop UI |
| Python | Main application language |

---

## 📁 Project Structure

```
Smart_Cook_AI_TUI/
│
├── cmd_main.py                    # Main application : Flask API + Tkinter UI + TTS
│
├── Arduino_Esp32_main/
│   └── Arduino_Esp32_main.ino    # ESP32 firmware : RFID scan + WiFi HTTP client
│
└── RFID_Card_Number/
    └── RFID_Card_Number.ino      # Utility sketch to read RFID card 
```

---

## ⚙️ Setup & Installation

### Hardware Wiring

**MFRC522 → ESP32**

| MFRC522 Pin | ESP32 Pin |
|:-----------:|:---------:|
| SDA (SS)    | GPIO 2    |
| SCK         | GPIO 18   |
| MOSI        | GPIO 23   |
| MISO        | GPIO 19   |
| RST         | GPIO 4    |
| GND         | GND       |
| 3.3V        | 3.3V      |

**Buzzer → ESP32**

| Buzzer Pin   | ESP32 Pin |
|:------------:|:---------:|
| + (Positive) | GPIO 15   |
| – (Negative) | GND       |

**Hardware Setup**

<div align="center">
  <img width="480" src="https://github.com/user-attachments/assets/8342108d-544b-4484-9138-c2683acbc0e9" alt="Hardware Setup Photo 1" />
  <img width="480" src="https://github.com/user-attachments/assets/7f1adb78-f7bb-49b4-bf8f-d3ed8cfb45e6" alt="Hardware Setup Photo 2" />
</div>

---


### Step 1 — Connect Hardware & Get RFID Card UIDs

After connecting the wires, connect the ESP32 to your PC via USB cable. First, open `RFID_Card_Number.ino` in Arduino IDE and scan the cards to get their ID numbers and note them down.

> ⚠️ Ensure that the necessary ESP32 board package and RFID library are installed in Arduino IDE.

---

### Step 2 — Flash the Main ESP32 Firmware

Open `Arduino_Esp32_main.ino` in Arduino IDE and flash it to the ESP32.

<div align="center">
  <img width="900" src="https://github.com/user-attachments/assets/b336c254-0c70-4120-9173-d8ec38d9db8c" alt="Arduino IDE — Opening ESP32 main firmware" />
</div>

In the code, input your **WiFi name (SSID)** and **WiFi password**. Make sure your PC and ESP32 are connected to the same network.

Now open Command Prompt and type `ipconfig` to get your PC's local IP address:

<div align="center">
  <img width="900" src="https://github.com/user-attachments/assets/3efc76bd-b29c-433f-8050-5d34a1df0b4a" alt="ipconfig — Finding local IP address" />
</div>

Copy the IP address shown above and paste it into the `serverURL` option in `Arduino_Esp32_main.ino`.

---

### Step 3 — Configure and Run the Python Application

Make sure Python and its necessary libraries are installed. Open `cmd_main.py`:

<div align="center">
  <img width="900" src="https://github.com/user-attachments/assets/8c92262c-8b66-40ef-95ea-334b480ec9ce" alt="cmd_main.py — API key configuration" />
</div>

In the code, replace `api_key` with your own **Google Gemini API key** string and save the file.

Now navigate to the location of `cmd_main.py`, open Command Prompt there and run:

```bash
python cmd_main.py
```

---

### ✅ All done! The project is ready.

Scan the desired RFID card to see the output.

---

## 🖥️ Demo — Application Output

**Recipe generation and step-by-step UI in action:**

<div align="center">
  <img width="900" src="https://github.com/user-attachments/assets/de759e31-de9e-4043-88ad-58d37abba4f3" alt="UI Output — Recipe started" />
</div>

<div align="center">
  <img width="900" src="https://github.com/user-attachments/assets/90068831-7fe2-4f7b-af14-67e0fe554c13" alt="UI Output — Step progress view" />
</div>

<div align="center">
  <img width="900" src="https://github.com/user-attachments/assets/d5924e32-789d-4028-b940-3c83f8c98a06" alt="UI Output — Step with timer" />
</div>

<div align="center">
  <img width="900" src="https://github.com/user-attachments/assets/62af066b-3fc5-4f25-b23e-1c7fdc6e013e" alt="UI Output — Timer countdown active" />
</div>
