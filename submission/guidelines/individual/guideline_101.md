# Đề xuất guideline cá nhân — 101

- Ngày 16/11/2026. Bản đề xuất mã 101, ưu tiên cấu tạo của vật.

## 1. Mục tiêu

Gán hộp chữ nhật 2D cho người thật và xe thật trong ảnh đường phố để thực hành huấn luyện và QC. Giữ người và phương tiện thành các đối tượng riêng để lỗi ở một lớp không che mất lỗi ở lớp kia.

## 2. Lớp và thuộc tính

| Lớp | Tiêu chí và ví dụ | Dễ nhầm |
|---|---|---|
| `person` | Người thật, kể cả người ngồi trên xe hoặc nhìn qua kính trong. Hai người trên `img_026.jpg` có hai box. | Tượng `img_017.jpg`, ảnh người trên billboard `img_018.jpg`. |
| `car` | Xe con, SUV hoặc pickup có khoang khách và thùng liền kiểu bán tải. `img_037.jpg` vẫn là pickup dù có nắp cao. | Xe tải đóng thùng riêng `img_036.jpg`. |
| `truck` | Xe có cabin và nền/thùng chở hàng riêng hoặc đầu kéo với rơ-moóc gắn vào. `img_002.jpg` là một tổ hợp tải. | Van thân liền; xe được chở là các thực thể riêng. |
| `van` | Xe thân hộp liền từ cabin đến khoang sau, ví dụ xe bạc `img_005.jpg`. | Bus thân dài và pickup lắp nắp thùng. |
| `motorcycle` | Xe hai bánh có động cơ, xe có sidecar và tuk-tuk/AUTO ba bánh có động cơ. | Xích lô hoặc xe đạp đạp chân. |
| `ignore_region` | Vùng nhỏ gọn bao cụm các đối tượng trong phạm vi nhưng quá nhỏ hoặc chồng đến mức không tách được. | Vé xe, tranh, tượng, bus và các đối tượng ngoài phạm vi để làm nền. |

- `occluded_level` dựa trên phần diện tích hình chiếu bị vật khác che trong khung. `0` = 0%, `1` >0–25%, `2` >25–60%, `3` >60% và còn thấy dấu hiệu nhận diện.
- `truncated=yes` khi vật tiếp tục ra ngoài một cạnh ảnh. `no` khi vật kết thúc trong ảnh hoặc bị vật khác che.
- `can_review=true` khi phân lớp, tính xác thực hoặc ranh giới còn nghi vấn. Ghi vấn đề cấp ảnh khi chưa thể vẽ.

## 3. Vẽ box

Vẽ box ôm các pixel nhìn thấy của vật, nối các phần nhìn thấy rời nhau của cùng một vật. Dùng box riêng cho người lái, hành khách và xe. Trang phục đang mặc thuộc người. Sidecar, gương ngoài xe và mui gắn vào xe thuộc xe. Hàng rời, bóng và phản chiếu ở ngoài box. Chỉ gán vật có chiều cao thấy được ≥20 px và có dấu hiệu nhận diện. Gom cụm không tách được bằng vùng hẹp, ưu tiên tách vật rõ ở tiền cảnh.

## 4. Ca thật cần quyết định

| Ảnh | Quyết định ban đầu | Lý do |
|---|---|---|
| `img_002.jpg` | Một `truck` cho đầu kéo và giàn chở, từng xe con nhìn rõ có box riêng. | Xe trên sàn có thân và bánh riêng, hai box được chồng nhau. |
| `img_026.jpg` | Một `motorcycle`, hai `person`. Đuôi xe chạm cạnh dưới và tiếp tục ra ngoài thì `truncated=yes`. | Hai mũ bảo hiểm và hai dáng người phân biệt được. |
| `img_035.jpg` | Tuk-tuk là `motorcycle`, ôm thân và mui, loại các bao buộc trên nóc. | Biên hàng thay đổi theo chuyến, biên xe giữ theo cấu tạo. |
| `img_037.jpg` | Pickup là `car`, box gồm nắp thùng cứng gắn cố định. | Đường phân cách cabin và thùng vẫn cho thấy nền pickup. |

## 5. QC và phiên bản

Bản cá nhân trước v1.0. Giữ các ngưỡng taxonomy. QC theo lớp, hình học, thuộc tính, box thừa/thiếu và cờ review. Mọi thay đổi cách chọn vật cần ghi phiên bản và phạm vi kiểm lại.
