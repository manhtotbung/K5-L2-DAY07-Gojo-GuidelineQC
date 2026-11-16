# Guideline gán nhãn 2D — Gojo v1.0

- Ngày 16/11/2026. Áp dụng cho hai lượt gán mã 101 và 102.

## 1. Mục tiêu

Gán bounding box cho người và phương tiện thật trong 40 ảnh của lab. Đầu ra dùng để thực hành phát hiện vật thể và QC ảnh tĩnh. Mỗi box là một vật cụ thể hoặc một vùng ignore, tính theo pixel ảnh gốc.

## 2. Lớp

| Lớp | Quy tắc áp dụng | Ví dụ và phản ví dụ |
|---|---|---|
| `car` | Xe con, SUV, taxi kiểu xe con và pickup có nền/thùng kiểu bán tải. | Pickup có nắp cao `img_037.jpg` vẫn là `car`. Xe cabin gắn thùng tải riêng ở `img_036.jpg` là `truck`. |
| `truck` | Xe hàng có cabin và thùng/sàn hàng tách biệt về kết cấu. Tổ hợp đầu kéo và rơ-moóc đang nối tính một xe. | Xe chở ô tô `img_002.jpg`. Xe van thân liền thuộc `van`. |
| `van` | Xe thân hộp liền từ cabin sang khoang sau, khoang sau kín và không có đường tách thùng rời của pickup/xe tải. | Xe bạc `img_005.jpg`. Bus/minibus dùng kết cấu bus với nhiều cửa sổ và nhiều hàng ghế rõ nằm ngoài phạm vi. |
| `motorcycle` | Mô tô, xe máy, scooter, cả tổ hợp sidecar và tuk-tuk ba bánh có động cơ. | Hai tuk-tuk `img_004.jpg`, sidecar `img_022.jpg`. Xe đạp và xích lô nằm ngoài phạm vi. |
| `person` | Người thật nhìn trực tiếp, kể cả người ngồi xe, người trên xe đạp, người mặc hóa trang nếu nhận ra người thật. | Hai người trên `img_026.jpg`. Tượng, ma-nơ-canh và người in trên pano không gán. |
| `ignore_region` | Vùng hẹp của các đối tượng mục tiêu rất nhỏ, quá mờ hoặc chồng đến mức khó phân định từng cá thể. | Dải sâu trong dòng xe `img_021.jpg`. Bus, đồ chơi và tượng giữ làm nền. |

## 3. Quy tắc vẽ box và thuộc tính

