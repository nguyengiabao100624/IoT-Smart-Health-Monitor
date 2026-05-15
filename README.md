<div align="center">

# 🩺 IoT Smart Health Monitor & Fall Detection System
**A Wearable Edge AI Device for Elderly Care & Real-Time Health Tracking**

[![Build Status](https://github.com/nguyengiabao100624/IoT-Smart-Health-Monitor/actions/workflows/pio-build.yml/badge.svg)](https://github.com/nguyengiabao100624/IoT-Smart-Health-Monitor/actions)
[![PlatformIO](https://img.shields.io/badge/PlatformIO-orange?style=for-the-badge&logo=PlatformIO&logoColor=white)](https://platformio.org/)
[![C++](https://img.shields.io/badge/C++-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white)](https://isocpp.org/)
[![ESP32](https://img.shields.io/badge/ESP32-E7352C?style=for-the-badge&logo=espressif&logoColor=white)](https://www.espressif.com/)
[![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)](https://firebase.google.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

[Vietnamese (Tiếng Việt) 👇](#-phiên-bản-tiếng-việt) | [English Version 👆](#-english-version)

</div>

---

## 🌍 English Version

### 📖 Overview
The **IoT Smart Health Monitor** is an advanced, wearable Edge AI device designed to protect and monitor the elderly. Built on the **ESP32** microcontroller, it continuously tracks vital signs, environmental factors, and utilizes machine learning (Edge Impulse) to detect falls with high accuracy. The data is synchronized in real-time via **Firebase** to an Android companion app, allowing caregivers to monitor patients from anywhere.

### ✨ Key Features
- ❤️ **Vital Signs Tracking:** Continuous monitoring of Heart Rate (BPM), Blood Oxygen (SpO2), and Body Temperature using MAX30102 & MLX90614 sensors.
- 🌫️ **Environmental Safety:** Measures ambient PM2.5 dust concentration to warn against poor air quality.
- 🚨 **AI Fall Detection (Edge AI):** Utilizes an MPU6050 accelerometer processed by a local TinyML model to detect falls instantly and accurately, minimizing false alarms.
- 🆘 **Emergency Response:** Physical SOS button, coupled with GPS positioning to locate the wearer instantly during an emergency.
- 🤖 **Gemini AI Integration:** Generates localized health advice and anomaly analysis via an AI virtual assistant in the Android app.
- 📲 **Real-time Synchronization:** Powered by Firebase Realtime Database for <100ms latency updates to the mobile application.

### 🏗️ System Architecture
The system employs a dual-core RTOS approach (FreeRTOS) to guarantee non-blocking sensor acquisition and reliable network transmission.
- **Core 0:** Dedicated to Firebase API calls and OTA updates.
- **Core 1:** Handles I2C/Analog sensor reads, Edge Impulse inference, and ST7789 TFT display updates.

*For an in-depth architectural breakdown, please see [`docs/DAKT1_ARCHITECTURE_ROOT.md`](docs/DAKT1_ARCHITECTURE_ROOT.md).*

### 🛠️ Hardware Setup (Pinout)
| Component | ESP32 Pin | Interface |
|-----------|-----------|-----------|
| **ST7789 TFT (1.3")** | SCL(16), SDA(17), RST(5), DC(18), CS(19) | SPI |
| **MAX30102 (HR/SpO2)**| SDA(21), SCL(22) | I2C (0x57) |
| **MLX90614 (Temp)**   | SDA(21), SCL(22) | I2C (0x5A) |
| **MPU6050 (IMU)**     | SDA(21), SCL(22) | I2C (0x68) |
| **Dust Sensor**       | Analog(33), LED(14) | Analog/Digital |
| **GPS Module**        | TX2(35), RX2(32) | UART2 |

### 🚀 Quick Start

<details>
<summary><b>Click to expand installation instructions</b></summary>

1. **Clone the repository:**
   ```bash
   git clone https://github.com/nguyengiabao100624/IoT-Smart-Health-Monitor.git
   ```
2. **Open with PlatformIO:** Open the project directory in VSCode with the PlatformIO extension installed.
3. **Configure Credentials:** Edit or create `include/secrets.h` to add your Firebase config and Wi-Fi credentials (if hardcoded, though WiFiManager is supported).
4. **Build & Flash:** Connect your ESP32 and click **Upload** in PlatformIO.

</details>

---

<br>

<div align="center">

## 🇻🇳 Phiên bản Tiếng Việt

</div>

### 📖 Tổng quan
**IoT Smart Health Monitor** là một thiết bị đeo tích hợp AI tiên tiến (Edge AI) được thiết kế đặc biệt để bảo vệ và theo dõi sức khỏe người cao tuổi. Dựa trên vi điều khiển **ESP32**, thiết bị liên tục đo các chỉ số sinh tồn, theo dõi môi trường xung quanh, và sử dụng Machine Learning để phát hiện sự cố té ngã với độ chính xác cao. Dữ liệu được đồng bộ hóa theo thời gian thực qua **Firebase** tới ứng dụng Android, giúp người nhà giám sát bệnh nhân từ xa 24/7.

### ✨ Tính năng nổi bật
- ❤️ **Giám sát Sinh tồn:** Đo Nhịp tim (BPM), Nồng độ Oxy trong máu (SpO2) và Thân nhiệt liên tục thông qua cảm biến MAX30102 & MLX90614.
- 🌫️ **Đo lường Môi trường:** Giám sát chất lượng không khí qua chỉ số bụi mịn PM2.5.
- 🚨 **Phát hiện Té ngã bằng AI:** Ứng dụng TinyML (Edge Impulse) xử lý dữ liệu gia tốc MPU6050 ngay trên biên (Edge) để xác định chính xác hành vi ngã, loại trừ các sai số do vận động mạnh.
- 🆘 **Cứu hộ Khẩn cấp:** Nút bấm SOS kết hợp định vị GPS giúp báo động ngay lập tức vị trí người bị nạn.
- 🤖 **Trợ lý Ảo Gemini AI:** Tích hợp AI phân tích dữ liệu y tế và đưa ra lời khuyên cá nhân hóa trực tiếp trên App Android.
- 📲 **Đồng bộ Thời gian thực:** Sử dụng Firebase Realtime Database với độ trễ siêu thấp (<100ms) để cập nhật thông tin lên điện thoại.

### 🏗️ Kiến trúc Hệ thống
Hệ thống sử dụng hệ điều hành thời gian thực (FreeRTOS) tận dụng kiến trúc lõi kép của ESP32:
- **Core 0:** Chuyên xử lý kết nối mạng (Firebase) và cập nhật không dây (OTA) để đảm bảo không bị nghẽn mạng.
- **Core 1:** Xử lý đọc cảm biến I2C/Analog, chạy mô hình AI nội bộ và render đồ họa lên màn hình TFT ST7789.

*Xem tài liệu kiến trúc chuyên sâu tại [`docs/DAKT1_ARCHITECTURE_ROOT.md`](docs/DAKT1_ARCHITECTURE_ROOT.md).*

### 🛠️ Hướng dẫn Cài đặt Nhanh

<details>
<summary><b>Nhấn để xem hướng dẫn cài đặt chi tiết</b></summary>

1. **Tải mã nguồn:**
   ```bash
   git clone https://github.com/nguyengiabao100624/IoT-Smart-Health-Monitor.git
   ```
2. **Mở với PlatformIO:** Mở thư mục dự án bằng VSCode có cài sẵn tiện ích PlatformIO.
3. **Cấu hình:** Cập nhật file `include/secrets.h` với thông tin Firebase của bạn.
4. **Nạp Code:** Kết nối mạch ESP32 và nhấn **Upload**.

</details>

---

### 📚 Tài liệu tham khảo bổ sung (Documentation)
Các phân tích thuật toán và nguyên lý phần cứng được lưu trữ trong thư mục `/docs`:
- [Nguyên lý Phát hiện Té ngã](docs/fall_detection_principles.md)
- [Nguyên lý Đo Nhịp tim / SpO2](docs/nguyen_ly_do_nhip_tim.md)
- [Phân tích Năng lượng & Tiêu thụ Pin](docs/power_analysis.md)
- [Tài liệu Thiết kế Dự án Tổng thể](PROJECT_FULL_DOCUMENTATION.md)

### 📄 Giấy phép (License)
Dự án được phân phối dưới giấy phép [MIT License](LICENSE). Vui lòng xem file `LICENSE` để biết thêm chi tiết.
