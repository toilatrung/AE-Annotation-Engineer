# So sánh L với R

Chỉ số L/R là thứ tự box cao ≥ H=40 trong từng frame, theo thứ tự XML; bắt đầu từ 1.
Box L trong ignore_region được báo IGNORE_SCOPE, không tính SPURIOUS.

## adasind_236370.jpg
- L2+R5 mid WRONG_CLASS
- R3 edge MISSING
## adasind_258420.jpg
- L6 mid SPURIOUS
- R3 mid MISSING
- R5 mid MISSING
- R7 mid MISSING
## adasind_310008.jpg

## Theo zone
| zone | n_ref | matched | missing | spurious |
|---|---|---|---|---|
| center | 4 | 4 | 0 | 0 |
| mid | 9 | 5 | 4 | 2 |
| edge | 7 | 6 | 1 | 0 |
