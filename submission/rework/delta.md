# Rework delta

| zone | matched before | matched after | missing before | missing after | spurious before | spurious after |
|---|---:|---:|---:|---:|---:|---:|
| center | 6 | 7 | 1 | 0 | 4 | 1 |
| mid | 8 | 8 | 1 | 1 | 1 | 1 |
| edge | 4 | 4 | 0 | 0 | 0 | 0 |

## Findings action=rework
- adasind_014670.jpg L2 SPURIOUS: đã sửa
- adasind_034080.jpg L9 SPURIOUS: đã sửa
- adasind_034080.jpg L10+R2 BOX_GEOMETRY: đã sửa
- adasind_014670.jpg L2 SPURIOUS: đã sửa
- adasind_034080.jpg L9 SPURIOUS: đã sửa
- adasind_034080.jpg L10+M3 SPURIOUS: đã sửa
- adasind_034080.jpg R2 MISSING: đã sửa

## Diễn giải (huy)

- **Chỉ sửa 3 ca P1 có căn cứ ảnh**, cùng một task CVAT `Day11 · ADASIND · B1-edge · raw_fisheye`, export task CVAT 1.1, khoá `A2E1-0B86`. Bản trước là r1_craft `9534-59A5`.
  1. `adasind_014670.jpg` L2 ThreeWheeler (522,952)–(560,1010): **xoá**. Crop 5× chỉ thấy mảng vàng sau người đứng, không có hình dạng xe; R và M đều không có (E1, R01).
  2. `adasind_034080.jpg` L9 ThreeWheeler (335,1063)–(375,1105): **xoá**. Vật ở xa nằm dưới H=40 theo cả hai nguồn: R vẽ một ThreeWheeler cao 35 px, M thấy hai Car cao 35/39 px. Box 42 px của tôi kéo mép trên quá phần thấy (E1, R01).
  3. `adasind_034080.jpg` L10 Car: (512,1068)–(572,1135) → **(509,1086)–(578,1153)**. Box cũ lệch lên ~20 px so với nóc/đáy xe trắng thấy trên ảnh (E1, R02).
- **Số trước → sau** (IoU 0.5, bảng trên): center matched 6→7, missing 1→0, spurious 4→1; mid và edge không đổi. Tổng box 23→21.
- **Còn lại sau rework, và vì sao không sửa:**
  - center spurious 1 = `adasind_014670.jpg` L6 ThreeWheeler (360,953)–(397,1000), cao 47 px (M7 cao 49 px). R **có** vẽ vật này nhưng box cao 37 px < H=40 nên bị loại khỏi phép so. Đây là ca sát ngưỡng: lệch 7–10 px ở mép trên đổi trong/ngoài phạm vi. Tôi **escalate** (E2, xin dải dung sai H), không xoá box cho khớp số.
  - mid missing 1 + spurious 1 = cùng một vật `adasind_014670.jpg` L4 Bus / R5 Truck (IoU 0.872). Phần phân biệt class nằm ngoài khung, nên **escalate** E2 (guideline gap), không tự đổi class theo reference.
- **Giới hạn phép so:** chỉ 3 frame, 20 box reference. Teaching reference không phải gold set, và một finding E0 có thể làm tăng "spurious" của L dù L đúng. Attribute `occluded` không được so (R11).
