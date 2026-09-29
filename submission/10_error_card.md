# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B1 | BOX_GEOMETRY | 1 |
| center | B1 | MISSING | 3 |
| center | B1 | SPURIOUS | 11 |
| center | B1 | WRONG_CLASS | 2 |
| center | C0 | SPURIOUS | 2 |
| edge | B1 | BOX_GEOMETRY | 1 |
| edge | B1 | MISSING | 1 |
| edge | B1 | SPURIOUS | 1 |
| mid | B1 | BOX_GEOMETRY | 1 |
| mid | B1 | MISSING | 6 |
| mid | B1 | SPURIOUS | 7 |
| mid | B1 | WRONG_CLASS | 3 |
| mid | C0 | SPURIOUS | 1 |

## Top defects
- SPURIOUS: 22 (ví dụ frame adasind_019560.jpg)
- MISSING: 10 (ví dụ frame adasind_001320.jpg)
- WRONG_CLASS: 5 (ví dụ frame adasind_014670.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy: lỗi nổi bật nhất là **SPURIOUS (22)**, nhưng con số gộp này chứa hai nhóm khác bản chất.
  - (a) **11/22 dòng là box chỉ model có** (`M_only` ở r3_diag). Tôi xếp 10 dòng vào `E4_model_domain` và 1 dòng vào `E5_unresolved` (034080 M11, vật quá mờ) vì cùng một mẫu lặp trên cả 3 frame, không phải một box lệch. Xe ba bánh bị gọi Truck/Car với IoU cao (001320 M5, M6; 014670 M6, M7; 034080 M10), và rider bị tách thành Pedestrian (001320 M3; 034080 M7, M8, M12). Nguyên nhân khả dĩ là taxonomy model khác quy ước R03/R04 của khoá, không phải méo fisheye, vì `edge` ít lỗi nhất.
  - (b) **11 dòng còn lại là box của tôi (L)**. Chúng chỉ ứng với 8 vật, vì `compare` ghi cùng vật ở cả dòng `r1_craft` lẫn `r3_diag`: C0 L6 (tách người ngồi trên yên thành Pedestrian, E1/R03), C0 L9 (tấm nhựa xanh, E1), 014670 L2 (mảng vàng sau người, E1), 034080 L9 (vật < H=40, E1) và 034080 L10 (box lệch, E1/R02). Ba ca không phải lỗi annotator: 014670 L6 là ca sát ngưỡng H=40 (`E2_guideline_gap`, R vẽ cùng vật nhưng cao 37 px); 014670 L4 Bus/Truck không phân định được vì đầu xe ngoài khung (`E2`); C0 L5 là xe máy đỗ bị che quá nửa (`E5_unresolved`).
- Cách sửa và ai nhận việc (`owner`):
  - Nhóm (b), lỗi annotator, đã **rework** trong B1-edge (`rework/delta.md`: center spurious 4→1, missing 1→0). C0 không khoá lại, nhưng bài học rider được áp dụng ở P2: 034080 L5/L6 đúng rider theo R.
  - Nhóm (a) giao `ai_team`: ánh xạ lại lớp đầu ra (auto-rickshaw → ThreeWheeler, gộp person-on-two-wheeler → Bike) hoặc fine-tune trước khi dùng model làm pre-label. Không sửa nhãn L theo model.
  - Ca H=40 và Bus/Truck giao `guideline` qua `30_escalation_ticket.md` và `20_guideline_patch.md`.
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule): `screenshots/b1edge_014670_L6_H40_borderline.png` (L/R/M cùng khung: M gọi ThreeWheeler là Truck/Car, R box 37 px), `screenshots/b1edge_034080_rework_before_after.png` (L9 xoá, L10 hạ xuống), `screenshots/c0_019560_rider_and_blue_sheet.png` (R03), `screenshots/b1edge_014670_L4_bus_vs_truck.png` (R04). Dòng findings: `r3_diag` 001320 M3/M5/M6, 014670 M6/M7, 034080 M7–M12 (E4); `r1_craft` 014670 L2, 034080 L9/L10 (E1, rework); 014670 L6 và L4 (E2, escalate). Số liệu: `r3_diag/model_compare.md`, `iou_sweep.md` (M ở mid không cải thiện khi hạ IoU xuống 0.3 → lỗi class, không phải hình học).
