# QA review · B1-dense

**Ghi chú quy trình:** nhóm 3 người (huy, manh, trung) đổi bài theo vòng A→B→C→A (`huy` soát `manh`, `manh` soát
`trung`, `trung` soát `huy` — xem `submission/00_setup/team.json`). Tại thời điểm làm bài P3, chưa nhận được file
đã khóa của `huy` sau hơn 5 phút chờ, nên theo `docs/03-roles-rotation-vi.md` coi như solo và chuyển sang **cold
review** chính bản khóa của mình trước (Phần 1). Sau đó `huy` đã chia sẻ repo cá nhân
(`https://github.com/Yuh5124/K4-L2-Day10-PhamQuangHuy-2A202602119-`), nên tôi làm thêm **peer review thật** đúng
vai trò P3 trên slice của huy (Phần 2) — cả hai đều chỉ dùng luật ở `docs/02-rules-vi.md`, chưa mở teaching
reference/model/worked overlay cho slice tương ứng.

## Phần 1 — Cold review (bản khóa của chính tôi, slice `B1-dense`, mã `E19E-0F0F`)

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_012570.jpg | L2+L3 (hai Pedestrian) | R05 | Hai người đi bộ đứng/đi sát nhau, vai trái của người bên phải (L3) chồng nhẹ lên vai người bên trái (L2) khoảng 10–15% diện tích ước lượng. Tôi đã đánh `occluded=false` cho cả hai vì thân người vẫn nhận diện được riêng biệt; cần người soát xác nhận có nên đổi L3 thành `occluded=true` không. |
| adasind_036720.jpg | L (ThreeWheeler lớn, sát ego) | R05, R07 | Box ThreeWheeler (660,690)-(1080,1470) rất gần polygon `ego_body` phía dưới (khoảng cách theo trục y ~90px) và tiếp giáp mép phải khung hình (`truncated=true`). Ranh giới giữa "thân xe ba bánh" và "thân xe/kính xe ego" khó phân định tuyệt đối ở vùng này; cần soát lại xem có phần nào của box đang lấn vào phần thân xe ego hay ngược lại. |
| adasind_012570.jpg | L (Car, mép trái) | R02 | Box Car (0,918)-(32,1000) chỉ rộng 32px — một mảnh rất hẹp của xe đỏ bị cắt bởi mép khung. Đã vẽ bám theo đúng phần nhìn thấy trên ảnh gốc (không đoán phần bị cắt), nhưng vì bề rộng box quá hẹp so với các box khác, cần người soát xác nhận không bỏ sót phần xe còn nhìn thấy được ở sát mép hơn nữa. |

Không có ca nào trong ba ca trên đổi nhãn chính (class) — chỉ là điểm cần xác nhận thêm về `occluded` và ranh giới
hình học, đúng tinh thần P3 "soát chỉ bằng luật, chưa biết reference".

## Phần 2 — Peer review thật (bản khóa của `huy`, slice `B1-edge`, mã `9534-59A5`)

Đã kiểm tính toàn vẹn file trước khi soát: `sha256` của `submission/r1_craft/annotations.xml` trong repo của huy
(`953459a5...5581`) khớp đúng với `sha256` ghi trong `submission/r1_craft/lock.txt` của huy (mã khóa `9534-59A5`)
— file chưa bị chỉnh sau khi khóa. Ba frame: `adasind_001320.jpg`, `adasind_014670.jpg`, `adasind_034080.jpg`.

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_034080.jpg | Bike (115,998,272,1292) + Bike (130,1015,185,1115) | R03 | **Phát hiện bằng luật, có kiểm chứng số liệu** (không suy đoán từ mắt): box `Bike` thứ hai `(130,1015,185,1115)` nằm **100% trong** box `Bike` thứ nhất `(115,998,272,1292)` (tính bằng diện tích chồng lấn / diện tích box nhỏ hơn = 1.0). Ảnh gốc cho thấy đây là **một** xe máy chở hai người (người lái + một người ngồi sau mặc áo/váy cam) — theo R03 "rider + xe hai bánh = một box Bike duy nhất", cả cụm bike+hai người chỉ cần **một** box. Box thứ hai (đánh `occluded=true`) nhiều khả năng là dư ra khi vẽ riêng phần người ngồi sau, vi phạm R03 và tạo `DUPLICATE`. Đề xuất: xóa box thứ hai, chỉ giữ box bao trọn cả cụm bike+hai người. |
| adasind_014670.jpg | Bus (0,832,75,1095) | R04 | Vật bị cắt gần hết bởi mép trái khung hình (chỉ ~75px rộng nhìn thấy), nhưng phần mui/thân xe còn lại (mái cong, cửa sổ xếp hàng) đủ đặc trưng để phân biệt xe buýt với xe tải — nhãn `Bus` hợp lý, không phải ca cần sửa; ghi lại để xác nhận đã kiểm kỹ chứ không đoán. |
| — (toàn bộ 3 frame) | R09 | Đã chạy kiểm tra tự động (không có box nào chiều cao < 40px, không có box nào nằm trong `ignore_region`, không có cặp cùng lớp IoU chồng lấn cao nào khác ngoài ca Bike ở trên) — phần còn lại của bộ nhãn đạt chuẩn theo luật. |

**Kết luận Phần 2:** 1 ca cần sửa thật sự (`Bike` trùng lặp ở `adasind_034080.jpg`, vi phạm R03), phần còn lại đạt
yêu cầu. Đã báo lại phát hiện này cho huy để anh tự quyết định rework trong repo của mình (repo của huy không
thuộc phạm vi sửa trực tiếp của tôi).
