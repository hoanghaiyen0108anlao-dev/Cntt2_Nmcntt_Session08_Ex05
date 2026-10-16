# 🚀 Smart-Farm — Trang trại thông minh

> **Tài liệu hướng dẫn sử dụng hệ thống IoT tại trang trại Lan**

## 1. 📖 Mô tả dự án

**Smart-Farm** là hệ thống ứng dụng *Internet vạn vật (IoT)* để thu thập dữ liệu môi trường, hỗ trợ quyết định tưới cây và giám sát hoạt động trang trại.

### Sơ đồ hệ thống

![Sơ đồ Smart-Farm: Cảm biến gửi dữ liệu đến bộ điều khiển, bộ điều khiển xử lý và điều khiển máy bơm hoặc van tưới, sau đó cập nhật trạng thái giám sát.](docs/smart-farm-architecture.png)

*Lưu ý: Đường dẫn ảnh trên là đường dẫn dự kiến. Cần bổ sung ảnh sơ đồ thực tế vào thư mục `docs` để hiển thị được.*

## 2. 🛠️ Hướng dẫn cài đặt

1. Kiểm tra cảm biến, bộ điều khiển và thiết bị tưới.
2. Cài đặt công cụ lập trình phù hợp với bo mạch.
3. Nạp chương trình đã được kiểm thử.
4. Cấu hình kết nối và ngưỡng tưới.
5. Thử chế độ thủ công trước khi bật chế độ tự động.

Ví dụ tạo thư mục nhật ký trên Windows bằng PowerShell:

```powershell
New-Item -ItemType Directory -Path ".\logs" -Force
```

Lệnh trên chỉ tạo thư mục lưu nhật ký trên máy tính, không tự cài đặt firmware cho thiết bị IoT.

## 3. ✅ Tính năng

- [x] Thu thập dữ liệu nhiệt độ và độ ẩm đất.
- [x] Xử lý dữ liệu theo ngưỡng cấu hình.
- [x] Hỗ trợ điều khiển máy bơm hoặc van tưới.
- [x] Theo dõi trạng thái thiết bị.
- [ ] Kiểm thử cảnh báo khi cảm biến mất kết nối.
- [ ] Kiểm thử chế độ an toàn khi mất mạng.

## 4. 📊 Bảng thông số hệ thống

| Thành phần | Vai trò | Dữ liệu |
|---|---|---|
| Cảm biến nhiệt độ | Đo nhiệt độ môi trường | Nhiệt độ |
| Cảm biến độ ẩm đất | Đo độ ẩm đất | Độ ẩm |
| Bộ điều khiển ESP32 (ví dụ) | Xử lý dữ liệu | Dữ liệu cảm biến |
| Mạch đóng cắt | Điều khiển tải | Tín hiệu bật/tắt |
| Máy bơm hoặc van | Thực hiện tưới | Lưu lượng nước |
| Nhật ký | Ghi nhận sự kiện | Trạng thái và lỗi |

### Mô hình IPO

| Giai đoạn | Ví dụ |
|---|---|
| **Input — Đầu vào** | Nhiệt độ, độ ẩm đất |
| **Process — Xử lý** | Kiểm tra dữ liệu, so sánh ngưỡng |
| **Output — Đầu ra** | Bật/tắt bơm, cập nhật cảnh báo |

## 5. 🧠 Chu kỳ Fetch–Decode–Execute

> **Fetch — Nạp lệnh:** CPU lấy lệnh tiếp theo từ bộ nhớ.
>
> **Decode — Giải mã:** CPU xác định lệnh cần thực hiện.
>
> **Execute — Thực thi:** CPU thực hiện thao tác theo lệnh. Trong Smart-Farm, chương trình có thể sử dụng các thao tác này để đọc dữ liệu, kiểm tra ngưỡng và cập nhật tín hiệu điều khiển.

*Fetch–Decode–Execute mô tả chu kỳ xử lý lệnh của CPU; IPO mô tả luồng đầu vào, xử lý và đầu ra của hệ thống.*

## 6. 🌱 Mô hình DIKW

DIKW mô tả quá trình chuyển từ dữ liệu sang thông tin, tri thức và quyết định.

```text
          /-------------------\
         /       WISDOM        \
        /   Quyết định tưới     \
       /------------------------\
      /       KNOWLEDGE          \
     /  Quy tắc tưới và an toàn   \
    /-----------------------------\
   /        INFORMATION            \
  /  Hiểu tình trạng môi trường     \
 /----------------------------------\
/               DATA                 \
\         Số đo cảm biến              /
 \----------------------------------/
```

1. **Data — Dữ liệu:** Cảm biến ghi nhận nhiệt độ và độ ẩm đất.
2. **Information — Thông tin:** Hệ thống kết hợp các số đo để xác định tình trạng hiện tại.
3. **Knowledge — Tri thức:** Áp dụng quy tắc tưới đã được xác nhận.
4. **Wisdom — Quyết định:** Tưới, hoãn tưới hoặc cảnh báo người vận hành dựa trên dữ liệu và điều kiện an toàn.

**Ví dụ:** Nhiệt độ cao không có nghĩa chắc chắn cây cần tưới. Hệ thống nên kiểm tra thêm độ ẩm đất và tình trạng nguồn nước trước khi ra quyết định.

## 7. ⚠️ Lưu ý vận hành

### Khi RAM gần đầy do nhật ký

- Giới hạn kích thước nhật ký và sử dụng cơ chế xoay vòng nhật ký.
- Không giữ toàn bộ nhật ký trong RAM nếu không cần thiết.
- Không xóa hàng loạt tệp khi chưa xác minh đường dẫn và nội dung.
- Kiểm tra tiến trình tiêu thụ bộ nhớ và nguyên nhân tăng RAM.
- Nếu bộ điều khiển không còn xử lý tin cậy, chuyển về trạng thái an toàn đã thiết kế và thông báo người vận hành.
- Đảm bảo lỗi ghi nhật ký không vô hiệu hóa cơ chế bảo vệ máy bơm hoặc van.

### Kiểm thử an toàn

1. Giả lập tình trạng RAM gần đầy trong môi trường thử nghiệm.
2. Kiểm tra cảnh báo và mức sử dụng bộ nhớ.
3. Xác minh nhật ký được giới hạn, không tăng vô hạn.
4. Kiểm tra chức năng ngắt bơm/van và các cơ chế bảo vệ.
5. Xác nhận hệ thống hoạt động bình thường sau khi xử lý.

> **An toàn:** Ngắt nguồn trước khi thay đổi dây nối. Thiết bị điện phải được lắp đặt đúng định mức và bởi người có chuyên môn khi cần thiết.

## 8. 📋 Checklist bàn giao

- [ ] Kiểm tra cảm biến và vị trí lắp đặt.
- [ ] Xác nhận ngưỡng tưới.
- [ ] Kiểm tra điều khiển thủ công.
- [ ] Kiểm tra tình huống mất kết nối.
- [ ] Kiểm thử nhật ký và RAM.
- [ ] Hướng dẫn người vận hành xử lý cảnh báo.

---

*Tài liệu mẫu. Cần xác minh cấu hình và thông số thực tế trước khi vận hành tại trang trại Lan.*
