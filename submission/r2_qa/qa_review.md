# QA review · B1-dense (bản khóa của trung, mã khóa E19E-0F0F)

**Ghi chú quy trình:** Tôi (manh) soát slice `B1-dense` của `trung` theo vòng A→B→C→A (`manh` soát `trung` — xem
`submission/00_setup/team.json`). Đã kiểm sha256 file `r1_craft/annotations.xml` của trung khớp đúng
`e19e0f0ff688bde6bae51d21bfe7e3898b01b4f6827b4a462047422fb465581f` = mã khóa `E19E-0F0F` (file toàn vẹn, chưa bị
sửa sau khi khóa). Soát chỉ bằng luật ở `docs/02-rules-vi.md`, chưa mở teaching reference/model/worked overlay
cho slice này.

| frame | object_ref | rule_id | nhận xét |
|---|---|---|---|
| adasind_012570.jpg | Bike (326,918,381,997) | R03 | Trong khung box `Bike` này, nhìn trên ảnh gốc thấy một phần màu trắng (có thể là một xe hơi/van) ở phía sau/cạnh người lái xe máy, không hoàn toàn là thân xe máy. R03 chỉ quy định "rider + xe hai bánh = một box", không bao gồm vật khác đứng/đậu cạnh đó. Đề nghị trung kiểm lại xem có cần tách một box `Car` riêng cho phần màu trắng phía sau không, trước khi coi đây là một box hoàn chỉnh. |
| adasind_012570.jpg | Car (0,918,32,1000) | R02 | Box rộng chỉ 32px — một mảnh rất hẹp của xe đỏ bị cắt bởi mép khung trái. Đã bám đúng phần nhìn thấy trên ảnh gốc (đúng R02), nhưng biên rộng hẹp này nên được trung xác nhận lại là đã lấy đủ phần còn thấy được, không thiếu. |
| adasind_036720.jpg | ThreeWheeler (660,690,1080,1470) | R05, R07 | Box rất lớn, sát polygon `ego_body` phía dưới và chạm biên phải khung hình (`truncated=true` — đúng, vì chạm x=1080). Khoảng cách giữa cạnh dưới box (y=1470) và polygon `ego_body` gần nhất (~y=1560–1650) là ~90–180px, không chồng lấn — ranh giới giữa "thân xe ba bánh" và "thân xe ego" ở đây là rõ ràng, không phải lỗi. Ghi nhận đã kiểm, không phải finding. |

**Kết luận:** 1 điểm cần trung tự xác nhận lại (Bike/Car ở `adasind_012570.jpg`, theo R03) — đây là ca khó vì hai
vật gần/chồng nhau, chỉ soát bằng luật (chưa mở reference) nên chỉ nêu được là "cần kiểm tra thêm", không khẳng
định đúng/sai. Hai điểm còn lại đã kiểm và đạt yêu cầu.
