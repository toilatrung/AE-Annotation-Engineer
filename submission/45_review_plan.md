# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| Vật nhỏ/xa ở zone `center`, cao 35–47 px (B1-edge: `adasind_014670.jpg` L6, L2; `adasind_034080.jpg` L9, L10) | L ở center: 4 spurious + 1 missing trên n_ref=7 (`zone_table.md`). Trong đó 2 ca tranh chấp ngưỡng H=40 (E2/E1), 1 ca đọc quá mức vật bị che (E1), 1 ca box lệch ~20 px (E1). Sau rework còn 1 spurious (ca H=40 đang escalate). | Đây là nơi **nhãn người** gãy nhiều nhất, và nguyên nhân chủ yếu là luật/đo đạc chứ không phải méo fisheye. Sửa luật R01b sẽ giảm nhiễu cho mọi slice. | Crop ≥4× kèm lưới toạ độ, số đo chiều cao L/R/M, `screenshots/b1edge_014670_L6_H40_borderline.png`, `rework/delta.md`. |
| Xe ba bánh và rider ở zone `mid` (`adasind_001320.jpg`, `adasind_014670.jpg`, `adasind_034080.jpg`) | M ở mid: 6 missing + 7 thừa trên n_ref=9. 10 dòng `E4_model_domain`: ThreeWheeler → Truck/Car (5 box) và rider tách Pedestrian + Bike (5 box). | Nếu dùng model này làm pre-label, annotator phải sửa class ở gần như mọi xe ba bánh/xe máy chở người. Cần kiểm trước khi tin số pre-label. Lỗi class chứ không phải hình học (`iou_sweep.md`: M mid matched 4→3 từ IoU 0.3 → 0.7). | `r3_diag/model_compare.md`, `model_compare.html`, `iou_sweep.md`, các dòng r3_diag M3/M5–M12. |

Giới hạn của kết luận từ ba frame ADASIND: chỉ 3 frame (20 box reference) từ **một** camera fisheye hướng trước, gắn trên xe hai bánh, cùng một khu phố và điều kiện nắng ban ngày. Mỗi zone chỉ 4–9 vật nên một ca đổi là đổi 10–25% số của zone. Teaching reference có ca còn tranh chấp (014670 L6 ngưỡng H, L4 Bus/Truck). Vì vậy đây là **tín hiệu chọn chỗ soi**, không phải tỷ lệ lỗi và không suy ra cho camera rear/left/right của hệ SVM.

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv` (kể cả tránh đếm nhiều frame liền nhau trong cùng cảnh
như nhiều ca độc lập), và vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi:

1. **Phân tầng trước, rồi mới lấy mẫu:** chia 50.000 frame theo `camera_id` × `normal/hard`. Nhãn hard lấy từ metadata/tín hiệu có sẵn: tốc độ thấp/lùi xe, đêm/ngược sáng, số VRU theo pre-label, vật trong vùng seam, vật cao 35–45 px. Sau đó lấy ngẫu nhiên trong từng ô theo số ở CSV (8 ô, tổng 200).
2. **Chống trùng cảnh:** gom frame theo `drive_id` + cửa sổ thời gian 10 giây. Mỗi cửa sổ tối đa 1 frame cho mỗi camera, và không quá 5% mỗi ô đến từ cùng một chuyến. Frame từ bốn camera cùng một timestamp được đánh dấu là **một sự kiện**, để khi đếm "ca seam" không nhân bốn.
3. **Soát độ phủ sau khi chọn:** lập bảng đếm theo camera × điều kiện (ngày/đêm, mưa, đô thị/ngoại ô) × loại vật (6 class, riêng ThreeWheeler và rider có người ngồi sau) × zone bán kính. Ô nào < 3 mẫu thì lấy bổ sung có chủ đích và ghi rõ là "chủ đích", không tính vào ước lượng.
4. **Vì sao chưa đo được tỷ lệ lỗi:** các ô hard được lấy dư có chủ đích (116/200 = 58% hard, trong khi hard trong 50.000 frame có thể chỉ vài %), nên tỷ lệ lỗi thô trên 200 frame bị lệch về phía ca khó. Muốn ước lượng tỷ lệ cho cả tập phải cân lại theo trọng số tầng hoặc lấy thêm một mẫu ngẫu nhiên đơn giản. Ngoài ra 200 frame chia cho 8 ô chỉ còn 20–30 frame mỗi ô, khoảng tin cậy rất rộng. Kế hoạch này dùng để **tìm và phân loại ca cần soi**, không để công bố chất lượng.
