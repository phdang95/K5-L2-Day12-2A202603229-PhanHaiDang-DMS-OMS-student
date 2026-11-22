# Tự đánh giá — Phan Hải Đăng · G10

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| `S01_rgb.png` | `face` (của `person` ở `rear_left`) & `frame_meta` | `lr_flip`, `box`, `guess` | Lần vẽ đầu tiên cạnh trên box `face` bao gồm cả phần tóc phồng phía trên, `eye_left`/`eye_right` bị gán theo hướng ảnh thay vì theo người, và `frame_meta.light` để `night` thay vì `unknown` cho ảnh dựng SVIRO. | Đã sửa (`eye_left=open`, `eye_right=unknown`, thu sát box `face` tới trán, đổi `light=unknown`) |
| `S03_ir.png` | `object` (`bag` ở `rear_right`) | `seat_flip` | Khi mới vẽ box `object` cho chiếc túi xách ở bên trái ảnh, CVAT tự động điền giá trị mặc định đầu danh sách là `seat=driver` và `type=other`. | Đã sửa (`seat=rear_right`, `type=bag`) |
| `D03.png` | `face` (`eye_left`) | `guess` | Ban đầu để `eye_left=open` dù tài xế quay đầu sang ghế phụ khiến mắt trái bị khuất sau gọng kính (trong khi các điểm `left_eye_upper`/`lower` đã bật cờ `occluded`). | Đã sửa (`eye_left=unknown` để nhất quán với cờ `occluded` và quy tắc không thấy thì `unknown`) |
| `D04.png` | `person` (`seatbelt`) & `phone` | `missing`, `box` | Ban đầu để `seatbelt=unknown` dù dải dây đai đen vắt chéo ngực rất rõ, và box `phone` lần đầu vẽ hơi lấn sang sống mũi (`nose_tip`). | Đã sửa (`seatbelt=worn_correct`, chỉnh lại box `phone` ôm đúng thân máy áp từ miệng lên tai phải под các ngón tay) |

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: Quên đổi giá trị mặc định của CVAT sau khi vẽ box mới (`seat=driver` trên các box OMS hoặc `light=day/night` trên ảnh màu SVIRO `S01_rgb`..`S08_rgb`).

Vì sao mình mắc: CVAT tự động gán phần tử đầu tiên trong danh sách thuộc tính (`driver` cho `seat`, `other` cho `type`), và khi nhìn ảnh RGB của SVIRO có ánh sáng mô phỏng ngày/đêm dễ theo phản xạ thị giác chọn `day`/`night` thay vì nhớ quy tắc dữ liệu tổng hợp (synthetic) phải để `light=unknown`.

Lần sau tránh bằng cách: Tạo thói quen bắt buộc mở ngay bảng **DETAILS** bên phải ngay sau khi thả chuột vẽ xong bất kỳ box/tag nào để kiểm tra thuộc tính trước khi vẽ đối tượng tiếp theo; luôn chạy `cabin_check.py` ngay sau từng cụm ảnh để bắt sớm cảnh báo `[WARN]`.

## 3. Ghi chú về disagreement_log

Một dòng bạn của mình đúng và mình sai (hoặc điểm dễ bất đồng nhất khi đối chiếu): Ở các khung `S01_rgb.png` (ghế `child_seat` trong vùng tối bên trái ảnh) và `D01.png` (`seatbelt` đang cầm trên tay thắt dở), mình học được rằng quy tắc **"Không thấy rõ hoặc đang thao tác dở thì ghi `unknown`"** (Guideline §4.4-1 và §4.7) quan trọng hơn việc cố suy luận từ bức ảnh IR cùng cảnh hay từ ngữ cảnh, giúp tránh dạy model học nhầm các mẫu nhiễu (noisy labels).
