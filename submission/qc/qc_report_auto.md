# Báo cáo QC tự động (nháp)

> Báo cáo được tổng hợp từ phiếu kiểm mẫu và `review_stats.json`.

## Mẫu

- 5 ảnh mẫu, 78 đối tượng.
- Các ảnh mẫu: `img_003.jpg`, `img_006.jpg`, `img_014.jpg`, `img_031.jpg`, `img_035.jpg`.

## Số đo

| Chỉ số | Kết quả | Mẫu số |
|---|---:|---:|
| Ảnh có lỗi QC | 0/5 = 0.0% | 5 ảnh mẫu |
| Đối tượng có lỗi QC | 0/78 = 0.0% | 78 đối tượng mẫu |
| Accepted | 397/578 = 68.7% | 578 box máy |
| Edited | 13/578 = 2.2% | 578 box máy |
| Deleted | 161/578 = 27.9% | 578 box máy |
| Added | 14/424 = 3.3% | 424 đối tượng sau duyệt |
| Bắt lỗi bẫy | 5/12 = 41.7% | 12 bẫy |

## Kết luận

**REWORK**

- Mẫu QC 0/5 ảnh có lỗi rõ ràng, nhưng mẫu chỉ có 5 ảnh nên chưa đủ để chứng minh tỷ lệ lỗi thật dưới 2%.
- Tỷ lệ bắt bẫy 5/12 = 41.7%, thấp hơn mục tiêu 80% (10/12), vì vậy cần siết checklist và rà lại các ảnh bẫy bị lọt.
- Các bẫy bị lọt gồm: `img_009.jpg`, `img_025.jpg`, `img_024.jpg`, `img_022.jpg`, `img_013.jpg`, `img_014.jpg`, `img_006.jpg`.

## Giới hạn thống kê

Với mẫu chỉ 5 ảnh, cận trên Wilson 95% của tỷ lệ ảnh lỗi vẫn có thể cao; kết quả 0/5 không đủ để khẳng định toàn bộ batch đạt ngưỡng 2%.
