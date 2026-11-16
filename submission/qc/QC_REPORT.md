# Báo cáo QC — Gojo

## 1. Mục đích, dữ liệu và phiên bản

- Mục đích và giới hạn sử dụng: Gán và kiểm tra box 2D cho bài Day 07 trên 40 ảnh tuyển chọn. Các cảnh đêm, vật bị che và cụm dày cần rà thị giác. Tập này phục vụ thực hành, chưa đánh giá độ đại diện cho vận hành.
- Nguồn và phép sử dụng dữ liệu: Giữ thông tin tác giả, giấy phép và URL của cả 40 ảnh trong [ATTRIBUTION.csv](../../data/ATTRIBUTION.csv). Ảnh gốc đi kèm gói nộp.
- Manifest và checksum nhãn cuối: SHA-256 COCO `d19adcd9755bb92f2065ca3b15ad5c76a85a43d9f7d4f09ea3f0797de9578be0`. [Manifest vòng 4](batch_manifest.json)
- Guideline và taxonomy khi gán / khi bàn giao: Dùng guideline và taxonomy v1.0 cho hai lượt nhãn IAA. Bàn giao theo guideline và taxonomy v1.1. Định nghĩa `van` ghi rõ bus và minibus kiểu bus nằm ngoài phạm vi. Pickup có nắp thùng thuộc `car`. Giữ ID lớp, miền thuộc tính và mục `rules`. [Taxonomy v1.1](../taxonomy_v1.1.json)

### Phương pháp

Hai thành viên 101 và 102 gán nhãn độc lập trên CVAT theo guideline v1.0. Mỗi người gán đủ 40 ảnh và xuất một tệp COCO riêng. Nhóm tính IAA từ hai tệp COCO, đối chiếu các ca bất đồng trên ảnh gốc, thống nhất cách sửa và rà soát lại bộ nhãn bàn giao.

## 2. Phân công và đồng thuận trước calibration

- Lượt hoàn thành / được giao: 80/80, mỗi mã 40 ảnh. Hai file riêng trong [labels](../labels/).
- Số cặp box ghép, IoU TB, đồng thuận lớp và mẫu số: 310 cặp 1–1 tại IoU ghép ≥0,50; IoU TB 0,904; cùng lớp 290/310 (93,5%); cùng mức che 250/310 (80,6%); cùng cắt mép 305/310 (98,4%). Ngưỡng cùng lớp 95% chưa đạt.
- Box chưa ghép, loại do ignore, bất đồng vùng ignore: 94 box của 101 và 47 box của 102 chưa ghép. 13 box khác được bỏ qua vì cả hai lượt đều có vùng ignore phủ. Phiếu IAA ghi 0 ca bất đồng vùng ignore và tổng cộng 366 ca gắn cờ trên 34 ảnh. [Tóm tắt IAA](iaa_summary.md)
- Ca hai người có thể cùng sai sau khi đối chiếu tham chiếu: Cả hai lượt nhãn v1.0 gán `car` cho thân vàng trên nóc ở `img_024.jpg`. Ca A001 đối chiếu ảnh gốc với R-01/R-04. Bản cuối bỏ box thân trưng bày và giữ box ô tô đen bên dưới. Với `img_017.jpg` từ tài liệu coach, tượng người cưỡi ngựa giữ làm nền.

## 3. Calibration và kiểm lại

- Ca đã xử lý, rule ID, bản sửa và người xác nhận: Bảng quyết định có 420 dòng gồm 366 ca IAA, A001–A004 và Q001–Q050. Q036 làm rõ bus và pickup theo R-02/R-04. Q037–Q050 chỉnh R-07 trên 14 ảnh. Q048 bỏ vùng ignore phủ ngang hàng chờ xe ở ảnh 032 và bổ sung 8 box người. A002–A004 còn chờ người duyệt lớp xác nhận. [Bảng quyết định](calibration_decisions.csv)
- Tác động v1.1 lên nhãn và ảnh: Vùng ignore chỉnh trên 11 ảnh có box bị phủ từ 50% trở lên và ba ảnh bổ sung 025, 031, 039. Thêm 58 box đối tượng và chỉnh biên một box xe máy. Số box có ít nhất 50% diện tích nằm trong một vùng ignore giảm từ 35 trên 11 ảnh ở bản vòng 3 xuống 0 trên 40 ảnh ở bản vòng 4. Phép đo này dùng để tìm vùng cần rà R-07.
- Tham chiếu bằng chứng khi Rework: Vòng 1 ghi Q016 về biên box xe đen ở ảnh 024. Vòng 2 ghi Q033–Q035 về box trùng ở ảnh 008, 021, 023. Vòng 3 ghi `khong` ở ảnh 032, sau đó phát hiện vùng ignore quá rộng và người bị bỏ sót. Vòng 4 ghi đối chiếu bộ nhãn đã sửa. Mỗi vòng có phiếu, snapshot nhãn và checksum. [Vòng 1](reviews/round-01/qc_review.csv) · [Vòng 2](reviews/round-02/qc_review.csv) · [Vòng 3](reviews/round-03/qc_review.csv) · [Vòng 4](reviews/round-04/qc_review.csv)

