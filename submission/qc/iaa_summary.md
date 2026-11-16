# Tóm tắt IAA (taxonomy v1.0)

- Ảnh được so sánh (có >=2 người gán): **40**; số người gán: 2
- Lượt gán đủ **80/80**.
- IoU trung bình và % cùng lớp **chỉ tính trên các cặp box đã ghép** (IoU >= 0.5, ghép 1-1 không phân biệt lớp). Box chưa ghép được thống kê riêng ở cột bỏ sót/thừa.
- Mẫu số: 310 cặp box ghép (tổng mọi cặp người gán, ảnh chung).

| Cặp | Ảnh | Cặp box ghép | IoU TB | Cùng lớp | Cùng occluded_level | Cùng truncated | Chưa ghép (A/B) | Bỏ qua do ignore | Đạt IAA |
|---|---|---|---|---|---|---|---|---|---|
| **TỔNG** | 40 | 310 | 0.904 | 93.5% | 80.6% | 98.4% | 94/47 | 13 | CHƯA ĐẠT |
| 101 và 102 | 40 | 310 | 0.904 | 93.5% | 80.6% | 98.4% | 94/47 | 13 | CHƯA ĐẠT |

Ngưỡng IAA: IoU TB >= 0.85, đồng thuận lớp >= 95%.

Số ca bất thường: 366 ({'bo_sot_thua': 141, 'iou_thap': 48, 'sai_lop': 20, 'sai_thuoc_tinh': 63, 'can_review_anh': 4, 'co_can_review': 90}), trên 34 ảnh cần calibration.
