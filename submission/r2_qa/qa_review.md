# QA review · B1-dense

Mã khóa: E19E-0F0F

**Ghi chú quy trình:** nhóm 3 người (huy, manh, trung) đổi bài theo vòng A→B→C→A (`huy` soát `manh`, `manh` soát
`trung`, `trung` soát `huy` — xem `submission/00_setup/team.json`). Tại thời điểm làm bài, chưa nhận được file đã
khóa của `huy` (repo riêng, chưa trao đổi export) sau hơn 5 phút chờ, nên theo `docs/03-roles-rotation-vi.md` coi
như solo và chuyển sang **cold review** chính bản khóa `r1_craft` của mình (slice `B1-dense`), chỉ dùng luật ở
`docs/02-rules-vi.md`, chưa mở teaching reference/model/worked overlay.

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_012570.jpg | L2+L3 (hai Pedestrian) | R05 | Hai người đi bộ đứng/đi sát nhau, vai trái của người bên phải (L3) chồng nhẹ lên vai người bên trái (L2) khoảng 10–15% diện tích ước lượng. Tôi đã đánh `occluded=false` cho cả hai vì thân người vẫn nhận diện được riêng biệt; cần người soát xác nhận có nên đổi L3 thành `occluded=true` không. |
| adasind_036720.jpg | L (ThreeWheeler lớn, sát ego) | R05, R07 | Box ThreeWheeler (660,690)-(1080,1470) rất gần polygon `ego_body` phía dưới (khoảng cách theo trục y ~90px) và tiếp giáp mép phải khung hình (`truncated=true`). Ranh giới giữa "thân xe ba bánh" và "thân xe/kính xe ego" khó phân định tuyệt đối ở vùng này; cần soát lại xem có phần nào của box đang lấn vào phần thân xe ego hay ngược lại. |
| adasind_012570.jpg | L (Car, mép trái) | R02 | Box Car (0,918)-(32,1000) chỉ rộng 32px — một mảnh rất hẹp của xe đỏ bị cắt bởi mép khung. Đã vẽ bám theo đúng phần nhìn thấy trên ảnh gốc (không đoán phần bị cắt), nhưng vì bề rộng box quá hẹp so với các box khác, cần người soát xác nhận không bỏ sót phần xe còn nhìn thấy được ở sát mép hơn nữa. |

Không có ca nào trong ba ca trên đổi nhãn chính (class) — chỉ là điểm cần xác nhận thêm về `occluded` và ranh giới
hình học, đúng tinh thần P3 "soát chỉ bằng luật, chưa biết reference".
