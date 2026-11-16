# Thông tin nhóm

- Tên nhóm: Gojo
- Mã thành viên (ba chữ số): 101, 102
- Người điều phối: 101
- Mã review theo vòng: 102 ở vòng 1, 101 ở vòng 2, 102 ở vòng 3 và 101 ở vòng 4.
- Phương pháp gán nhãn và trạng thái xác nhận được ghi trong báo cáo QC.
- Hình thức bàn giao: gói ZIP kèm starter repo, 40 ảnh gốc và ghi công.

## Kết quả tóm tắt

- Lượt gán hoàn tất / được giao: 80/80, mỗi mã 40 ảnh.
- IAA trước calibration: IoU TB 0,904 trên 310 cặp ghép; cùng lớp 290/310 (93,5%). Chưa đạt ngưỡng cùng lớp 95%.
- Số ảnh phải calibration: 34/40 theo IAA. Đợt sửa trước QC vòng 4 chỉnh vùng ignore trên 14 ảnh và thêm 58 box đối tượng.
- Vòng QC và số ảnh đã duyệt / cả lô: vòng 4 rà tự động 40/40 ảnh. Phiếu vòng 3 ghi 0/40 trước khi phát hiện lỗi ở ảnh 032.
- Số lỗi xác minh thêm sau sửa ở vòng 4: phiếu ghi 0/40 ảnh và 0/449 đối tượng. Ba box còn cờ `can_review`.
- **Quyết định: Hold.** Cần xác nhận ca A002–A004 trên ảnh gốc. Xem [báo cáo QC](submission/qc/QC_REPORT.md).