## 4. Kết quả duyệt

- Vòng QC: 4
- Người review và ngày: Phiếu tự động vòng 4 dùng mã 101 ngày 16/11/2026. Việc xác nhận trực tiếp của người duyệt lớp đang chờ.
- Đã duyệt / cả lô: Lượt rà vòng 4 ghi nhận 40/40 ảnh.
- Ảnh lỗi / ảnh đã duyệt: Vòng 1 ghi 20/40 (50,0%). Vòng 2 ghi 3/40 (7,5%). Phiếu vòng 3 ghi 0/40 trước khi phát hiện lỗi ở ảnh 032. Phiếu vòng 4 ghi 0/40 lỗi xác minh thêm sau các sửa đổi Q037–Q050. Ba ca mở vẫn chờ đối chiếu trực tiếp.
- Annotation đối tượng hiện có bị lỗi / annotation đối tượng đã duyệt: Vòng 1 ghi 20/385 (5,2%). Vòng 2 ghi 3/394 (0,8%). Phiếu vòng 3 ghi 0/391 (0,0%). Vòng 4 ghi 0/449 lỗi xác minh thêm (0,0%). Mẫu số không tính 45 vùng ignore của bản vòng 4.
- Vật bỏ sót, ảnh và vùng: Vòng 1 ghi 12 vật bỏ sót, ví dụ người áo vàng ở ảnh 032. Đợt sửa trước vòng 4 bổ sung 58 box, gồm 8 người trong hàng chờ ở ảnh 032. Lượt rà sau khi sửa chưa ghi thêm vật bỏ sót đã xác minh.
- Ca khó / phần còn lại, lỗi lặp và ví dụ: Vòng 1 ghi 16 lỗi kích thước hoặc vị trí. Vòng 2 ghi ba box trùng ở ảnh 008, 021 và 023. Vòng 4 còn cờ `can_review` ở ảnh 005, 011 và 030 cho các ca A002–A004. Kết quả lượt rà vẫn cần người duyệt lớp xác nhận. [Phiếu vòng 4](reviews/round-04/qc_review.csv)
- Một lỗi máy chặn được và một lỗi cần người đọc ảnh: Validator chặn box vượt kích thước ảnh. Việc xác định thân vàng trên nóc là phần trưng bày trong ca A001 cần đối chiếu ảnh gốc với quy tắc gán nhãn.

## 5. Quyết định bàn giao

- Quyết định: Hold
- Lý do, người nhận và phạm vi sử dụng: Ba box chưa chốt do che nặng hoặc khó phân biệt vật thật với cụm trưng bày. Gói dành cho coach và nhóm để kiểm bài và tiếp tục rà; trì hoãn Release cho đến khi xử lý cờ.
- Nếu còn việc: Người duyệt lớp mở `img_005.jpg` hộp #65, `img_011.jpg` hộp #170 và `img_030.jpg` hộp #361 trong nhãn vòng 4 để xác nhận đối tượng và biên. Mã 101 phụ trách cập nhật khi có kết quả. Sau mỗi bản sửa chạy lại `prepare_calibration.py`, `build_final.py` và mở vòng QC mới.
- Kiểm ngược từ báo cáo tới ảnh, rule và nhãn: Ca Q048 nối ảnh 032 với R-07 và tọa độ của tám box mới. Snapshot vòng 3 có vùng ignore #319 tại COCO `[271,253,358,113]`. Snapshot vòng 4 bỏ vùng đó và giữ các box người tách được. Ca Q036 đối chiếu pickup có nắp ở ảnh 037 với R-04 và box `car` #466. [Ảnh 032](../../data/images/img_032.jpg) · [Ảnh 037](../../data/images/img_037.jpg) · [Nhãn vòng 4](reviews/round-04/labels_snapshot.json)
