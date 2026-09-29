# Sensor context

- Rig: ADASIND không kèm tài liệu rig, ghi theo quan sát trên `adasind_001320.jpg` (frame trong slice
  `B1-dense`). Camera gắn trên xe hai bánh (xe máy), hướng ống kính theo chiều xe di chuyển tới — khung hình
  cho thấy mặt đường phía trước, các xe khác (xe ba bánh, xe máy) và người đi bộ cùng chiều, không phải camera
  lùi phía sau.
- `ego_body` nhìn thấy ở góc dưới-trái khung hình: một phần cẳng tay/bàn tay đang nắm tay lái và một phần ống
  quần/đùi của người lái xe (ego rider), tại vị trí khoảng x=0–220, y=1750–1920 trên ảnh 1080×1920. Đây là phần
  cơ thể của người điều khiển xe gắn camera, không phải đối tượng cần gán nhãn.
- Vòng kính (lens circle) chiếm gần trọn chiều ngang khung hình (đường kính ~1620 px trên ảnh rộng 1080 px,
  nghĩa là vòng kính bị cắt hai bên trái/phải) và lệch lên phía trên theo chiều dọc: tâm vòng khoảng y≈891 trên
  ảnh cao 1920 px, bán kính ~810 px, nên viền đen phía trên khung hình rất mỏng (~80 px) trong khi viền đen phía
  dưới dày hơn nhiều (~220 px, từ y≈1701 đến 1920). Hai polygon `ignore_region` (lens_border) trong prefill của
  slice khớp với ranh giới vòng kính này ở cả ba frame.
