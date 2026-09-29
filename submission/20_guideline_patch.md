# Guideline patch

- **Rule mới đề xuất (R12 — Một vật thể, một box):** Khi hai vật thể chồng lấn hoặc đứng/đậu sát nhau — dù cùng
  lớp (hai `ThreeWheeler` đậu liền nhau) hay khác lớp (người lái xe máy đứng ngay trước một xe hơi khuất một
  phần) — mỗi vật thể còn phân biệt được ranh giới riêng (dù mờ) phải có box riêng. Không gộp thành một box chỉ
  vì tại thời điểm vẽ nhanh chúng trông như một khối; dừng lại kiểm tra "đây có chắc là MỘT vật thể vật lý không"
  trước khi chốt box ở mọi cụm vật chồng lấn.
- **Áp dụng cho:** box của cả 6 class động (`Bus, Bike, Car, Pedestrian, Truck, ThreeWheeler`), đặc biệt ở zone
  `center` nơi vật thường đứng gần nhau nhất theo góc nhìn camera.
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** R03 chỉ quy định trường hợp cụ thể "rider + xe hai
  bánh = một box" (gộp có chủ đích, ngoại lệ). Không có rule nào nói rõ hướng ngược lại — khi nào PHẢI tách hai
  vật chồng lấn thành hai box. Tôi đã mắc đúng lỗi này hai lần độc lập trong buổi: vòng `calib` (gộp hai
  `ThreeWheeler` đậu sát nhau thành một box, xem `findings.csv` dòng `calib/L2/R5/R6`) và vòng `r1_craft`
  (`adasind_012570.jpg`, gộp một `Bike` và một `Car` khuất phía sau thành một box, xem dòng `L6/R7+M9/R8+M6`) —
  cả hai lần đều được cả reference lẫn model độc lập xác nhận là hai vật thể riêng biệt. Đây không phải trùng hợp
  ngẫu nhiên mà là một xu hướng lặp lại cần luật rõ ràng để nhắc kiểm tra.
- **`rules_version` mới:** v1.0.0 → v1.1.0
- **Hiệu lực từ:** áp dụng ngay từ vòng `rework` của tôi (đã tách box theo rule này trong
  `submission/rework/annotations-v2.xml`); đề xuất áp dụng cho mọi vòng tiếp theo của lớp.
