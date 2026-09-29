# Tự soát


## Checklist thủ công
- [x] Phạm vi H=40 và vật cần vẽ — soát từng frame ở độ phóng đại 2–4x; hai xe hơi/sedan mờ ở hậu cảnh
  adasind_036720.jpg (~35px) dưới ngưỡng H=40 nên không vẽ, đúng R01.
- [x] lens_border và ego_body — cả 3 frame đều có 2 polygon lens_border (đã import sẵn, chỉ soát lại) và
  polygon ego_body tôi tự vẽ (tay/ống tay áo người lái + một phần yên/kính xe ở adasind_036720.jpg).
- [x] Class sáu nhãn — Car, Bike, ThreeWheeler, Pedestrian, Truck theo đúng bảng 6 class ở docs/02-rules-vi.md.
- [x] Rider và Bike — hai trường hợp người + xe hai bánh (người lái xe máy ở adasind_012570.jpg, người đạp xe
  ở adasind_036720.jpg) đều gộp thành một box `Bike` duy nhất theo R03.
- [x] Geometry trên ảnh fisheye gốc — box/polygon vẽ theo pixel ảnh gốc, không nắn thẳng vật ở rìa cong.
- [x] truncated và occluded — `truncated` lấy theo hàm hình học (`svm11.zones.truncated`) so khớp vòng kính/biên
  khung; `occluded` phán đoán bằng mắt cho từng box.
- [x] Vật thiếu hoặc box trùng — không có cặp box cùng class IoU > 0.7 (selfqc tự động không báo dòng nào).
- [x] ignore_region có reason — mọi polygon ignore đều có đúng một `reason` (`lens_border` hoặc `ego_body`).
- [x] Tên task raw_fisheye và export CVAT 1.1 — task `Day11 · ADASIND · B1-dense · raw_fisheye`, export
  định dạng CVAT for images 1.1.

## Fill ratio (K12)
- adasind_001320.jpg box 3 edge: 0.731
- adasind_001320.jpg box 4 edge: 0.714
- adasind_001320.jpg box 5 center: 0.937
- adasind_036720.jpg box 1 edge: 0.781
mean edge: 0.742 (n=3)
mean center: 0.937 (n=1)
