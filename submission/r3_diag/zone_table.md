# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 9 | 3 | 2 | 3 | 7 | WRONG_CLASS (1) |
| mid | 7 | 0 | 0 | 3 | 1 | — |
| edge | 3 | 0 | 0 | 2 | 4 | — |

## Nhận xét

- **L (tôi) gãy chủ yếu ở zone `center`**: 3 missing + 2 spurious, đều từ một ca duy nhất ở `adasind_012570.jpg`
  (L6+R7 WRONG_CLASS, L8+R9 BOX_GEOMETRY, R8 MISSING — xem `findings.csv`). Zone `mid` và `edge` của tôi có 0
  missing/spurious. Ở zone `mid`/`edge`, **M (model) lại là nguồn lỗi chính**: `M thừa` = 1 (mid) và 4 (edge) trên
  n_ref chỉ 7 và 3 — tỷ lệ thừa cao hơn hẳn L dù mẫu rất nhỏ.
- **Giả thuyết cho zone `center`**: lỗi của tôi không phải do méo fisheye (vị trí center ít méo nhất) mà do một
  cụm vật thể chồng lấn — người lái xe máy đứng ngay trước một xe hơi trắng khuất một phần (xem ảnh phóng to
  trong `findings.csv` dòng L6+R7). Tôi vẽ một box `Bike` rộng trùm cả phần xe phía sau thay vì tách hai box. Đây
  là lỗi loại **E1_annotator_error** lặp lại (cùng dạng với ca gộp hai xe ba bánh ở vòng `calib`), không phải lỗi
  hình học ngẫu nhiên — nên đề xuất patch luật ở `20_guideline_patch.md`.
- **Giả thuyết cho `M thừa` cao ở `center`** (7/9, chủ yếu tại `adasind_012570.jpg` — 4 dòng `M_only (center)`):
  ảnh này có một cụm xe hơi/van trắng đậu san sát ở hậu cảnh gần đường chân trời; model có thể đang phát hiện
  từng xe trong cụm đó dù kích thước dưới ngưỡng `H=40` mà lớp học quy ước, hoặc dưới `ignore`/ngoài phạm vi gán
  nhãn của bài. Đây **không hẳn là model sai** mà nhiều khả năng là lệch quy ước phạm vi (**E2_guideline_gap**):
  model không biết luật `H=40` của lớp, nên không thể kết luận là lỗi miền dữ liệu (`E4_model_domain`) chỉ từ một
  frame.
- **Giả thuyết `E4_model_domain` cho zone `edge`**: `M thừa`=4 và `M missing`=2 trên chỉ 3 vật edge — tỷ lệ lỗi
  rất cao, phù hợp với giả thuyết model gốc (huấn luyện trên ảnh phẳng) gãy ở vùng méo cạnh rìa fisheye. Tuy
  nhiên **n_ref=3 quá nhỏ để kết luận** — cần nhiều slice/frame hơn (ngoài phạm vi 3 frame của bài này) mới đủ
  bằng chứng; ghi nhận đây là giả thuyết cần kiểm thêm, không phải kết luận (`E5_unresolved` ở mức tổng thể).
- **Giới hạn của slice 3 frame**: một slice chỉ có 3 ảnh (19 vật in-scope tổng cộng, riêng edge chỉ 3 vật) không
  đủ để tách bạch giữa "model kém ở rìa vì méo hình học" và "model kém ở rìa vì ít dữ liệu huấn luyện có rìa
  fisheye" — cả hai giả thuyết đều hợp lý và cần tập dữ liệu lớn hơn để phân xử.
