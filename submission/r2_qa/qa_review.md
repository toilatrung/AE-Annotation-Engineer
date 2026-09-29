# QA review · B1-edge

Mã khóa: 9534-59A5

**Hình thức: cold review chính bản khoá của mình.** Theo `team.json`, vòng đổi bài của nhóm (huy, manh, trung) giao cho tôi soát bản khoá của **manh (B4-edge)**. Tôi chưa nhận được file export đã khoá và mã khoá của manh, nên theo `docs/03-roles-rotation-vi.md` (chờ quá 5 phút → coi như solo) tôi cold review `submission/r2_qa/../r1_craft/annotations.xml` (mã 9534-59A5). Pha này chỉ dùng ảnh gốc, `qa_overlay.html` và `docs/02-rules-vi.md`. Tôi **chưa** mở reference `refs/slice-B1-edge.zip`, `model_compare` hay worked overlay của slice. Chỉ số `L#` theo `qa_overlay.html` (thứ tự box cao ≥40 px trong export khoá).

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_014670.jpg | L4 | R04 | Box `Bus` (0,832)–(75,1095) ở mép trái. Trên ảnh phóng to chỉ thấy thân vàng, một tấm lá chớp/ô cửa xám ở phần trên (0–60, 840–930) và một bánh xe (50–70, 1000–1060); phần đầu xe nằm ngoài khung. Dấu hiệu này hợp cả xe buýt học sinh lẫn xe tải có thùng che. Theo R04, class cần đối chiếu lại; chưa đủ thông tin để khẳng định Bus hay Truck. |
| adasind_034080.jpg | L8 | R04 | Box `ThreeWheeler` (260,1055)–(300,1097), cao 42 px, sát ngưỡng H=40, rất mờ ở xa. Chỉ thấy khối đuôi tối và mui sáng, không đếm được bánh. Class ThreeWheeler hay Car chưa kiểm được bằng mắt; cần người thứ hai xem ảnh gốc. |
| adasind_034080.jpg | L9 | R04 | Box `ThreeWheeler` (335,1063)–(375,1105), cùng tình trạng L8: 42 px, mờ, class chưa chắc. Nếu hai box này đổi class thì nên đổi cùng một quyết định, vì cùng kiểu vật và cùng khoảng cách. |
| adasind_034080.jpg | L6 | R03 | Box `Bike` (130,1015)–(185,1115) cho người áo trắng sau cặp rider áo cam. Chỉ thấy phần thân trên và một vùng biển số bị làm mờ ngay dưới người; bánh xe và yên bị L5 che. Nhãn "rider trên xe hai bánh" hợp lý theo R03, nhưng bằng chứng về chiếc xe gián tiếp (chỉ thấy biển số mờ). Giữ nhưng đánh dấu cần xác nhận. |
| adasind_001320.jpg | L2 | R02 | Box `ThreeWheeler` đỏ (93,805)–(184,940), `occluded=true` đúng vì người đi bộ L3 đứng trước. Mép trái 93 là nơi người đi bộ bắt đầu che; phần thân xe sau người có thể kéo tới ~x=85. R02 yêu cầu bám phần **nhìn thấy**, nên mép 93 chấp nhận được; ghi lại để thống nhất cách xử lý vật bị che ở mép. |
| adasind_034080.jpg | L3 | R02 | Box `Pedestrian` (995,1025)–(1077,1222) gồm cả túi trắng người này cầm bên trái (995–1015, 1100–1145). Rules v1.0.0 không nói rõ đồ mang theo có nằm trong box người hay không; box hiện rộng hơn thân người khoảng 10 px bên trái. |

## Mục đã soát và không thấy vi phạm
- R07/R08: mỗi frame có 2 `lens_border` và 1 `ego_body` bám tay/tay lái/đùi người lái ở góc trái-dưới. Slice không có frame ngoại lệ.
- R09: không box nào nằm ≥50% trong polygon ignore.
- R05: 4 box `truncated=true` đều chạm biên khung (x=0 hoặc x=1080).

Ghi finding r2_qa: cell=L_only, rule_id có giá trị, why để trống.
