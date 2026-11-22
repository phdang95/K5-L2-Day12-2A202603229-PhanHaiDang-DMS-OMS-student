# Danh sách lớp v0 — Phan Hải Đăng · G10

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `face` (`eye_left`, `eye_right`) | `open` / `closed` / `unknown` | Tính chỉ số PERCLOS (tỷ lệ thời gian mắt nhắm) và phát hiện tài xế buồn ngủ hoặc ngủ gật (microsleep) | Có (rất rõ nhờ đèn LED NIR 940 nm, trừ khi bị lóa phản xạ tròng kính hoặc quay mặt đi) |
| 2 | `face` (`gaze_zone`) | `road` / `mirror_l` / `mirror_r` / `mirror_c` / `cluster` / `center` / `lap` / `other` | Phát hiện mất tập trung thị giác (visual distraction) khi tài xế nhìn rời khỏi mặt đường (`road`) quá ngưỡng thời gian an toàn (ví dụ nhìn xuống đùi `lap` hay màn hình `center`) | Có (dựa vào hướng đầu và vị trí đồng tử/mống mắt trên ảnh NIR) |
| 3 | `face` (`glasses`) | `none` / `clear` / `sun` / `unknown` | Đánh giá độ tin cậy của nhánh ước lượng mắt/gaze và chuyển chế độ cảnh báo theo hướng đầu (head pose) khi tài xế đeo kính râm tối (`sun`) hoặc kính phản xạ | Có (thấy rõ gọng kính; tuy nhiên trên NIR tròng kính râm có thể trở nên trong suốt một phần) |
| 4 | `face13` (13 keypoints) | Tọa độ $(x, y)$ + cờ `occluded` / `outside` | Ước lượng góc quay đầu 3D (yaw, pitch, roll), độ mở mí mắt (EAR - Eye Aspect Ratio) và độ mở miệng (MAR - phát hiện ngáp) | Có (nhận diện tốt các mốc giải phẫu mặt cả ban ngày lẫn ban đêm) |
| 5 | `hand` (`whose`, `side`, `on_wheel`) | `driver`/`passenger` · `left`/`right` · `yes`/`no`/`unknown` | Giám sát trạng thái tay đặt trên vô lăng (Hands-On-Wheel Detection) để quyết định quyền chuyển giao điều khiển L2/L3 (takeover readiness) và phân biệt thao tác của tài xế với hành khách ghế phụ | Có (nhưng vùng dưới vô lăng có thể bị bóng khuất, cần cân chỉnh dải tương phản NIR) |
| 6 | `phone` (`state`) | `held` / `mounted` / `lap` | Cảnh báo hành vi nguy hiểm và vi phạm luật giao thông khi tài xế cầm điện thoại nghe/nhắn tin (`held`) hoặc để trên đùi (`lap`) khi đang vận hành xe | Có (nhận diện qua hình dáng cạnh, góc máy hoặc ánh sáng phát ra từ màn hình) |
| 7 | `person` (`seat=driver`, `seatbelt`) | `worn_correct` / `worn_incorrect` / `not_worn` / `unknown` | Nhắc nhở thắt dây an toàn đúng quy cách; phát hiện trường hợp cài dây giả sau lưng hoặc luồn dưới nách (`worn_incorrect`) gây nguy hiểm khi bung túi khí | Có (dải dây đai tạo tương phản độ sáng với quần áo trên ảnh NIR) |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | `person` (`seat`) | `front_passenger` / `rear_left` / `rear_middle` / `rear_right` | Xác định sơ đồ chỗ ngồi có người trong khoang xe (Seat Occupancy Detection) để tối ưu lực bung túi khí, điều hòa theo vùng và cảnh báo dây an toàn từng ghế | Có (nhận diện hình thể người ngồi ở mọi điều kiện ánh sáng ngày/đêm) |
| 2 | `person` (`seatbelt`) | `worn_correct` / `worn_incorrect` / `not_worn` / `unknown` | Kích hoạt cảnh báo dây an toàn độc lập cho từng hành khách hàng ghế sau (Rear Seatbelt Reminder theo chuẩn Euro NCAP) | Có (khi không bị vật khác che khuất hoàn toàn) |
| 3 | `infant_seat` (`seat`, `facing`) | Ghế theo xe · `rear` / `front` | Phát hiện nôi trẻ sơ sinh (đặc biệt loại quay ngược `facing=rear`) để tự động vô hiệu hóa túi khí đối diện tránh chấn thương tử vong cho trẻ và kích hoạt cảnh báo bỏ quên trẻ (CPD) | Có (nhận diện qua cấu trúc khung nôi, quai xách và hướng lòng nôi) |
| 4 | `child_seat` (`seat`, `occupied`) | Ghế theo xe · `no` / `yes` / `unknown` | Phân biệt ghế trẻ em đang có trẻ ngồi (`occupied=yes`) hay ghế trống (`occupied=no`) để kích hoạt hệ thống Child Presence Detection (CPD) chống bỏ quên trẻ em trong xe khóa kín | Có (trên ảnh IR/NIR nhìn rõ lòng ghế trẻ em ngay cả khi khoang sau tối như ở cảnh `S01`) |
| 5 | `object` (`seat`, `type`) | Ghế theo xe · `bag` / `box` / `cup` / `other` | Tránh báo động giả (False Positive) cho cảm biến trọng lượng ghế (không nhầm túi xách/thùng hàng nặng là người không thắt dây) và nhắc khách hàng quên đồ vật khi rời xe | Có (nhận diện theo biên dạng hình học, nhưng mất đặc trưng màu sắc vật) |
| 6 | `frame_meta` (`camera_blocked`, `modality`, `light`) | `yes`/`no` · `rgb`/`ir` · `day`/`night`/`unknown` | Tự chẩn đoán lỗi che khuất ống kính camera (Camera Blockage / Tampering Detection) để báo lỗi hệ thống DMS/OMS kịp thời thay vì đưa ra dự đoán sai | Có (phát hiện ngay khi toàn khung hình bị vật cản che hoặc mất tín hiệu) |