- **R-01 — Chủ thể có thật và nhìn trực tiếp.** Bỏ tượng, mô hình, đồ chơi, người in trên pano, bóng, phản chiếu qua gương và phần của xe chủ (xe đang mang máy ảnh). Vật thật nhìn qua cửa kính trong vẫn gán nếu xác định được. Xe cải trang cỡ thật có dấu hiệu vận hành rõ như người lái và bánh chịu tải vẫn gán.
- **R-02 — Một vật, một box.** Mỗi người một box riêng, kể cả người lái và hành khách. Mỗi xe một box riêng. Box có thể chồng nhau. Người trên xe đạp vẫn là `person`. Xe đạp, xích lô, bus và vật ngoài taxonomy để làm nền.
- **R-03 — Phần nhìn thấy.** Lấy cực trị x/y của phần thấy được và dùng một box dù các phần bị vật khác ngăn cách. Ép vào biên ảnh. Tính trang phục và mũ đang đội, thân xe, bánh, mui, gương gắn cố định và sidecar. Bỏ hàng rời, hành lý, bóng và vệt nhòe ngoài biên có thể xác định.
- **R-04 — Cấu tạo xe.** Phân pickup/van/truck theo đường tách khoang hàng và loại nền xe. Nắp thùng cao trên pickup vẫn là phụ kiện của `car`. Tuk-tuk có động cơ vào `motorcycle`, gồm mui và thân, không kéo theo hàng trên mui. Xe được chở chỉ gán riêng nếu nhận diện được thân và là xe thật. Thân vỏ trưng bày không có đủ dấu hiệu là xe hoàn chỉnh thì bỏ.
- **R-05 — Che khuất.** Ước % diện tích hình chiếu của vật nằm trong ảnh bị vật khác che. `occluded_level=0` khi 0%, `1` khi >0 đến 25%, `2` khi >25 đến 60%, `3` khi >60% nhưng còn đủ dấu hiệu để xác định vật/lớp. Không cộng phần ngoài khung vào tỷ lệ. Nếu khó phân biệt hai mức sát ngưỡng, dùng `can_review`.
- **R-06 — Cắt mép.** `truncated=yes` khi vật bị khung ảnh cắt và còn tiếp tục ra ngoài, kể cả khi vừa bị che. Vật nằm sát biên nhưng kết thúc trong ảnh là `no`. Mép của vật che không phải mép ảnh.
- **R-07 — Vật nhỏ và đám đông.** Đo chiều cao box phần nhìn thấy ở ảnh gốc. Vật đã định lớp, tách được và cao ≥20 px thì vẽ. Vật thấp hơn hoặc nhiều đối tượng đan xen, nhòe đến mức không thể gán riêng thì khoanh vùng nhỏ nhất chứa phần không tách được. Ưu tiên đưa vật rõ, gần ra ngoài vùng ignore. Một vật ngoài taxonomy không tạo vùng ignore.
- **R-08 — Ca mơ hồ.** Nếu vẫn đủ dấu hiệu gán box, bật `can_review=true` và giữ lớp tạm. Nếu không khoanh được vật hoặc cần xét cả vùng, ghi `image_issues.csv`. Nhãn cuối chỉ giữ các lựa chọn đã đối chiếu ảnh gốc.

## 4. Ca khó và lý do chốt

| Ảnh | Quy tắc | Quyết định v1.0 |
|---|---|---|
| `img_002.jpg` | R-02, R-04 | Một box cho xe tải chở xe và box riêng cho từng xe con nhìn rõ. |
| `img_004.jpg`, `img_035.jpg` | R-03, R-04 | Dùng phương án 101 cho tuk-tuk vì xe có động cơ và cấu trúc gần xe máy ba bánh. Loại hàng rời khỏi box. |
| `img_018.jpg`, `img_020.jpg`, `img_033.jpg`, `img_038.jpg` | R-01 | Bỏ ảnh người, phản chiếu và mô hình. Xe thật sau hàng rào ở ảnh 018 vẫn gán nếu đủ dấu hiệu. |
| `img_019.jpg`, `img_027.jpg` | R-01 | Xe diễu hành có dấu hiệu vận hành và người lái ở ảnh 019 thuộc `car`. Pickup cố định trên bệ tưởng niệm ở ảnh 027 giữ làm nền. |
| `img_022.jpg`, `img_026.jpg` | R-02, R-03, R-06 | Sidecar cùng một xe. Xe trong hầm và hai người có ba box, mỗi box xét cắt mép riêng. |
| `img_024.jpg`, `img_028.jpg` | R-01, R-04 | Chốt dựa trên cấu tạo: xe thật độc lập có box. Khung trưng bày và tượng chở trên thùng không kéo dài box xe bên dưới. |
| `img_037.jpg` | R-04 | Chọn `car` theo nền pickup dù nắp thùng cao dễ đọc nhầm là van. |

Hai bản đề xuất khác nhau ở tuk-tuk, pickup có nắp và mốc che ít. Nhóm dùng mốc 25% để có một khoảng đo cố định, giữ lớp theo kết cấu và sử dụng `can_review` ở ca khó.

## 5. QC và phiên bản

Khóa v1.0 trong lúc gán. Giữ các ngưỡng `rules` của taxonomy. Dùng IAA để tìm khác biệt, ảnh gốc để xét lớp và hình học, và duyệt toàn bộ 40 ảnh trước quyết định bàn giao. Mọi sửa đổi cần có rule ID và dấu vết trong bảng calibration.
