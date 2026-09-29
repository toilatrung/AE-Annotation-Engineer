# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `e0646890f21a497d370ca4249b990b4dd0c2139d6d68eb07e412b5cde4fa52b5`; slice `B4-edge`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_236370.jpg, adasind_258420.jpg, adasind_310008.jpg. Frame thiếu trong export: không.
TP=17; FP=3; FN=3; số lần đối chiếu=23; mean IoU của TP=0.828.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.739 | 0.948 | 0.870 |
| precision | 0.850 | 0.910 | 0.750 |
| recall | 0.850 | 0.828 | 0.500 |
| jaccard | 0.739 | 0.765 | 0.500 |
| dice | 0.850 | 0.852 | 0.667 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 3 | 1 | 1 | 0.913 | 0.750 | 0.750 | 0.600 | 0.750 |
| Car | 1 | 0 | 1 | 0.957 | 1.000 | 0.500 | 0.500 | 0.667 |
| Pedestrian | 8 | 2 | 1 | 0.870 | 0.800 | 0.889 | 0.727 | 0.842 |
| ThreeWheeler | 4 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| Truck | 1 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_236370.jpg | 7 | 2 | 0 | 0.778 | 0.778 | 1.000 |
| adasind_258420.jpg | 5 | 1 | 3 | 0.556 | 0.833 | 0.625 |
| adasind_310008.jpg | 5 | 0 | 0 | 1.000 | 1.000 | 1.000 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Car | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|---:|
| Bike | 3 | 0 | 0 | 0 | 0 | 1 |
| Car | 0 | 1 | 0 | 0 | 0 | 1 |
| Pedestrian | 0 | 0 | 8 | 0 | 0 | 1 |
| ThreeWheeler | 0 | 0 | 0 | 4 | 0 | 0 |
| Truck | 0 | 0 | 0 | 0 | 1 | 0 |
| <extra> | 1 | 0 | 2 | 0 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
