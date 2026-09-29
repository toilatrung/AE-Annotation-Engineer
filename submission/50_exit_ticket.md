# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy
   tắc riêng? Vì sao?

   **Không phải `DUPLICATE`.** Trong taxonomy của bài (`docs/05-taxonomy-vi.md`), `DUPLICATE` mô tả việc MỘT
   annotator vẽ hai box cho một vật trên CÙNG một ảnh — lỗi thao tác. Hai box trên hai camera KHÁC NHAU là hai
   phép chiếu 2D độc lập, hợp lệ và đúng đắn của cùng một vật thể 3D; mỗi camera phải tự nhất quán trong phạm vi
   của chính nó, không được phép "biết trước" có camera kia để tự xoá bớt một box — làm vậy sẽ phá vỡ tính độc
   lập của quy trình gán nhãn từng camera. Cần một quy tắc RIÊNG (kiểu "cross-camera correspondence"), không tái
   dùng `DUPLICATE`, vì việc ghép hai box này thành một vật cần thêm dữ liệu mà một camera đơn không có: timestamp
   đồng bộ và calibration extrinsic giữa hai camera (xem `46_gold_set_plan.md`). Gọi nhầm là `DUPLICATE` sẽ khiến
   người sửa xoá một box hợp lệ chỉ vì thiếu bằng chứng ghép cặp.

2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái
   Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.

   Trên **cùng một camera**: giữ nguyên track ID khi vật còn quan sát được và định danh không đổi (cùng người,
   cùng xe) dù hình dạng/vị trí đổi dần; thêm **keyframe** khi có thay đổi hình học lớn giữa hai frame liên tiếp
   (vật rẽ hướng, phóng to nhanh khi tới gần, đổi từ `mid` sang `edge` zone) để track không bị nội suy sai; chuyển
   trạng thái **Outside** khi vật rời khỏi trường nhìn thật (ra khỏi vòng kính, bị `ego_body` che hoàn toàn) —
   không xoá track, vì vật có thể quay lại và cần giữ cùng ID.

   Trước khi **nối track qua hai camera**, cần ít nhất ba bằng chứng: (1) timestamp của hai stream đã đồng bộ đủ
   chính xác (sai số nhỏ hơn thời gian vật di chuyển qua vùng seam), (2) calibration extrinsic giữa hai camera đã
   hiệu chỉnh để chiếu box về cùng một hệ toạ độ (BEV hoặc 3D), và (3) một policy rõ ràng về việc hệ thống đích
   cần một ID toàn cục hay chấp nhận hai ID riêng biệt theo từng camera. Thiếu một trong ba, không tự nối track —
   giữ hai track riêng và escalate như câu 1.

3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`),
   bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?

   Ca cụ thể: `adasind_012570.jpg`, object `L6` (vòng `r1_craft`, slice `B1-dense`). Lúc vẽ, tôi tin một box
   `Bike` là đủ cho người lái xe máy đang tiến về phía camera, vì mắt thường ở lần nhìn đầu không tách rõ một xe
   hơi trắng khuất một phần ngay phía sau/cạnh người lái (ảnh phóng to ở `screenshots/l6_merge_before_after.png`
   cho thấy rõ hơn nhiều so với xem ảnh gốc). Reference tách thành `R7 Car` + `R8 Bike`.

   **Cách xử lý:** tôi không sửa ngay chỉ vì reference khác — theo `docs/05-taxonomy-vi.md`, reference có thể sai
   (`E0_reference_defect`) nên cần bằng chứng độc lập thứ hai trước khi kết luận đó là lỗi của tôi. Tôi đối chiếu
   thêm với model (M9 Car, M6 Bike cũng độc lập tách thành hai box ở đúng vị trí đó) — hai nguồn độc lập cùng
   đồng ý là bằng chứng đủ mạnh, nên tôi phân loại `E1_annotator_error` (không phải `E0`) và rework: tách lại
   thành hai box đúng như reference/model, ghi số trước/sau đo được trong `rework/delta.md` (zone `center`: matched
   6→8, missing 3→1, spurious 2→1).

   **Nếu làm lại slice này, tôi sẽ đổi:** chủ động dừng lại và tự hỏi "đây có chắc là MỘT vật thể không?" ở MỌI
   cụm vật chồng lấn tại zone `center` trước khi vẽ box đầu tiên, thay vì chỉ cảnh giác ở zone `edge` (nơi tôi mặc
   định nghĩ rủi ro cao hơn vì méo hình học). Ca này cho thấy rủi ro lớn nhất của tôi trong buổi không đến từ méo
   ống kính mà từ **độ rối của cảnh** (occlusion cluster) ở vùng ít méo nhất — một giả định sai về "zone an toàn"
   mà tôi mang theo từ đầu buổi.
