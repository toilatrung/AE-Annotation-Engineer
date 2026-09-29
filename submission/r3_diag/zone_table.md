# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 7 | 1 | 4 | 3 | 5 | SPURIOUS (3) |
| mid | 9 | 1 | 1 | 6 | 7 | WRONG_CLASS (1) |
| edge | 4 | 0 | 0 | 1 | 1 | — |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên: **L gãy nhiều nhất ở `center`**: 4 spurious + 1 missing trên n_ref=7. Cụ thể là 014670 L2 (mảng vàng sau người, E1), 014670 L6 (ca sát H=40), 034080 L9 (vật < H=40, E1) và 034080 L10/R2 (box lệch lên ~20 px, spurious + missing cùng một xe). **M gãy nhiều nhất ở `mid`**: 6 missing + 7 thừa trên n_ref=9. `edge` ít lỗi nhất cho cả hai (L 0/0; M 1/1 trên n_ref=4).
- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame: lỗi của **L** không do méo rìa. Chúng tập trung ở vật **nhỏ và xa gần tâm ảnh** (cao 35–47 px), nơi chênh vài pixel ở mép trên đổi vật từ ngoài sang trong phạm vi H=40, và một ca đọc quá mức vật bị che. Lỗi của **M** ở `mid` gần như toàn bộ là **sai taxonomy chứ không sai hình học**: xe ba bánh bị gọi Truck/Car (5 box, IoU cao) và rider bị tách thành Pedestrian + Bike (001320 M3; 034080 M7, M8, M9, M12). `compare` tính mỗi ca sai class thành một missing + một thừa, nên số M ở `mid` bị thổi phồng; vật ở mid của slice này lại chủ yếu là xe ba bánh và xe máy chở người. Không kết luận "model yếu ở mid vì méo": `edge` (vùng méo nhất) M chỉ sai 1/4, và `iou_sweep.md` cho thấy số M ở mid gần như không đổi từ IoU 0.3 đến 0.7 (matched 4→3), tức vấn đề là class chứ không phải độ ôm box. **Giới hạn:** chỉ 3 frame, 20 box reference, 1 camera; mỗi zone chỉ 4–9 vật, một ca đổi là đổi 10–25% số của zone. Teaching reference có ca sát ngưỡng còn tranh luận (014670 L6), nên các số này là tín hiệu để chọn ca soi, không phải tỷ lệ lỗi.
