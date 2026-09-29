# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_001320.jpg
## adasind_014670.jpg
- L2 center SPURIOUS
- L4+R5 mid WRONG_CLASS
- L6 center SPURIOUS
## adasind_034080.jpg
- L9 center SPURIOUS
- L10+R2 center BOX_GEOMETRY

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 7 | 6 | 1 | 4 |
| mid | 9 | 8 | 1 | 1 |
| edge | 4 | 4 | 0 | 0 |
