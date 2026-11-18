# Label contract — Phố châu Á — BỔ SUNG v1.1 — 2A202603062 - Hồ Xuân Hùng

## 1. Câu hỏi mở đã chốt

| Câu hỏi | Quyết định | Ảnh ví dụ | case_id |
|---|---|---|---|
| Mask xe ba bánh có được gồm người lái không? | Không. Mask chỉ bao phủ thân xe; loại vùng người lái và người đi đường. | `img_013.jpg` | `CAL-01` |

## 2. Ca khó mới

| Ảnh | Tình huống | Máy làm gì | Quyết định | Quy tắc mới |
|---|---|---|---|---|
| `img_024.jpg` | wrong_class bị lọt | Gán sai lớp cho phương tiện | Đối chiếu hình dạng/ngữ cảnh rồi sửa lớp | Box đúng nhưng lớp sai phải đổi lớp trước khi accept. |
| `img_022.jpg` | missing bị lọt | Bỏ sót một đối tượng | Quét ảnh và vẽ bổ sung | Quét toàn ảnh trái-sang-phải, trên-xuống-dưới trước khi duyệt box máy. |
| `img_013.jpg` | mask dính người lái | Mask xe phủ lẫn người lái | Negative click/chỉnh biên | Mask xe không bao gồm người lái hoặc người đi đường. |

## 3. Lịch sử thay đổi

| Phiên bản | Thay đổi | Lý do |
|---|---|---|
| v1.0 | Bản giảng viên | — |
| v1.1 | Chốt phạm vi mask xe, kiểm tra lớp phương tiện và quét ảnh trước khi accept. | `CAL-01`, `CAL-02`, `CAL-03` |
