# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh):
  1. Đoạn từ (533, 720) đến (405, 650) — vạch sơn chéo ở tiền cảnh bên trái-giữa, sát mép dưới khung hình, phân
     ranh giữa hai ô đỗ liền kề trong hàng gần camera nhất.
  2. Đoạn từ (699, 622) đến (960, 685) — vạch sơn chéo ở phía phải khung hình, thuộc hàng ô đỗ xa hơn (gần dãy
     ô đối diện xe màu đỏ), cùng hướng hội tụ về điểm biến mất với các vạch khác nên xác định được đây là ranh ô
     chứ không phải mép làn.
- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao: dải sơn màu vàng nhạt chạy dọc theo chân hàng rào ở hậu
  cảnh (khoảng x=350–580, y=451–466) không được gán `parking_line`. Dải này song song với hàng rào, không cắt
  ngang bất kỳ ô đỗ nào và không tạo ranh giới giữa hai ô — đây là vạch chỉ giới/curb-paint đánh dấu mép bãi hoặc
  lối tiếp cận, không phải vạch chia ô, nên không gán nhãn theo quy tắc ở `docs/11-parking-lines-vi.md`.
- Polygon `free_space` dừng ở đâu; có phần bị che nào không: đã vẽ 10 polygon `free_space` nhỏ, mỗi polygon phủ
  một mảng mặt đường trống trong một hàng lối xe chạy, trải từ y≈498 (gần đường chân trời/hàng rào) đến y=720
  (mép dưới khung hình), toàn chiều rộng ảnh. Các polygon dừng lại trước vị trí chiếc xe đỏ (khoảng x=190–230,
  y=454–479) — không polygon nào lấn vào vùng xe hay vùng cây/hàng rào phía sau đường chân trời (y<498), vì đó
  không phải mặt đường quan sát được.
- Ca chưa chắc cần hỏi người soát (nếu không có, ghi "không có"): các vạch chéo mảnh ở dải y≈498–520 (hàng ô xa
  nhất, gần đường chân trời) khá ngắn và mờ; không chắc từng đoạn là ranh ô đỗ riêng lẻ hay là điểm bắt đầu bị
  cắt của cùng một vạch dài hơn — cần người soát xác nhận trên ảnh gốc độ phân giải đầy đủ trước khi coi là hai
  ô tách biệt.
