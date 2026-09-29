# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `953459a52a450ec4f6502c411e00becd9ac145aa6791618121173523af54bfe0`; slice `B1-edge`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_001320.jpg, adasind_014670.jpg, adasind_034080.jpg. Frame thiếu trong export: không.
TP=18; FP=5; FN=2; số lần đối chiếu=24; mean IoU của TP=0.872.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.750 | 0.951 | 0.875 |
| precision | 0.783 | 0.736 | 0.000 |
| recall | 0.900 | 0.708 | 0.000 |
| jaccard | 0.720 | 0.628 | 0.000 |
| dice | 0.837 | 0.703 | 0.000 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 3 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| Bus | 0 | 1 | 0 | 0.958 | 0.000 | 0.000 | 0.000 | 0.000 |
| Car | 3 | 1 | 1 | 0.917 | 0.750 | 0.750 | 0.600 | 0.750 |
| Pedestrian | 5 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| ThreeWheeler | 6 | 3 | 0 | 0.875 | 0.667 | 1.000 | 0.667 | 0.800 |
| Truck | 1 | 0 | 1 | 0.958 | 1.000 | 0.500 | 0.500 | 0.667 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_001320.jpg | 6 | 0 | 0 | 1.000 | 1.000 | 1.000 |
| adasind_014670.jpg | 4 | 3 | 1 | 0.571 | 0.571 | 0.800 |
| adasind_034080.jpg | 8 | 2 | 1 | 0.727 | 0.800 | 0.889 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Bus | Car | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|---:|---:|
| Bike | 3 | 0 | 0 | 0 | 0 | 0 | 0 |
| Bus | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| Car | 0 | 0 | 3 | 0 | 0 | 0 | 1 |
| Pedestrian | 0 | 0 | 0 | 5 | 0 | 0 | 0 |
| ThreeWheeler | 0 | 0 | 0 | 0 | 6 | 0 | 0 |
| Truck | 0 | 1 | 0 | 0 | 0 | 1 | 0 |
| <extra> | 0 | 0 | 1 | 0 | 3 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
