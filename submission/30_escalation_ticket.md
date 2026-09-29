# Escalation ticket

## Ticket 1

- **Frame:** `adasind_014670.jpg` (slice B1-edge), vật: đuôi auto-rickshaw ở center, L6 ThreeWheeler (360,953)–(397,1000). Reference có box cùng vật (360,964)–(396,1001) nhưng cao 37 px < H=40 nên bị lọc; model M7 Truck (359,953)–(398,1002) cao 49 px. Dòng liên quan: `findings.csv` r1_craft/r3_diag 014670 L6 (`E2_guideline_gap`, `action=escalate`); decision log D3.
- **Ảnh chụp:** `submission/screenshots/b1edge_014670_L6_H40_borderline.png`
- **Expected impact:** vật đổi trong/ngoài phạm vi chỉ vì 7–10 px ở mép trên. Trong slice 3 frame này, ca này chiếm 1/1 spurious còn lại ở zone `center` sau rework (`rework/delta.md`). Nó cũng làm precision micro của `local_quality.md` thấp hơn thực chất. Nếu không có quy tắc dung sai, mọi báo cáo zone có vật xa/nhỏ sẽ lẫn nhiễu vẽ tay với lỗi thật, và annotator có thể bị yêu cầu "sửa" nhãn đúng.
- **Owner:** `guideline` (quyết định luật), phối hợp `qa` (người giữ teaching reference đo lại mép trên của R).
- **Recommendation:** (1) Áp dụng patch R01b ở `20_guideline_patch.md` (dải 35–45 px là don't-care, attribute `near_threshold`). (2) Người giữ reference đo lại vật này trên ảnh gốc phóng to. Theo crop 6× của tôi, khung đuôi tối bắt đầu y≈957, nên chiều cao ≥42 px; nếu đồng ý thì nâng mép trên box R. (3) Cho tới khi có quyết định, **giữ** box L và không tính ca này vào lỗi annotator.

## Ticket 2

- **Frame:** `adasind_014670.jpg`, vật xe vàng bị khung cắt ở mép trái, L4 Bus (0,832)–(75,1095), R5 Truck (0,845)–(70,1100), M3 Bus (0,825)–(81,1089); IoU L/R 0.872. Dòng liên quan: r1_craft 014670 L4+R5, r3_diag L4+M3 và R5 (`E2_guideline_gap`, `action=escalate`); QA r2_qa L4; decision log D4.
- **Ảnh chụp:** `submission/screenshots/b1edge_014670_L4_bus_vs_truck.png`
- **Expected impact:** phần phân biệt class (đầu/cabin, cửa khách) nằm ngoài khung. Ba nguồn cho hai đáp án (L, M: Bus; R: Truck). `local-quality` tính một FP Bus + một FN Truck, làm Bus/Truck thành "nhãn thấp nhất" (precision/recall 0.000) dù hình học đúng. Nếu không có luật, mỗi annotator tự chọn một kiểu; vật bị cắt ở mép sẽ nhiễu class nhiều nhất đúng ở vùng rìa fisheye.
- **Owner:** `guideline`.
- **Recommendation:** thêm vào R04 một quy tắc cho xe bị cắt: nếu phần thấy không đủ phân biệt Bus/Truck thì chọn theo dấu hiệu thấy được theo danh sách ưu tiên (cửa sổ khách dọc thân → Bus; thùng hàng/lá chớp thùng → Truck). Nếu vẫn mơ hồ, gán `ignore_region` `reason=unreadable` cho phần xe đó. Cần reference owner xem frame lân cận (nếu có trong ADASIND) trước khi chốt class cho ca này. Trong lúc chờ, giữ L4=Bus và ghi `keep_with_reason` sau khi có quyết định.
