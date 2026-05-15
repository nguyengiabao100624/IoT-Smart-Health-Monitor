# Nguyên lý đo nồng độ bụi mịn (Dust Density Principle)

Tài liệu này giải thích cách hệ thống tính toán chỉ số bụi mịn PM2.5 sử dụng cảm biến Sharp GP2Y1010AU0F.

## 1. Nguyên lý hoạt động
Cảm biến bụi Sharp hoạt động dựa trên phương pháp **tán xạ ánh sáng**:
*   Một đèn LED hồng ngoại và một cảm biến quang (phototransistor) được đặt chéo góc bên trong buồng tối.
*   Khi không khí có bụi đi qua, ánh sáng từ LED bị tán xạ bởi các hạt bụi và đi tới cảm biến quang.
*   Cường độ ánh sáng tán xạ tỉ lệ thuận với nồng độ bụi trong không khí.

## 2. Quy trình tính toán trong mã nguồn

### Bước 1: Đọc tín hiệu Analog và chuyển sang Điện áp (Volt)
Hệ thống lấy mẫu từ chân cảm biến và chuyển đổi giá trị số (ADC) sang giá trị điện áp thực tế:
*   **Giá trị ADC:** 0 - 4095 (12-bit trên ESP32).
*   **Điện áp tham chiếu:** 4.95V.
*   **Công thức:** `Voltage = ADC_Value * (4.95 / 4095.0)`

### Bước 2: Tính toán nồng độ bụi (µg/m³)
Sử dụng công thức dựa trên đặc tính tuyến tính của cảm biến:
*   **Công thức:** `RawDust = (0.17 * Voltage - 0.1) * 1000`
*   **Giải thích:**
    *   `0.17`: Hệ số góc (Slope) của cảm biến (chuyển Volt sang mg/m³).
    *   `-0.1`: Trị số bù (Offset) để hiệu chuẩn về 0 khi không có bụi.
    *   `1000`: Nhân thêm 1000 để chuyển từ mg/m³ sang µg/m³ (đơn vị thông dụng cho bụi mịn).

### Bước 3: Lọc mượt dữ liệu (EMA Filter)
Để kết quả không bị nhảy số liên tục do nhiễu môi trường, hệ thống áp dụng bộ lọc trung bình động:
*   **Công thức:** `CurrentDust = (0.1 * RawDust) + (0.9 * CurrentDust)`
*   **Ý nghĩa:** Mỗi lần cập nhật mới chỉ lấy 10% giá trị vừa đọc được cộng với 90% giá trị cũ đã lưu, giúp con số hiển thị ổn định và mượt mà.

## 3. Thông số kỹ thuật tóm tắt
*   **Thời gian lấy mẫu:** Đèn LED bật trong 280µs trước khi đọc mẫu.
*   **Thời gian chu kỳ:** Khoảng 10ms cho mỗi lần lấy mẫu.
*   **Đơn vị hiển thị:** µg/m³.
