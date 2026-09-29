# Tự soát

Slice `B1-edge` (huy): `adasind_001320.jpg`, `adasind_014670.jpg`, `adasind_034080.jpg`. Tôi soát trên ảnh gốc phóng to 2–3× có lưới toạ độ, theo đúng thứ tự 9 mục của `docs/04-selfqc-checklist-vi.md`, **trước khi khoá và trước khi mở reference của slice**.

## Cảnh báo tự động đã xử lý
- Lần `selfqc` trên bản nháp (`exports/r1-draft.zip`, export **mức job**) báo `Tên task thiếu raw_fisheye`. Nguyên nhân: export của job chỉ ghi `<job>` trong meta, không có tên task. Tên task trong CVAT vẫn là `Day11 · ADASIND · B1-edge · raw_fisheye`. Tôi export lại ở **mức task** (CVAT for images 1.1, không kèm ảnh) thành `exports/r1-final.zip`, chạy lại `draft` → `fill` → `selfqc`: cảnh báo đã hết.
- Sửa sau self-QC (mục 6): Truck `adasind_001320.jpg` (342,810)–(460,945) đổi `occluded` false → **true**, vì người lái xe máy (Bike 454–533) che góc phải-dưới của thùng xe.

## Checklist thủ công
- [x] Phạm vi H=40 và vật cần vẽ: đủ 6 + 7 + 10 box (001320 / 014670 / 034080). Đã cân nhắc và **không** box các vật có phần nhìn thấy thấp hơn 40 px: ô tô nhỏ (220–237, 857–884) cao 27 px ở 001320; xe ba bánh (403–430, 955–988) cao 33 px và xe máy đỗ (~440–470, 970–1000) ở 014670.
- [x] lens_border và ego_body: 2 `lens_border` import sẵn ở mỗi frame khớp vòng kính theo `assets/frames.csv` (001320: tâm 606,891, r=810; 014670: 455.7,895.1, r=811.9; 034080: 483.5,922.1, r=811.3). Tôi không sửa hai polygon này. Cả ba frame đều **thấy** tay, tay lái và đùi người lái ở góc trái-dưới, nên mỗi frame có một `ego_body` tự vẽ: 001320 khoảng x 0–300, y 1100–1662; 014670 khoảng x 0–290, y 1160–1700; 034080 khoảng x 0–200, y 1270–1700. Slice này không chứa frame ngoại lệ 006840 hay 271039.
- [x] Class sáu nhãn: xe ba bánh chở khách (auto-rickshaw vàng/đỏ) → `ThreeWheeler`, không gọi Truck/Bus. Xe tải thùng có chữ trên mui ở 001320 → `Truck`. Xe vàng nhiều cửa sổ bị cắt ở mép trái 014670 → `Bus`.
- [x] Rider và Bike: rút kinh nghiệm từ lỗi C0 (L6, R03), tôi nhìn điểm tựa hông/yên trước khi quyết định. 034080: người lái cùng người ngồi sau (áo cam) = **một** `Bike` (115,998)–(272,1292). Người áo trắng phía sau ngồi trên xe hai bánh (thấy biển số bị làm mờ dưới người) = `Bike` riêng, `occluded`. 001320: người lái scooter = một `Bike`. Người đi bộ cạnh xe đỏ = `Pedestrian`. Không có ai trong ô tô/xe ba bánh bị box riêng.
- [x] Geometry trên ảnh fisheye gốc: box bám mép nhìn thấy trên ảnh gốc, không nắn thẳng. Box ô tô xanh ở rìa phải 014670 kéo tới x=1080 vì xe bị khung cắt. Không có box nào phủ cả dãy xe; hai xe ở xa 034080 (260–300 và 335–375) là hai box riêng.
- [x] truncated và occluded: `truncated=true` chỉ cho vật chạm biên khung hình (ThreeWheeler vàng x=0 ở 001320, Bus x=0 và Car x=1080 ở 014670, Car x=0 ở 034080). `selfqc` không báo lệch so với hình học vòng kính. `occluded=true` cho vật bị vật khác che: ThreeWheeler đỏ (người đi bộ che), Truck (rider che), Bus (xe ba bánh vàng che), ThreeWheeler (522–560) sau người đi bộ, Bike áo trắng, Car trắng sau xe ba bánh, Car xanh sau người đứng.
- [x] Vật thiếu hoặc box trùng: không có hai box cho một vật. ThreeWheeler (522–560, 952–1010) và Pedestrian (535–562, 960–1042) ở 014670 chồng nhau nhưng là hai vật khác nhau (người đứng trước xe ba bánh). Ca nghi ngờ: hai xe nhỏ ở xa 034080 (260–300, 1055–1097) và (335–375, 1063–1105) cao ~42 px, rất mờ. Tôi gán `ThreeWheeler` theo dáng mui/đuôi, nhưng class chưa chắc; sẽ ghi vào findings nếu reference khác.
- [x] ignore_region có reason: 3 `ego_body` + 6 `lens_border`, mỗi polygon đúng một `reason`. Không box nào nằm ≥50% trong ignore; `selfqc` không báo lỗi này.
- [x] Tên task raw_fisheye và export CVAT 1.1: task `Day11 · ADASIND · B1-edge · raw_fisheye`, export task dataset **CVAT for images 1.1**, tắt Save images.

## Ghi chú trung thực về cách làm
- Nhãn được tạo trên CVAT local (2.76.1) qua REST API của chính CVAT, do trợ lý AI (Claude) thao tác theo quan sát ảnh phóng to, không phải bằng chuột trên giao diện. Luồng vẫn giữ nguyên: import prefill, sửa/giữ/thêm, export CVAT 1.1, `draft`, `selfqc`, sửa, export cuối, `lock`.
- Prefill frame 1: giữ Bike #1; sửa ThreeWheeler #2 (thu hẹp, thêm `occluded`) và ThreeWheeler #3 (xbr 88 → 75 vì phần x>75 là xe đỏ phía sau).
- Trong lúc chẩn đoán C0, tôi đã liệt kê box model đóng băng (`assets/model-yolo26m.xml`) của cả 4 frame, trước khi khoá slice này. Toàn bộ box B1-edge đã được quyết định từ quan sát ảnh trước bước đó, và tôi không đổi box nào theo model (ví dụ vẫn giữ `ThreeWheeler` cho hai xe xa ở 034080 dù model gọi là Car). Ghi lại để người chấm biết thứ tự thật.

## Fill ratio (K12)
- adasind_001320.jpg box 5 edge: 0.874
- adasind_014670.jpg box 3 edge: 0.812
- adasind_034080.jpg box 3 edge: 0.485
- adasind_034080.jpg box 7 center: 0.787
mean edge: 0.724 (n=3)
mean center: 0.787 (n=1)
