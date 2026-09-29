# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Xe/người cắt ngang tại giao lộ giờ cao điểm; cảnh ngược sáng (bình minh/hoàng hôn) | Ngược sáng làm mất contour vật nhỏ/xa; vật cắt ngang tốc độ cao gây motion blur — rủi ro va chạm trực diện cao nhất trong 4 hướng | Giữ đúng tiêu cự/lens center calibration riêng của front cam; không dùng chung tham số với side cam | So khớp thời điểm với camera trái/phải ở vùng seam hai bên trước khi chốt gold; ít nhất 2 người soát độc lập cho ca cắt ngang |
| rear | Vật trong điểm mù rất gần đuôi xe khi lùi; xe máy áp sát | `ego_body` (cản sau/biển số) chiếm phần lớn khung hình, dễ nhầm ranh giới ego với vật thật ở khoảng cách rất gần | Polygon `ego_body` phải cập nhật đúng hình dạng cản sau thật (khác hình dạng tay lái/gương của front) | Đối chiếu với video lùi thực tế (không chỉ ảnh tĩnh) hoặc cảm biến khoảng cách nếu có, vì ảnh tĩnh dễ đánh giá sai khoảng cách vật rất gần |
| left | Người đi bộ/xe máy đi song song sát hông xe, đúng góc seam với front và rear | Zone `edge` bị méo mạnh nhất trong 4 camera vì nằm ở "khe" giữa hai hướng nhìn chính; dễ trùng lặp hoặc bỏ sót vật ở góc chồng lấn (seam) với camera kế bên | Left và camera kế cận (front, rear) phải dùng chung một mốc thời gian và calibration extrinsic đã đăng ký trước khi so khớp seam | Ít nhất 2 người soát độc lập cho mỗi cặp seam (left-front, left-rear), ghi rõ box nào là "chủ" (primary) để tránh đếm trùng một vật thành hai ca |
| right | Tương tự left (đối xứng): người/xe sát hông phải, seam với front và rear | Cùng lý do với left — méo rìa mạnh, rủi ro seam hai đầu | Right và camera kế cận (front, rear) cùng mốc thời gian/calibration extrinsic | Cùng quy trình 2 người soát độc lập cho cặp seam (right-front, right-rear) |

- **Khi nào cần refresh gold set:** khi đổi phần cứng/vị trí lắp camera (tiêu cự, góc lắp), khi calibration lens
  thay đổi (sau bảo dưỡng/thay camera), hoặc khi rule/taxonomy đổi (`rules_version` tăng) khiến nhãn cũ không còn
  khớp định nghĩa mới (ví dụ patch R12 ở `20_guideline_patch.md` làm một số box cũ cần tách lại).
- **Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box:** người đi bộ đứng đúng góc giao giữa
  camera trái và camera trước, xuất hiện ở rìa của cả hai ảnh với hai box khác nhau. Trước khi ghép thành một
  track/ID cần: (1) timestamp đồng bộ giữa hai stream, (2) calibration extrinsic đã hiệu chỉnh để chiếu về cùng
  một điểm trong không gian (BEV), và (3) policy rõ ràng về output đích (hệ thống cần một ID toàn cục hay chấp
  nhận output riêng từng camera). Thiếu một trong ba điều kiện thì giữ hai box riêng và escalate, không tự ghép.
- **Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn
  camera:** mỗi camera có hình học méo, ánh sáng và bối cảnh điểm mù khác nhau (ví dụ front ít méo/nhiều sáng,
  rear bị `ego_body` che nhiều, left/right méo rìa mạnh nhất) — độ đồng thuận cao ở front không suy ra được độ
  tin cậy tương tự ở ba camera còn lại. Bằng chứng từ báo cáo `local-quality`/`model` của bài lab này (một camera
  ADASIND) cho thấy ngay cả trong MỘT camera, lỗi đã khác nhau rõ rệt theo zone (`center` vs `edge`) — khác camera
  còn khác biệt hơn nữa, nên gold set phải được đánh giá và refresh riêng biệt cho từng camera, không gộp chung
  một con số chất lượng duy nhất.
