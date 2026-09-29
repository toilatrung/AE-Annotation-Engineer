# Guideline patch

- **Rule mới đề xuất:** **R01b — Dải sát ngưỡng H (35–45 px).**
  - Đo chiều cao từ **mép trên của phần vật nhìn thấy** (mui/nóc/đầu người, kể cả mui vải sáng màu) tới điểm tiếp đất hoặc mép dưới nhìn thấy, trên ảnh gốc phóng to ≥4×.
  - Nếu phép đo rơi vào 35–45 px, người gán nhãn vẫn vẽ box theo đúng phần thấy và bật attribute mới `near_threshold=true`.
  - Khi so với reference, box có `near_threshold=true` ở **một trong hai phía** được tính **don't-care**: không TP, không FP, không FN, giống cách R09 xử lý ignore.
  - Ví dụ ca thật: `adasind_014670.jpg`, đuôi auto-rickshaw (360–397, 953–1000). Tôi đo ~43–47 px (L cao 47, model 49), reference vẽ (360,964)–(396,1001) cao 37 px. Chỉ lệch ~10 px ở mép trên mà vật đổi từ "L thừa" sang "đúng". Ví dụ thứ hai: `adasind_034080.jpg` vật ở xa (335–376, 1063–1105): R 35 px, M 35/39 px, L cũ 42 px.
  - Ví dụ phụ (R02b, đồ mang theo): túi/đồ người cầm sát thân (034080, người phải, túi trắng 995–1015) **được** nằm trong box `Pedestrian`. Đồ đặt xuống đất thì không.
- **Áp dụng cho:** R01 (phạm vi H=40), mọi class động; thêm attribute `near_threshold` (checkbox) cho 6 class trong `assets/labels.json`. Cách tính trong `compare`/`local-quality` cần coi box `near_threshold` là don't-care. Không ảnh hưởng `ignore_region`.
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** R01 chỉ nói "vật cao ≥ 40 px phải có box; thấp hơn không box". Luật không nói đo từ đâu (mui xe mờ có tính không), không có dung sai, trong khi hai người vẽ tay trên vật xa, mờ lệch nhau 5–10 px là bình thường. Hệ quả trong slice B1-edge: 2/5 lỗi L ở zone `center` (`r3_diag/zone_table.md`) và 1 dòng escalate là tranh chấp ngưỡng chứ không phải sai nhãn. Số đo chất lượng vì vậy phản ánh nhiễu vẽ tay nhiều hơn lỗi thật.
- **`rules_version` mới:** v1.0.0 → **v1.1.0**.
- **Hiệu lực từ:** round `rework` tiếp theo sau khi Lab Coach/guideline owner duyệt; bắt buộc cho mọi slice từ lượt gán nhãn kế tiếp. Các lock hiện tại (r1_craft `9534-59A5`, rework `A2E1-0B86`) giữ nguyên, không khoá lại hồi tố.
