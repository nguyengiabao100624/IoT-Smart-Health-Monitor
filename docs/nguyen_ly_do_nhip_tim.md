# Nguyên lý đo nhịp tim (Heart Rate Measurement Principle)

Tài liệu này giải thích chi tiết nguyên lý vật lý và thuật toán xử lý tín hiệu được sử dụng để đo nhịp tim trong dự án.

## 1. Cơ sở vật lý (PPG - Photoplethysmography)
Dự án sử dụng kỹ thuật **Quang thể tích đồ (PPG)** thông qua cảm biến **MAX30102**:
*   **Chiếu sáng:** Đèn LED hồng ngoại (IR) chiếu vào các mô và mạch máu ở đầu ngón tay.
*   **Hấp thụ:** Hemoglobin trong máu hấp thụ ánh sáng IR. Khi tim đập, lượng máu tăng lên làm lượng ánh sáng hấp thụ thay đổi.
*   **Cảm biến:** Cảm biến quang học đo lượng ánh sáng phản xạ ngược lại, từ đó tái tạo thành dạng sóng nhịp tim.

## 2. Thông số cấu hình phần cứng
Các thông số này được thiết lập để tối ưu hóa tín hiệu đầu vào:
*   **Sample Rate:** 200 Hz (Lấy mẫu 200 lần/giây).
*   **Sample Average:** 8 (Gộp 8 mẫu thành 1 để khử nhiễu).
    *   *Tốc độ dữ liệu thực tế:* 25 Hz.
*   **Pulse Width:** 411 µs.
*   **ADC Range:** 4096 (12-bit).
*   **LED Current:** Cấu hình mức 60 (tương đương khoảng 12mA).

## 3. Thuật toán xử lý và lọc nhiễu
Để ra được con số ổn định, tín hiệu trải qua 4 bước xử lý phần mềm:

### A. Kiểm tra tín hiệu thật (Pulsatile Check)
*   **Ngưỡng IR:** Phải > 50,000 (Xác nhận có ngón tay).
*   **Độ biến thiên (Peak-to-Peak):** Hiệu số cực đại và cực tiểu IR phải > 300 (Xác nhận mạch đang đập, không phải vật thể tĩnh).

### B. Xử lý sóng đôi (Dicrotic Notch)
Do mạch máu có hiện tượng phản hồi sóng phụ (sóng đôi) dễ gây đếm nhầm làm nhịp tim bị nhân đôi:
*   **Logic:** Nếu `Nhịp hiện tại > Nhịp trung bình * 1.3` hoặc `Nhịp hiện tại > Nhịp trung bình + 18`, thuật toán sẽ tự động **chia 2** giá trị đó.

### C. Bộ lọc làm mượt (Filtering)
*   **EMA Filter (Exponential Moving Average):** 
    *   Công thức: `smoothBPM = (smoothBPM * 0.85) + (currentBPM * 0.15)`.
    *   Giúp số không bị nhảy đột ngột.
*   **Median Filter (Lọc trung vị):** 
    *   Sử dụng cửa sổ trượt 13 mẫu dữ liệu gần nhất.
    *   Lấy giá trị ở giữa để loại bỏ hoàn toàn các điểm nhiễu cao/thấp bất thường.

### D. Giới hạn sinh lý
*   **Dải đo:** Chỉ chấp nhận kết quả từ **45 BPM đến 130 BPM**. Mọi kết quả nằm ngoài dải này đều bị coi là nhiễu và loại bỏ.

## 4. Quy trình xác thực (Validation)
1.  **Skip Samples:** Bỏ qua 4 mẫu đầu tiên khi bắt đầu đo.
2.  **Stability:** Cần thu thập đủ 7 mẫu hợp lệ liên tiếp.
3.  **Completion:** Khi đạt đủ số mẫu, hệ thống sẽ chốt dữ liệu và hiển thị 100% tiến độ.
