# Escalation ticket

## Ticket 1

- **Frame:** `adasind_001320.jpg`, `adasind_012570.jpg`, `adasind_036720.jpg` (cả 3 frame của slice `B1-dense`) —
  hiện tượng lặp lại đúng 5 lần trong 3 frame.
- **Ảnh chụp:** `submission/screenshots/b1dense_012570_final_boxes.png`;
  chi tiết box tại `submission/r3_diag/model_compare.html`.
- **Expected impact:** Model đóng băng (YOLO26m) không có lớp `ThreeWheeler` — mọi xe ba bánh trong ảnh đều bị gán
  nhầm thành `Car` và/hoặc `Truck` (đôi khi xuất **cả hai** nhãn trùng khít cho cùng một box, ví dụ
  `adasind_012570.jpg` vị trí (224,906,323,1029) ra cả `M8 Car` và `M10 Truck`). Vì ADASIND (và nhiều cảnh đường
  phố Nam Á tương tự) có mật độ xe ba bánh cao, bất kỳ số liệu quality-control nào dùng model này làm baseline sẽ
  luôn bị lệch (tăng ảo `M thừa`, tăng ảo `M missing` do sai lớp) cho các slice nhiều `ThreeWheeler` — không phản
  ánh đúng chất lượng nhãn thật của annotator.
- **Owner:** `ai_team`
- **Recommendation:** Bổ sung lớp `ThreeWheeler` (fine-tune hoặc thêm post-processing ánh xạ hình dạng đặc trưng
  xe ba bánh) trước khi dùng model này làm gợi ý pre-label hoặc thước đo quality tự động cho các slice có nhiều xe
  ba bánh; nếu chưa kịp, gắn cờ cảnh báo "không tin cậy cho ThreeWheeler" khi hiển thị `model_compare.md`.

## Ticket 2

- **Frame:** `adasind_036720.jpg`, object `L4+R1` (`ThreeWheeler` rất lớn, sát ego, box (660,690)-(1080,1470),
  chiếm hơn 1/3 khung hình).
- **Ảnh chụp:** `submission/screenshots/b1dense_012570_final_boxes.png` (tham khảo bố cục tương tự);
  overlay đầy đủ ở `submission/r3_diag/model_compare.html`.
- **Expected impact:** Model bỏ sót hoàn toàn (không có box nào chồng lên vùng này) một vật thể rất lớn, rất gần
  ego và bị cắt bởi cả biên phải khung hình lẫn rìa ống kính. Nếu domain thật có nhiều tình huống xe/vật áp sát
  ego (đường hẹp, tắc đường), model sẽ có một điểm mù hệ thống đúng lúc vật gần nhất — rủi ro an toàn cao hơn hẳn
  so với lỗi ở vật xa.
- **Owner:** `ai_team`
- **Recommendation:** Bổ sung dữ liệu huấn luyện có vật thể cực gần/bị cắt nhiều bởi rìa ống kính (không chỉ méo
  hình học rìa mà còn tỷ lệ khung hình bất thường); ưu tiên kiểm thử riêng nhóm case "vật áp sát ego" trước khi
  triển khai.
