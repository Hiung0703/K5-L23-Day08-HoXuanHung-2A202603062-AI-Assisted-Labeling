# Số đo review (det)

- Ảnh tính: 24 (batch,gold); box máy: 578; đối tượng sau duyệt: 424; ngưỡng accepted: IoU >= 0.9
- Đã **loại 23 đối tượng nằm quanh các bẫy** khỏi bảng này: lỗi nhân tạo không phản ánh chất lượng máy.

## 1. Bốn trạng thái (AI error profile)
| Trạng thái | Số | Tỉ lệ | Mẫu số |
|---|---|---|---|
| accepted (giữ nguyên) | 397 | 397/578 = 68.7% | 578 box máy |
| edited (sửa) | 13 | 13/578 = 2.2% | 578 box máy |
| deleted (xoá) | 161 | 161/578 = 27.9% | 578 box máy |
| added (vẽ thêm) | 14 | 14/424 = 3.3% | 424 đối tượng sau duyệt |

Trong số deleted + added, **6 trường hợp là đổi lớp** (máy gán sai lớp, box đúng chỗ): đó không phải máy bỏ sót. Số vật máy **sót thật** ≈ added − đổi lớp = 8.

### Theo lớp
| Lớp | accepted | edited | deleted | added |
|---|---|---|---|---|
| person | 247 | 6 | 117 | 0 |
| bicycle | 6 | 0 | 2 | 1 |
| motorbike | 81 | 4 | 4 | 7 |
| car | 40 | 1 | 13 | 3 |
| bus | 2 | 0 | 1 | 1 |
| truck | 1 | 0 | 5 | 1 |
| three_wheeler | 20 | 2 | 19 | 1 |

## 2. So với gold (nhãn gán tay từ đầu)
Ảnh gold: 4; đối tượng gold: 61 (mẫu số của recall)
| | TP | FP | FN | Precision | Recall |
|---|---|---|---|---|---|
| Máy (pre-label) | 55 | 23 | 6 | 70.5% (KTC 95%: 60–79%) | 90.2% (KTC 95%: 80–95%) |

*Lạc quan: ngưỡng được chọn trên chính các ảnh gold này (in-sample), và chỉ có 4 ảnh. Đọc khoảng tin cậy, đừng đọc con số đơn.*


(Ảnh gold không nằm trong nhãn sau duyệt: không tính residual / new error. Đó là chủ ý: duyệt lại ảnh bạn vừa tự gán tay sẽ đo trí nhớ, không đo thiên kiến.)

## 3. Ảnh bẫy
- Bắt được 5/12 = 41.7% (KTC 95%: 19–68%) (ngưỡng đạt: 80%). Chỉ 12 bẫy nên khoảng tin cậy rất rộng: đọc cùng danh sách bẫy lọt, đừng kết luận chỉ từ %.
  - loose: 1/3
  - extra: 2/3
  - wrong_class: 0/3
  - missing: 2/3
  - lọt: img_009.jpg (loose), img_025.jpg (extra), img_024.jpg (wrong_class), img_022.jpg (missing), img_013.jpg (wrong_class), img_014.jpg (loose), img_006.jpg (wrong_class)

## Gợi ý đọc số
- `deleted` cao → ngưỡng thấp hoặc ánh xạ nhãn sai; `added` cao → máy bỏ sót (xem lớp nào);
- `accepted` thấp (< 50%) → sửa mỗi box lâu hơn vẽ mới: đổi model/ngưỡng hoặc vẽ tay;
- bắt bẫy < 80% → duyệt lại lô, siết checklist (quét ảnh TRƯỚC khi nhìn nhãn máy).
