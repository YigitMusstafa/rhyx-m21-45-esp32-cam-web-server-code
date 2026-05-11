# ESP32-CAM with RHYX M21-45 Camera Module

> **Why this repo exists:** Most ESP32-CAM boards ship with an OV2640 sensor — but sometimes you get a **RHYX M21-45** instead. Finding working code for this module is a pain, so here it is.

![RHYX M21-45 on ESP32-CAM](images/m21_module.jpg)

---

## 📷 What is the M21-45?

The **RHYX M21-45** is a camera module that occasionally ships with ESP32-CAM boards as an alternative to the OV2640. It looks nearly identical and fits the same FPC connector, but the initialization can behave differently depending on the firmware.

The good news: **it works with the standard `esp_camera` driver** — you just need the right pin config and settings.

---

## ✅ Working Pin Configuration (AI Thinker ESP32-CAM)

```cpp
#define PWDN_GPIO_NUM     32
#define RESET_GPIO_NUM    -1
#define XCLK_GPIO_NUM      0
#define SIOD_GPIO_NUM     26
#define SIOC_GPIO_NUM     27
#define Y9_GPIO_NUM       35
#define Y8_GPIO_NUM       34
#define Y7_GPIO_NUM       39
#define Y6_GPIO_NUM       36
#define Y5_GPIO_NUM       21
#define Y4_GPIO_NUM       19
#define Y3_GPIO_NUM       18
#define Y2_GPIO_NUM        5
#define VSYNC_GPIO_NUM    25
#define HREF_GPIO_NUM     23
#define PCLK_GPIO_NUM     22
```

---

## 🔧 Camera Init Settings

```cpp
config.xclk_freq_hz = 20000000;
config.pixel_format = PIXFORMAT_JPEG;
config.frame_size   = FRAMESIZE_VGA;  // 640x480
config.jpeg_quality = 12;             // 0-63, lower = better quality
config.fb_count     = 2;
config.grab_mode    = CAMERA_GRAB_LATEST;
```

> If you get `camera init failed` errors, try lowering `xclk_freq_hz` to `16000000` or changing `frame_size` to `FRAMESIZE_QQVGA`.

---

## 📡 Live Stream Example

The included sketch starts a Wi-Fi HTTP server and streams MJPEG video:

- Open `http://<ESP32-IP>/` in your browser → live video
- `/stream` endpoint → raw MJPEG stream (usable with OpenCV)

Set your Wi-Fi credentials in the sketch:

```cpp
const char* ssid     = "YOUR_WIFI";
const char* password = "YOUR_PASSWORD";
```

Flash the board, open Serial Monitor at 115200 baud, and you'll see the IP address printed after connecting.

---

## 🛠️ Requirements

- Arduino IDE with ESP32 board package installed
- Board: **AI Thinker ESP32-CAM**
- Library: `esp_camera` (included with ESP32 Arduino core)

---

## ❓ How to tell if you have an M21

If your ESP32-CAM came with a camera module that has **RHYX M21-45** printed on the back of the sensor PCB (as shown in the photo above), this repo is for you. The OV2640 has no such marking and usually has a slightly different lens housing.

---

## 🤝 Contributing

If you've tested this with other frame sizes, formats, or board variants — PRs and issues are welcome!
