# TEAM — Ngày 12

Mã bộ ảnh: **G10** · Hình thức: nhóm / cá nhân (Đội 034 · Rất khó nghĩ)

| Họ tên | MSSV | Đã gán đủ 22 ảnh? | Review chéo với (peer diff) | Số dòng disagreement đã xử lý |
|---|---|---|---|---|
| Phan Hải Đăng | 2A202603229 | có | Nguyễn Anh Tiến (2A202603120) | 120 |
| Nguyễn Anh Tiến | 2A202603120 | có | Phan Hải Đăng (2A202603229) | 120 |

Ca hai bạn vẫn không thống nhất và đã hỏi coach (ghi tên ảnh + đối tượng):

- `G10 S02_rgb.png` / `S02_ir.png` (`child_seat` ở `rear_middle` có hình mô hình trẻ em ngồi): Đã xác nhận với coach dùng bản cập nhật `cabin_schema.py` cho phép `child_seat.occupied = yes` và không vẽ thêm box `person` cho mô hình trẻ em ngồi trong ghế trẻ em.
- `G10 D01.png` & `D03.png` (`face13` trên mặt tài xế quay nghiêng góc lớn sang ghế phụ): Thống nhất giữ các điểm mốc mặt nhìn thấy được ở nửa mặt trái và bật cờ `occluded`/`outside` cho các điểm bị che khuất thay vì để toàn bộ 13 điểm `outside=1` (gây cảnh báo `face13 không có điểm nào hiện`).

## Cá nhân

### PhanHaiDang-2A202603229
- Phần mình làm (ngoài 22 ảnh của mình, ví dụ ai review ai, ai tổng hợp ca hỏi coach): Tự gán hoàn chỉnh 22/22 ảnh bộ `G10`, chạy `cabin_check` đạt 0 lỗi / 0 cảnh báo ở cả Checkpoint A, Checkpoint B và Final; chạy `cabin_check diff` đối chiếu toàn bộ 120 dòng bất đồng với file `G10_Day12_review.zip` của bạn Nguyễn Anh Tiến, điền đầy đủ `disagreement_log.csv` và tinh chỉnh biên box trong `export_final_v2.zip` (`check_final_v2.txt` đạt 0 lỗi, 0 cảnh báo).
- Điều mình học được hôm nay: Nắm vững quy ước hệ quy chiếu gắn với xe (`rear_right` nằm bên trái ảnh khi camera nhìn về sau) và hệ quy chiếu gắn với người (`left_eye_*` và tay trái `side=left` có tung độ $y$ lớn hơn trên ảnh DMS xoay 90°), kỷ luật đổi ngay thuộc tính mặc định `seat=driver` của CVAT, ghi `unknown` / bật cờ `occluded`/`outside` thay vì đoán, và phân biệt ảnh IR thật (NIR 940 nm) với ảnh IR mô phỏng (SVIRO).
