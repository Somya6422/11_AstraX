# UNSPOKEN: Chest-Mounted Communication Device

A wearable assistive device for **visually impaired** and **non-verbal** individuals.

## Features

### UNSEEN Mode (For the Visually Impaired)
- **Stereo Obstacle Detection:** Two ultrasonic sensors detect objects on left and right sides.
- **Haptic Feedback:** Vibration motors alert the user to nearby obstacles (closer = stronger vibration).
- **AI Scene Description:** Camera captures images and describes the environment using a local AI model.

### UNSPOKEN Mode (For Non-Verbal Communication)
- **Body Language Recognition:** IMU sensor detects lean gestures.
    - Lean Forward → "Yes"
    - Lean Back → "No"
    - Tap Chest → "Help"
    - Cover Chest → "SOS"
- **Offline Speech Output:** Gestures are converted to speech without internet.
- **Personalized Calibration:** Adapts to user's individual range of motion.

## Hardware

- Raspberry Pi Zero 2W
- Pi Camera Module V2
- MPU6050 IMU (Gyroscope/Accelerometer)
- 2x HC-SR04 Ultrasonic Sensors
- 2x Vibration Motors (with 2N2222 transistor drivers)

## Software

- **Raspberry Pi:**
    - `main.py` - Sensor fusion loop (offline obstacle detection, gesture recognition)
    - `camera_stream.py` - Camera capture and streaming to server
- **Server (Laptop):**
    - Node.js WebSocket server
    - Custom local vision engine for offline AI
- **Web Dashboard:**
    - React-based monitoring interface
    - Real-time sensor visualization

## Quick Start

### 1. Server Setup (Laptop)
```bash
cd server
npm install
npm run production
```

### 2. Web Dashboard
Open `http://localhost:8080` in your browser.

### 3. Raspberry Pi
```bash
cd raspberry_pi
pip3 install mpu6050-raspberrypi websockets --break-system-packages
python3 main.py
```

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     CHEST MOUNT DEVICE                      │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐  │
│  │  Ultrasonic │  │  Ultrasonic │  │      Camera         │  │
│  │   (Left)    │  │   (Right)   │  │    (Pi Camera)      │  │
│  └──────┬──────┘  └──────┬──────┘  └──────────┬──────────┘  │
│         │                │                     │             │
│         └────────┬───────┘                     │             │
│                  ▼                             ▼             │
│  ┌───────────────────────────────────────────────────────┐  │
│  │              RASPBERRY PI ZERO 2W                     │  │
│  │  ┌──────────────────┐  ┌──────────────────────────┐   │  │
│  │  │  MPU6050 (IMU)   │  │  Vibration Motors (L/R)  │   │  │
│  │  └──────────────────┘  └──────────────────────────┘   │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
                              │
                              │ WiFi (Local Network)
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                      LAPTOP (BRAIN)                         │
│  ┌──────────────┐  ┌──────────────┐  ┌─────────────────┐    │
│  │ Node Server  │  │ Local Vision │  │  Web Dashboard  │    │
│  │  (Port 8080) │  │   Engine     │  │    (React)      │    │
│  └──────────────┘  └──────────────┘  └─────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

## License

MIT License
