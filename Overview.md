# Rakuremo

## Overview

This project aims to provide web service for user to control & monitor
system with ESP32 & camera (RTSP).

## Infrastructure

Key components:

1. **Backend**: Node.js and Express, use MySQL.
2. **Camera Service**:  Golang, convert RTSP to stable HTTP protocol.
3. **Frontend Admin**: Next.js-based admin interface.
4. **Frontend User**: A Next.js-based user interface.

![Infrastructure Diagram](./iot-system.jpg)

## Install & Develop

### Prerequisites

1. **Node.js**: v22
2. **Golang**: v1.16
3. **Docker** (Optional)
4. **Running Mosquitto MQTT Broker (see below)**
5. **Ready-to-connect MySQL Database (config in .env file)**

### Backend

1. Clone the repository:
   ```bash
   git clone https://github.com/rakuremo/backend
   cd backend
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Create a `.env` file and configure the environment variables as in the .env.example file:
   ```env
   PORT=3000
   ...
   ```

4. Start the server:
   ```bash
   npm start
   ```

### Camera Service

1. Clone the repository:
   ```bash
   git clone https://github.com/rakuremo/camera-service
   cd camera-service
   ```

2. Install dependencies and run (requires Golang):
   ```bash
   GO111MODULE=on go run *.go
   ```

3. Open the browser and navigate to:
   ```bash
   http://127.0.0.1:8083
   ```

### Frontend Admin

1. Clone the repository:
   ```bash
   git clone https://github.com/rakuremo/frontend-admin
   cd frontend-admin
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Start the development server:
   ```bash
   npm run dev
   ```

### Frontend User

1. Clone the repository:
   ```bash
   git clone https://github.com/rakuremo/frontend
   cd frontend
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Start the development server:
   ```bash
   npm run dev
   ```

### MQTT Broker

1. Install Mosquitto MQTT Broker:
   ```bash
   sudo apt-get install mosquitto mosquitto-clients
   ```

2. Start the Mosquitto service:
   ```bash
    sudo systemctl start mosquitto
    ```

3. Check the status:
    ```bash
    sudo systemctl status mosquitto
    ```

    Should see the following output:
      ```bash
      ● mosquitto.service - Mosquitto MQTT Broker
          Loaded: loaded (/lib/systemd/system/mosquitto.service; enabled; vendor preset: enabled)
          Active: active (running) since Sun 2024-12-16 14:00:00 UTC; 1min 30s ago
      ```

## Limitations

1. **Video Codecs**: Currently, only H264 video codec is supported.
2. **Audio Codecs**: No audio codec support is available.
3. **Performance**: CPU usage is approximately 0.2%-1% per stream on an Intel Core i7 processor.
4. **Docker Support**: Installation from Docker is currently under maintenance.
5. **HLS Stability**: HLS is not stable and may not work as expected.
6. **MSE Support**: MSE is not supported on iOS devices.
7. **WebRTC**: WebRTC is under development and not yet available.

## API Documentation

For detailed API documentation, refer to the [API docs](./docs/api.md).

## Configuration

### Backend

Configure the backend by setting environment variables in the `.env` file.

### Camera Service

Configure the camera service by editing the `config.json` file. Example configuration:

```json
{
  "server": {
    "debug": true,
    "log_level": "info",
    "http_demo": true,
    "http_debug": false,
    "http_login": "demo",
    "http_password": "demo",
    "http_port": ":8083",
    "ice_servers": ["stun:stun.l.google.com:19302"],
    "rtsp_port": ":5541"
  },
  "streams": {
    "demo1": {
      "name": "test video stream 1",
      "channels": {
        "0": {
          "name": "ch1",
          "url": "rtsp://admin:admin@YOU_CAMERA_IP/uri",
          "on_demand": true,
          "debug": false,
          "audio": true,
          "status": 0
        }
      }
    }
  }
}
```

### Frontend

Configure the frontend by setting environment variables in the `.env` file. (direct the API to the backend server that is currently running)

## Command-line

### Camera Service

Use the following command to see available arguments:
```bash
./RTSPtoWeb --help
```

Response:
```bash
Usage of ./RTSPtoWeb:
  -config string
        config patch (/etc/server/config.json or config.json) (default "config.json")
  -debug
        set debug mode (default true)
```
