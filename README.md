# 🖱️ COMET AIR MOUSE (ESP32-C3 BLE HID)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Hardware](https://img.shields.io/badge/Hardware-ESP32--C3-red.svg)](hardware/pinout.md)
[![Protocol](https://img.shields.io/badge/Protocol-BLE%20HID-blue.svg)](docs/setup_guide.md)
[![Developer](https://img.shields.io/badge/Developer-Kesicode-brightgreen.svg)](https://github.com/Kesicode)

Motion-controlled wireless Bluetooth Low Energy (BLE) HID Air Mouse powered by the **ESP32-C3** microcontroller and **MPU6050** 6-axis Gyroscope/Accelerometer.

---

## ✨ Features

- **Native Bluetooth LE HID Mouse**: Acts as a native Bluetooth mouse; no additional software/drivers needed on host PC or mobile device (Windows, macOS, Linux, Android, iOS).
- **Gyroscope Motion Tracking**: Smooth 2D cursor control with exponential smoothing, deadzone filtering, and diagonal acceleration boost.
- **Dual-Function Button Controls**:
  - **Left Button**: Short press (< 180ms) = Left Click | Long press (≥ 180ms) = Scroll Down
  - **Right Button**: Short press (< 180ms) = Right Click | Long press (≥ 180ms) = Scroll Up
- **Dual Automatic Gyro Calibration**: Auto-calibrates sensor offsets during boot for drift-free precision tracking.
- **Smart BLE Power & Movement Guard**: Motion tracking suspends automatically when Bluetooth is disconnected to conserve battery.

---

## 📁 Repository Layout

```
AIR-MOUSE/
├── .gitignore                          # Git ignore rules for build binaries and IDE outputs
├── README.md                           # Main repository documentation
├── LICENSE                             # MIT Open Source License
├── src/                                # Source code directory
│   └── ESP32-C3-AIR-MOUSE/
│       └── ESP32-C3-AIR-MOUSE.ino      # ESP32-C3 Air Mouse Arduino sketch
├── hardware/                           # Hardware pinout & schematic specifications
│   └── pinout.md
└── docs/                               # Setup & developer documentation
    └── setup_guide.md
```

---

## 🔌 Hardware Pinout

| Component | Pin / Function | ESP32-C3 GPIO | Note |
|---|---|---|---|
| **MPU6050** | SDA | **GPIO 5** | I2C Data Line |
| **MPU6050** | SCL | **GPIO 4** | I2C Clock Line |
| **MPU6050** | VCC / GND | 3.3V / GND | Power Supply |
| **Left Button** | Click / Scroll Down | **GPIO 3** | Internal Pull-Up (`INPUT_PULLUP`) |
| **Right Button** | Click / Scroll Up | **GPIO 2** | Internal Pull-Up (`INPUT_PULLUP`) |

For full hardware details, view [`hardware/pinout.md`](hardware/pinout.md).

---

## 🚀 Getting Started

### ⚡ Option 1: Quick Web Flashing (Recommended - No IDE Required)

You do **not** need to install Arduino IDE, board cores, or libraries to flash the Air Mouse. You can flash the pre-compiled firmware directly from your browser:

1. **Download `.bin` File**: Get the latest release `.bin` file from GitHub Releases.
2. **Open Web ESPTool**: Open an esptool web flasher in a Web Serial supported browser (Chrome, Edge, Opera):
   - [Espressif Web ESPTool](https://espressif.github.io/esptool-js/) or [Adafruit Web Serial ESPTool](https://adafruit.github.io/Adafruit_WebSerial_ESPTool/)
3. **Connect Board**: Connect your ESP32-C3 via USB, set baud rate to `115200` or `921600`, click **Connect**, and select your device's COM/Serial port.
4. **Flash Firmware**:
   - For pre-compiled binary (`.bin`): Add file at offset `0x0000`.
   - Click **Program / Flash**.
5. **Pair & Use**: Reset the board, place it flat on a desk during boot for auto gyro calibration, then pair **`COMET AIRMOUSE`** under host Bluetooth settings!

---

### 💻 Option 2: Compiling & Flashing from Source (For Developers)

If you want to customize or compile the source code manually:

1. **Required Arduino Libraries**:
   - **Wire** (Built-in ESP32 core)
   - **MPU6050_tockn** (Install via Arduino Library Manager)
   - **HijelHID_BLEMouse** (BLE HID library for ESP32)
2. **Clone & Open**:
   ```bash
   git clone https://github.com/Kesicode/AIR-MOUSE.git
   cd AIR-MOUSE
   ```
   Open `src/ESP32-C3-AIR-MOUSE/ESP32-C3-AIR-MOUSE.ino` in **Arduino IDE**.
3. **Select Board**: Select **ESP32C3 Dev Module** under `Tools > Board > esp32`.
4. **Compile & Upload**: Upload to your ESP32-C3 board via Arduino IDE.

For step-by-step flashing instructions, view [`docs/setup_guide.md`](docs/setup_guide.md).

---

## ⚙️ Configuration & Tuning

Key parameters inside [`ESP32-C3-AIR-MOUSE.ino`](src/ESP32-C3-AIR-MOUSE/ESP32-C3-AIR-MOUSE.ino):

```cpp
// Cursor Motion Tuning
float smoothing   = 0.82; // Exponential gyro filter factor (0.0 = raw, 1.0 = heavy smoothing)
float sensitivity = 0.20; // Cursor movement speed multiplier
int   deadzone    = 1;    // Minimum movement threshold to eliminate sensor jitter

// Button & Scroll Timing
const unsigned long clickThreshold = 180; // Hold duration (ms) to trigger scrolling instead of click
const unsigned long scrollDelay    = 120; // Scroll pulse repeat delay (ms)
```

---

## 👤 Author

Developed and maintained by **Kesicode** ([@Kesicode](https://github.com/Kesicode)).

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