## 2 câu hỏi leo thang

Câu hỏi bạn muốn hỏi người viết guideline (hoặc khách hàng) vì guideline v1.0 chưa trả lời. Mỗi câu kèm **tên khung** làm ví dụ.

1. **Câu hỏi:** Khi trên ghế trẻ em (`child_seat`) hoặc nôi em bé (`infant_seat`) có trẻ em / mô hình trẻ em đang ngồi hoặc nằm bên trong (sau khi schema đã mở rộng cho phép `child_seat.occupied = yes`), chúng ta chỉ gán duy nhất 1 box `child_seat` (`occupied = yes`) / `infant_seat`, hay có cần thêm thuộc tính `occupied = yes/no` cho cả `infant_seat` để phân biệt nôi trống (`S02`, `S06`, `S08`) với nôi có em bé nằm (`S03`, `S05`, `S07`) phục vụ bài toán Child Presence Detection (CPD)?
   - **Khung ví dụ:** `G10 S02_ir.png` / `S02_rgb.png` (ghế `rear_middle` có trẻ ngồi trên `child_seat`) và `G10 S03_ir.png`, `S05_ir.png`, `S07_ir.png` (ghế `infant_seat` có em bé nằm bên trong) so với `G10 S06_ir.png`, `S08_ir.png` (`infant_seat` trống).
   - **Bạn tạm gán thế nào và vì sao:** Với `S02` ở ghế `rear_middle`, mình cập nhật `cabin_schema.py` theo hướng dẫn của coach để gán 1 box `child_seat` (`seat=rear_middle`, `occupied=yes`) và không vẽ `person` (vì Guideline 4.2 quy định mỗi ghế một box chính và `person` chỉ dành cho người lớn). Với `S03`, `S05`, `S07`, mình gán 1 box `infant_seat` kèm đúng hướng `facing` (`front` ở `S03, S05`; `rear` ở `S07`) vì schema `infant_seat` hiện tại chưa có trường `occupied`.
2. **Câu hỏi:** Đối với các trường hợp tài xế quay đầu nghiêng góc rất lớn sang ghế phụ (> 80°–90°) khiến camera chỉ còn thấy phần gáy, tai, râu và một góc nhỏ của gọng kính mà không thấy trực tiếp nhãn cầu hay đầu mũi, ngưỡng cụ thể để quyết định "không vẽ box `face`" (theo mục 4.5) so với "vẫn vẽ box `face` (`eye_*=unknown`) + `face13` với các điểm bật cờ `occluded`/`outside`" là bao nhiêu phần trăm diện tích vùng mặt?
   - **Khung ví dụ:** `G10 D01.png` và `G10 D03.png` (tài xế quay mặt hẳn sang phía ghế phụ bên phải, chỉ lộ sườn mặt trái, tai trái và gọng kính bên trái).
   - **Bạn tạm gán thế nào và vì sao:** Mình vẫn vẽ box `face` ôm phần sườn mặt/đầu nhìn thấy được với `eye_left=unknown`, `eye_right=unknown`, `glasses=clear`, `gaze_zone=other`, đồng thời vẽ `face13` trong đó các điểm bị che bởi gọng kính/góc nghiêng bật cờ `occluded` và các điểm khuất hoàn toàn ở nửa mặt phải bật cờ `outside`. Cách này giúp model DMS vẫn giữ được bounding box khuôn mặt và hướng quay đầu cực đại (extreme head yaw) để phát hiện tài xế không nhìn đường mà không làm bẩn dữ liệu mắt mở/nhắm.
