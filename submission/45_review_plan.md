# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| `adasind_012570.jpg` (slice `B1-dense`) | 16 dòng `r3_diag` liên quan đến frame này: 1 ca `E1_annotator_error` đã rework (`L6` gộp Bike+Car) + 2 ca `BOX_GEOMETRY` nhỏ (P2) + 4 ca `E4_model_domain` (model gán nhầm `Car`/`Truck` cho `ThreeWheeler`, đôi khi trùng lặp cả hai nhãn) | Frame có mật độ vật thể cao nhất (8 box) và cụm xe/người chồng lấn nhiều nhất trong 3 frame — nơi tôi thực sự mắc lỗi gộp box (không phải giả định), có bằng chứng rework đo được cải thiện (`rework/delta.md`: center matched 6→8) | `findings.csv` (object_ref `L6`, `L8+M12`, `R7+M9`, `R8+M6`, `R9+M13`); `screenshots/l6_merge_before_after.png`; `r3_diag/model_compare.html` |
| `adasind_036720.jpg` (slice `B1-dense`) | 1 ca `LR_noM MISSING` (`L4+R1`): model bỏ sót hoàn toàn một `ThreeWheeler` chiếm >1/3 khung hình, rất gần và bị cắt bởi cả rìa ống kính lẫn biên phải | Đại diện cho nhóm "vật cực gần ego, bị cắt nhiều" — khác hẳn giả thuyết méo-rìa-thông-thường; model có điểm mù đúng lúc vật gần nhất, rủi ro an toàn cao hơn lỗi ở vật xa | `findings.csv` object_ref `L4+R1`; `submission/30_escalation_ticket.md` Ticket 2; `r3_diag/model_compare.html` |

**Giới hạn của kết luận từ ba frame ADASIND:** một slice 3 frame, 19 vật in-scope (chỉ 3 vật ở zone `edge`) là mẫu
quá nhỏ để suy ra tỷ lệ lỗi tổng thể của cả tập dữ liệu hay khẳng định chắc chắn nguyên nhân gốc (`E4_model_domain`
cho zone `edge` chỉ dựa trên 3 mẫu — xem cảnh báo ở `r3_diag/zone_table.md`). Mẫu lặp lại 5 lần của lỗi
`ThreeWheeler→Car/Truck` là bằng chứng đủ mạnh để escalate, nhưng các giả thuyết khác (ví dụ model gãy ở edge vì
méo hình) vẫn cần thêm dữ liệu để xác nhận.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv`: rải mẫu theo thời gian/tuyến đường/điều kiện sáng khác
nhau trong mỗi camera, tránh lấy nhiều frame liên tiếp từ cùng một đoạn video ngắn — hai frame cách nhau vài trăm
mili-giây của cùng một tình huống (ví dụ cùng một xe đang đi qua) thực chất là **một** ca lặp lại, không phải hai
ca độc lập; nếu đếm cả hai vào "200 ca đã review" sẽ phóng đại độ phủ thật. Kế hoạch 200 frame (phân theo
`normal`/`hard` × 4 camera) chỉ giúp **tìm ra ca cần soi kỹ** theo phân tầng rủi ro đã biết trước (seam, điểm mù,
backlight...); nó **chưa đo được tỷ lệ lỗi** của toàn bộ 50.000 frame vì đây là mẫu phân tầng có chủ đích
(stratified, ưu tiên ca khó), không phải mẫu ngẫu nhiên đơn thuần — muốn ước lượng tỷ lệ lỗi tổng thể cần lấy mẫu
ngẫu nhiên (không phân tầng) riêng, tách bạch với mục tiêu "tìm ca khó" của 200 frame này.
