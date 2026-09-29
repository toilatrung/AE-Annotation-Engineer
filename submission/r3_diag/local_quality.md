# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `e19e0f0ff688bde6bae51d21bfe7e3898b01b4f6827b4a462047422fb465581f`; slice `B1-dense`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_001320.jpg, adasind_012570.jpg, adasind_036720.jpg. Frame thiếu trong export: không.
TP=16; FP=2; FN=3; số lần đối chiếu=20; mean IoU của TP=0.834.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.800 | 0.950 | 0.800 |
| precision | 0.889 | 0.920 | 0.600 |
| recall | 0.842 | 0.853 | 0.600 |
| jaccard | 0.762 | 0.819 | 0.429 |
| dice | 0.865 | 0.880 | 0.600 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 3 | 2 | 2 | 0.800 | 0.600 | 0.600 | 0.429 | 0.600 |
| Car | 2 | 0 | 1 | 0.950 | 1.000 | 0.667 | 0.667 | 0.800 |
| Pedestrian | 3 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| ThreeWheeler | 7 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| Truck | 1 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_001320.jpg | 6 | 0 | 0 | 1.000 | 1.000 | 1.000 |
| adasind_012570.jpg | 6 | 2 | 3 | 0.600 | 0.750 | 0.667 |
| adasind_036720.jpg | 4 | 0 | 0 | 1.000 | 1.000 | 1.000 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Car | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|---:|
| Bike | 3 | 0 | 0 | 0 | 0 | 2 |
| Car | 1 | 2 | 0 | 0 | 0 | 0 |
| Pedestrian | 0 | 0 | 3 | 0 | 0 | 0 |
| ThreeWheeler | 0 | 0 | 0 | 7 | 0 | 0 |
| Truck | 0 | 0 | 0 | 0 | 1 | 0 |
| <extra> | 1 | 0 | 0 | 0 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
