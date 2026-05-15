# Nguyên lý phát hiện té ngã (Fall Detection Principle)

Hệ thống sử dụng phương pháp bảo vệ đa tầng, kết hợp giữa các ngưỡng vật lý tức thời và mô hình học máy (AI) để phát hiện té ngã chính xác nhất.

## 1. Phương pháp Ngưỡng vật lý (Threshold-based)
Sử dụng dữ liệu gia tốc từ cảm biến **MPU6050** để theo dõi các biến đổi đột ngột:

*   **Giai đoạn Rơi tự do (Free Fall):** Tổng gia tốc 3 trục (vector sum) giảm xuống **< 0.8G**. Đây là lúc người dùng bắt đầu bị mất thăng bằng và rơi xuống.
*   **Giai đoạn Va chạm (Impact):** Sau giai đoạn rơi tự do (trong vòng 1.5s), nếu gia tốc tăng vọt lên **> 1.6G**, hệ thống xác định có sự va chạm mạnh với mặt sàn.
*   **Va chạm cực mạnh:** Nếu gia tốc đột ngột **> 3.0G**, hệ thống sẽ kích hoạt cảnh báo ngay lập tức (phòng trường hợp té ngã mạnh mà không trải qua giai đoạn rơi tự do).

## 2. Phương pháp Trí tuệ nhân tạo (AI Deep Learning)
Sử dụng bộ phân loại được huấn luyện từ **Edge Impulse** (thư viện `healthycare_inferencing`):

*   **Cửa sổ dữ liệu:** Hệ thống lấy mẫu gia tốc liên tục và lưu vào một mảng dữ liệu (Sliding Window).
*   **Suy luận AI (Inference):** Khi mảng dữ liệu đầy, AI sẽ phân tích "mẫu hình di chuyển" (như độ rung lắc, hướng ngã, lực tác động) thay vì chỉ nhìn vào một con số đơn lẻ.
*   **Xác suất:** Nếu kết quả phân tích cho nhãn "Fall" có độ tin cậy **> 0.75 (75%)**, cảnh báo té ngã sẽ được kích hoạt.

## 3. Quy trình Cảnh báo thông minh
Để giảm thiểu báo động giả (ví dụ: máy bị rơi nhưng người không sao):

1.  **Cảnh báo tạm thời:** Hệ thống vào trạng thái `fallWarning` và hiển thị màn hình đếm ngược 10 giây.
2.  **Giai đoạn chờ:** Người dùng có thể nhấn vào màn hình hoặc chọn "Hủy" trên ứng dụng điện thoại để dừng báo động nếu vẫn bình thường.
3.  **Kích hoạt khẩn cấp:** Nếu sau 10 giây không có sự can thiệp từ người dùng:
    *   Trạng thái `isFalling` được xác nhận.
    *   Còi Buzzer hú liên tục.
    *   Gửi tín hiệu SOS và tọa độ GPS lên Firebase để người thân nhận được ngay tức khắc.

## 4. Công thức tính Vector Gia tốc
Công thức tính tổng lực tác động từ 3 trục X, Y, Z:
`VectorSum = sqrt(accX^2 + accY^2 + accZ^2)`
