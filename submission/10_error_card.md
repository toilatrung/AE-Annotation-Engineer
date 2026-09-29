# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B1 | BOX_GEOMETRY | 1 |
| center | B1 | MISSING | 7 |
| center | B1 | SPURIOUS | 8 |
| center | B1 | WRONG_CLASS | 1 |
| center | C0 | MISSING | 1 |
| edge | B1 | MISSING | 2 |
| edge | B1 | SPURIOUS | 4 |
| mid | B1 | ATTRIBUTE | 1 |
| mid | B1 | MISSING | 3 |
| mid | B1 | SPURIOUS | 1 |
| mid | C0 | MISSING | 1 |
| mid | C0 | SPURIOUS | 1 |
| unknown | B1 | BOX_GEOMETRY | 2 |

## Top defects
- SPURIOUS: 14 (ví dụ frame adasind_019560.jpg)
- MISSING: 14 (ví dụ frame adasind_019560.jpg)
- BOX_GEOMETRY: 3 (ví dụ frame adasind_036720.jpg)

## Đọc `r3_diag/local_quality.md`

Đây là phép đo offline (ghép IoU≥0.5, chỉ rectangle, so với **teaching reference** — không phải gold set) trên
bản export `r1_craft` **đã khóa** (`E19E-0F0F`, trước rework): `TP=16, FP=2, FN=3`, trên 20 lần đối chiếu hình học
→ `micro accuracy=0.800`, `micro precision=0.889`, `micro recall=0.842`, `mean IoU của các cặp TP=0.834` (khớp
hình học khá chặt khi đã bắt đúng cặp). Đọc theo class: **`Bike` là class yếu nhất** — `TP=3, FP=2, FN=2`,
`precision=recall=0.600`, thấp hẳn so với `ThreeWheeler`/`Pedestrian`/`Truck` (đều đạt 1.000). Theo frame:
`adasind_012570.jpg` kéo toàn bộ số xấu xuống (`TP=6, FP=2, FN=3, accuracy=0.600`) trong khi hai frame còn lại
đạt `accuracy=1.000`.

**Truy một xung đột cụ thể về đúng ảnh:** dòng `mismatching_label` trong
`r3_diag/local_quality_confusion.csv`/`local_quality_conflicts.csv` ghi rõ `frame=adasind_012570.jpg, left#6
(Bike) ↔ reference#7 (Car), IoU=0.573` — đây chính là ca `L6` đã phân tích ở trên (người lái xe máy che một phần
xe hơi phía sau); hai dòng `missing_annotation` (`reference#8`, `reference#9`, đều `Bike`) cũng cùng cụm này. Đây
là lý do duy nhất khiến `Bike` tụt xuống `precision/recall=0.6` — không phải `Bike` khó nói chung, mà là **một
cụm chồng lấn cụ thể** đã sửa ở vòng rework (số liệu `local_quality` này đứng yên vì đo trên bản khóa `r1_craft`
gốc, không tự cập nhật sau rework — muốn thấy số mới phải chạy lại `local-quality` trên bản rework, ngoài phạm vi
bắt buộc của bài).

**Giới hạn khi đọc số này:** chỉ 3 frame/20 lần đối chiếu — một cụm lỗi duy nhất (ca `L6`) đã đủ kéo `Bike
recall` từ 1.0 xuống 0.6, nên chỉ số theo class ở mẫu nhỏ này rất nhạy với từng ca đơn lẻ, không nên suy rộng
thành "Bike khó gán nhãn hơn ThreeWheeler". Ngưỡng ghép `IoU≥0.5` là quy ước riêng của lab, không phải chuẩn
ngành; và `reference` là bản nháp Lab Coach sửa tay (`docs/05-taxonomy-vi.md`), không phải gold set đã kiểm
nhiều người — số đo này mô tả độ khớp với MỘT bản tham chiếu, không phải điểm rubric hay chứng nhận chất lượng.

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- **Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy:** Phần lớn SPURIOUS/MISSING ở `center` (8 spurious, 7
  missing tại slice B1-dense) đến từ **một mẫu lặp lại 5 lần trong cả 3 frame**: model (frozen YOLO26m) không có
  lớp `ThreeWheeler`, luôn thay bằng `Car` và/hoặc `Truck` — ví dụ `adasind_001320.jpg` L2+R3 (M5 gán `Truck`),
  `adasind_012570.jpg` L5+R4 (model xuất cả `M8 Car` **và** `M10 Truck` trùng khít cho cùng một box),
  `adasind_036720.jpg` L3+R3 (`M4 Car`). Đây là `E4_model_domain` có bằng chứng lặp lại nhiều lần, không phải suy
  đoán từ một ca — nên `action=escalate` cho `ai_team` thay vì chỉ ghi chú. Nguyên nhân thứ hai (khác hẳn, do
  chính tôi) là ca `adasind_012570.jpg` object `L6`: tôi vẽ một box `Bike` rộng trùm cả người lái xe máy **và**
  một phần xe hơi khuất phía sau (`E1_annotator_error`) — cả reference (`R7 Car`, `R8 Bike`) và model
  (`M9 Car`, `M6 Bike`) đều độc lập xác nhận đây là hai vật thể tách biệt, nên đủ bằng chứng để rework thay vì chỉ
  ghi nhận là góc nhìn khác (`E0`).
- **Cách sửa và ai nhận việc (`owner`):** Ca `L6` do tôi (`owner=annotator`) sửa ngay trong vòng rework — đã tách
  thành `Bike` (330,923,360,992) và `Car` (335,922,385,979) theo toạ độ R/M, xem `submission/rework/delta.md`
  (zone `center`: matched 6→8, missing 3→1, spurious 2→1). Ca thiếu lớp `ThreeWheeler` của model không thuộc phạm
  vi sửa của tôi — đã ghi `owner=ai_team, action=escalate` trong `findings.csv` và đề xuất bổ sung lớp riêng ở
  `20_guideline_patch.md`.
- **Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule):** Chi tiết toạ độ và box của cả ba nguồn (L/R/M)
  cho từng ca nằm trong `findings.csv` cột `evidence` (dòng `round=r3_diag`, các `object_ref` L2+R3, L3+R6, L5+R4,
  L7+R6, L3+R3, L6, R7+M9, R8+M6...); rule liên quan: R01 (ngưỡng H=40, không ảnh hưởng ca này), R05 (`truncated`
  hình học), và nguyên tắc "một vật thể — một box" (chưa có `rule_id` riêng, đề xuất thêm ở
  `20_guideline_patch.md`). Ảnh chụp minh chứng ở `screenshots/l6_merge_before_after.png`.
