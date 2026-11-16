# Đề xuất guideline cá nhân — 102

- Ngày 16/11/2026. Bản đề xuất mã 102, ưu tiên bằng chứng nhìn thấy.

## 1. Mục tiêu

Phát hiện vật thể có thể nhận diện trực tiếp từ ảnh 2D. Tập 40 ảnh dùng để luyện cách giải quyết ca khó và kiểm tính nhất quán.

## 2. Lớp và thuộc tính

| Lớp | Dấu hiệu chính và ví dụ | Phản ví dụ |
|---|---|---|
| `car` | Xe con/SUV và xe diễu hành cỡ thật có bánh, người lái và dấu hiệu là xe vận hành (`img_019.jpg`). | Xe mô hình trong tủ `img_038.jpg`. |
| `truck` | Xe tải có cabin và bộ phận chở hàng phân biệt bằng kết cấu (`img_036.jpg`). | Xe con chở đồ bên trong. |
| `van` | Thân xe dạng hộp, khoang sau kín, ưu tiên dấu hiệu cửa trượt và thân liền (`img_005.jpg`). | Bus chở nhiều khách. Ban đầu cần xem lại `img_037.jpg` vì nắp cao dễ làm nhầm van. |
| `motorcycle` | Scooter, mô tô và xe máy có sidecar (`img_022.jpg`). Đề xuất ban đầu đưa tuk-tuk vào ngoài phạm vi cho tới khi nhóm thống nhất. | Xích lô `img_023.jpg`. |
| `person` | Người thật có thân, đầu hoặc chi đủ rõ, kể cả người đi xe đạp. | Ma-nơ-canh `img_033.jpg`, người in trên biển. |
| `ignore_region` | Cụm xa, mờ hoặc dày nơi không thể phân định từng thể hiện của lớp mục tiêu. | Một vật ngoài taxonomy không cần vùng ignore. |

- `occluded_level` ước theo diện tích vật bị che bên trong ảnh. `0` = 0%, `1` >0–30%, `2` >30–60%, `3` >60% và vẫn xác định được lớp. Cắt mép ghi riêng.
- `truncated` nhận `yes` nếu đường viền vật thực sự tiếp tục qua rìa ảnh, còn lại `no`.
- `can_review` bật nếu có hai cách giải thích hợp lý. Nếu không thể khoanh vật, ghi ảnh và vùng để người duyệt mở lại.

## 3. Vẽ box

Box người theo phần thấy được, gồm quần áo và mũ đang đội. Tách người với vật người điều khiển. Bỏ bóng và nhòe kéo dài ở nơi chưa nhìn rõ đường viền. Box xe theo các đầu mút phần nhìn thấy bao gồm phụ kiện cứng gắn cố định. Vật cao <20 px hoặc cụm trộn lẫn không thể tách thì ghi vùng ignore hẹp. Giữ box các vật vẫn tách được quanh cụm.

## 4. Ca thật cần quyết định

| Ảnh | Quyết định ban đầu | Lý do |
|---|---|---|
| `img_019.jpg` | Gán xe vàng `car` và người lái, loại hình chuột trang trí. | Có bánh, biển số và người điều khiển, đủ dấu hiệu là xe thật. |
| `img_020.jpg` | Để ảnh âm tính nếu chỉ nhận ra xe qua gương và phần nội thất của xe chụp. | Cả ảnh gương và xe chủ không phải mục tiêu nhìn trực tiếp trong cảnh. |
| `img_022.jpg` | Vẽ một `motorcycle` gồm cả sidecar. | Bộ bánh và khung kéo gắn vào mô tô, vận hành cùng nhau. |
| `img_033.jpg` | Loại ma-nơ-canh và ảnh phản chiếu. | Thấy vật giống người, xe qua kính vẫn cần kiểm bản chất của ảnh. |

## 5. QC và phiên bản

Bản cá nhân trước v1.0. Sau khi thống nhất, áp dụng cùng ngưỡng của lab cho hai lượt. Đối chiếu ảnh gốc trước khi chốt một ứng viên là bỏ sót hay thừa.
