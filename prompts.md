# Khai báo dùng AI — Phan Hải Đăng

Nhắc lại: không đưa ảnh cabin hay ảnh tài xế (kể cả ảnh chụp màn hình) cho bất kỳ công cụ AI nào.

| # | Công cụ | Bạn hỏi gì (tóm tắt) | Bạn dùng kết quả thế nào | Bạn có kiểm lại không? Bằng gì? |
|---|---|---|---|---|
| 1 | Antigravity AI Assistant (Local CLI/IDE) | Hướng dẫn lệnh cài đặt môi trường, giải nén gói dữ liệu vào thư mục repo, cú pháp chạy script `cabin_check.py` ở các Checkpoint A, B, Final và tra cứu quy tắc trong `GUIDE.md` / `COMMON_ERRORS.md` | Dùng các lệnh CLI để chạy `scripts/cabin_check.py`, cập nhật `cabin_schema.py` theo bản vá `child_seat.occupied=yes` của coach và kiểm tra định dạng file nộp theo `SUBMISSION.md` | Có. Tự kiểm tra trực tiếp trên giao diện CVAT (`localhost:8080`), đối chiếu từng thuộc tính với `GUIDE.md` và xác nhận bằng kết quả `0 lỗi, 0 cảnh báo` từ `python scripts/cabin_check.py check` |
