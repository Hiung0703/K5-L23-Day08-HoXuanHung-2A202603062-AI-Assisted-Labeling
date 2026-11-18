# Báo cáo Day 08 — 2A202603062 - Hồ Xuân Hùng

## 1. Model và recipe

- YOLO26m được dùng cho `person`, `bicycle`, `motorbike`, `car`, `bus`, `truck`.
- YOLOE-s với prompt `three_wheeler` được dùng bổ sung vì YOLO26 thường gọi tuk-tuk/xe ba bánh là `car` hoặc `truck`.
- SAM 2.1 (`sam2.1_b.pt`) chạy cùng YOLO26 trên `img_004.jpg` và `img_013.jpg`, tạo 16 mask với tốc độ 2.38 giây/ảnh trên CUDA.
- [model_log.csv](model_log.csv) ghi 7 lần chạy: YOLO26 conf 0.25 tạo 215 box/8 ảnh, conf 0.5 tạo 140 box/8 ảnh; YOLOE tạo 8 box `three_wheeler`/8 ảnh; chạy toàn bộ tạo 1672 box YOLO26 và 125 box YOLOE trên 36 ảnh.
- Lỗi tuk-tuk bị gọi `car`/`truck` không thể giải quyết chỉ bằng đổi confidence; cần YOLOE prompt `three_wheeler`, map lớp và review.
- Ngưỡng từ [threshold_sweep.md](qc/threshold_sweep.md): `person=0.15,bicycle=0.3,motorbike=0.5,three_wheeler=0.15`; lớp còn lại dùng mặc định 0.3. Ngưỡng chọn trên gold 4 ảnh nên số đo lạc quan.

## 2. AI error profile

Theo [review_stats.md](qc/review_stats.md), trên 578 box máy: accepted `397/578 = 68.7%`, edited `13/578 = 2.2%`, deleted `161/578 = 27.9%`; added `14/424 = 3.3%` đối tượng sau duyệt.

So với gold 4 ảnh và 61 đối tượng: TP `55`, FP `23`, FN `6`; precision `55/(55+23) = 70.5%`, recall `55/61 = 90.2%`. Máy có recall khá nhưng precision còn thấp, nên chỉ dùng làm pre-label.

## 3. Thiên kiến tự động hóa và ảnh bẫy

Máy bắt được `5/12 = 41.7%` bẫy, thấp hơn mục tiêu `80%` (10/12). Bẫy bị lọt: `img_009.jpg` loose, `img_025.jpg` extra, `img_024.jpg` wrong_class, `img_022.jpg` missing, `img_013.jpg` wrong_class, `img_014.jpg` loose, `img_006.jpg` wrong_class.

Accepted `397/578 = 68.7%` không chứng minh máy đáng tin: bắt bẫy thấp cho thấy nguy cơ accept box trông hợp lý mà chưa quét ảnh. Ba quy tắc giảm thiên kiến là: (1) quét trái-sang-phải, trên-xuống-dưới trước khi xem box; (2) kiểm riêng `three_wheeler` trước khi accept `car`/`truck`; (3) đánh dấu box mơ hồ để review lại, không dùng confidence cao làm lý do accept.

## 4. Máy có đáng dùng không?

Máy đáng dùng như pre-label cho lớp phổ biến vì recall trên gold là `55/61 = 90.2%`, nhưng precision là `55/78 = 70.5%` và còn FN `6/61`. Với `three_wheeler`, cần YOLOE và review thủ công. `time_log.csv` hiện ghi ước tính 37 phút cho gold và 37 phút cho batch từ timestamp file, không phải số bấm giờ trực tiếp.

## 5. Mask SAM

Hiện có 3 file mask SAM trong `masks/`: `tuktuk_img013.jpg`, `sam_img004.jpg` và `sam_img013_pipeline.jpg`. Mask tuk-tuk phủ một phần người lái trong cabin, nên phải loại vùng người lái và chỉ giữ thân xe. Hai file còn lại là kết quả SAM trong pipeline YOLO26 + SAM 2.1.

## 6. Calibration

Có 3 quyết định trong [calibration_decisions.csv](qc/calibration_decisions.csv): mask xe dính người lái, wrong class ở `img_024.jpg`, và missing ở `img_022.jpg`. Các quy tắc tương ứng đã ghi trong [guideline_v1.1_calibrated.md](guidelines/guideline_v1.1_calibrated.md).

## 7. QC và quyết định

Mẫu QC gồm 5 ảnh ngẫu nhiên theo seed 42, tổng 78 đối tượng. [qc_review.csv](qc/qc_review.csv) ghi nhận `0/5 = 0.0%` ảnh lỗi và `0/78 = 0.0%` đối tượng lỗi trong mẫu. Mẫu nhỏ nên chưa đủ chứng minh toàn batch dưới ngưỡng 2%; cận trên 95% vẫn rộng.

Quyết định: **Rework**, vì bắt bẫy chỉ `5/12 = 41.7%`, thấp hơn 80%. Cần rà lại 7 trap bị lọt, cập nhật nhãn nếu cần, rồi chạy lại thống kê và QC.

## 8. Ghi chú cá nhân

- Dùng AI hỗ trợ soạn recipe, kiểm tra cấu trúc file và phân tích số liệu; nhãn cuối cần kiểm bằng mắt trong CVAT.
- Gold đã validate: 4 ảnh, 69 đối tượng.
- Máy: NVIDIA GeForce RTX 3060 Laptop GPU, CUDA hoạt động; CVAT server v2.81.0.
- `time_log.csv` đang dùng ước tính theo timestamp; nếu có số bấm giờ thật thì thay lại trước khi nộp.
