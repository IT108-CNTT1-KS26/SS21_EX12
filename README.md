# BÀI LÀM: ĐẠI TU HỆ THỐNG KIOSK SẠC ĐIỆN TOÀN THÀNH PHỐ

**Thông tin ca trực:**
* Kỹ thuật viên trưởng ca trực NOC
* Thời gian xử lý sự cố khẩn cấp: 25 phút
* Hệ thống: Mạng lưới Kiosk sạc điện VoltCharge (20 trạm toàn thành phố)

---

## Phần 1: Tra cứu Cấp tốc Tài liệu & Hotfix (Lesson 01 & 02)

### 1. Câu lệnh Google Search chuẩn xác để tải tệp `CanBus_Interface.dll`
Cú pháp tìm kiếm tối ưu kết hợp toán tử giới hạn phạm vi tên miền, khớp chính xác và loại trừ rác/phần mềm độc hại:

```text
site:kvaser.com "CanBus_Interface.dll" download -ad -crack -torrent -warez
```

* **Giải thích cấu trúc câu lệnh:**
  * `site:kvaser.com`: Ép kết quả chỉ hiển thị từ máy chủ chính thức của nhà sản xuất phần cứng, tránh tải nhầm mã độc/trojan từ các trang share file lậu.
  * `"CanBus_Interface.dll"`: Khớp chính xác tên tệp thư viện liên kết động đang bị thiếu.
  * `-ad -crack -torrent -warez`: Loại bỏ hoàn toàn các trang SEO lừa đảo tải file DLL, trang crack và quảng cáo gây nhiễu.

---

### 2. Cách tìm nhanh Workaround trên GitHub Issues (Trạng thái OPEN)
Vì đây là bản cập nhật tự động `v2.4.2` mới đẩy xuống gây lỗi thiếu file driver, giải pháp chính thức từ nhà phát triển chưa được phát hành, kỹ thuật viên cần thao tác nhanh:

1. **Bộ lọc tìm kiếm trên GitHub Issues:** Truy cập tab **Issues** của repo thư viện trạm sạc và dán cú pháp lọc:
   ```text
   is:issue is:open "CanBus_Interface.dll" OR "Error 126"
   ```
2. **Xác định mẹo tạm thời (Workaround):**
   * Lướt nhanh xuống các phản hồi và tìm các bình luận có lượng Reaction tích cực cao nhất (nhiều biểu tượng **👍**, **🚀**, **❤️**).
   * Tìm bình luận được đánh dấu hoặc ghim (**Pinned/Marked as answer**) từ Maintainer/Owner của repository.
   * Tìm các đoạn mã (code block) kèm từ khóa *"temporary fix"*, *"bypass check"*, *"workaround"*, hoặc cờ môi trường (ví dụ: `SKIP_CANBUS_CHECK=true` hoặc cờ khởi chạy `--mock-driver`) giúp ứng dụng bỏ qua bước nạp file DLL trong lúc chờ hotfix chính thức.

---

## Phần 2: Ứng dụng Trợ lý AI Giải mã & Xử lý Xung đột (Lesson 03)

### 3. Prompt 3 thành phần gửi Trợ lý AI giải mã lỗi `EACCES`
Soạn prompt theo chuẩn **Vai trò (Role) + Bối cảnh & Lỗi thực tế (Context & Logs) + Yêu cầu hành động cụ thể (Task/Output)**:

> **Prompt:**  
> "Bạn là chuyên gia DevOps và Quản trị hệ thống Windows. Tôi đang vận hành ứng dụng Node.js quản lý kiosk sạc điện trên môi trường Windows 11 và gặp lỗi nghiêm trọng sau:  
> `EACCES: permission denied, open 'C:\VoltLogs\charge.log'`  
> Hãy giải mã ngắn gọn nguyên nhân gốc rễ và cung cấp đúng 3 bước lệnh/thao tác cụ thể nhất để cấp quyền ghi tệp cho dịch vụ ngay lập tức mà không cần khởi động lại máy."

---

### 4. Quy trình 2 dòng lệnh Terminal giải phóng cổng 8080 mà không cần Restart
Mở Command Prompt (cmd) hoặc PowerShell với quyền Administrator và chạy lần lượt:

* **Bước 1: Tìm PID của tiến trình đang chiếm cổng 8080**
  ```cmd
  netstat -ano | findstr :8080
  ```
  *(Quan sát cột cuối cùng của kết quả trả về để lấy số `PID`, ví dụ: `14256`)*

* **Bước 2: Buộc tắt tiến trình bị treo theo PID vừa tìm được**
  ```cmd
  taskkill /PID <PID_VỪA_TÌM> /F
  ```
  *(Thay `<PID_VỪA_TÌM>` bằng số PID thực tế, ví dụ: `taskkill /PID 14256 /F` để giải phóng ngay cổng 8080 cho dịch vụ sạc khởi động lại).*

---

## Phần 3: Chuẩn Hóa Bug Report Khẩn Cấp Gửi Đội Ngũ R&D (Lesson 04)

### 5. Phiếu Báo Cáo Lỗi Khẩn Cấp (Emergency Bug Report)

**Tiêu đề:** `[CRITICAL HOTFIX] Sự cố tê liệt 20 trạm sạc sau bản Auto-update v2.4.2 - Yêu cầu dừng cập nhật & Rollback khẩn cấp`  
**Gửi:** Giám đốc Công nghệ (CTO), Trưởng nhóm R&D / Backend Team  
**Người báo cáo:** Kỹ thuật viên trưởng ca trực NOC  
**Mức độ nghiêm trọng:** P0 (Blocker / Production Down)

---

#### 1. Context (Bối cảnh hệ thống)
* **Thời gian xảy ra:** 17:30, thứ Sáu.
* **Quy mô ảnh hưởng:** Toàn bộ kiosk tại 20 trạm sạc VoltCharge toàn thành phố trong khung giờ cao điểm mưa bão.
* **Tác nhân kích hoạt:** Hệ thống vừa tự động cập nhật lên phiên bản firmware/client `v2.4.2`.
* **Hậu quả vận hành:** Toàn bộ dịch vụ sạc bị treo cứng, hàng trăm phương tiện taxi điện và người dân không thể sạc xe.

#### 2. Expected Behavior (Hành vi kỳ vọng)
* Bản update `v2.4.2` cài đặt ngầm thành công, tự động liên kết đúng driver CAN-Bus sẵn có.
* Tiến trình ghi nhận nhật ký hoạt động bình thường vào thư mục `C:\VoltLogs\`.
* Tiến trình cũ tự động giải phóng socket mạng và khởi chạy lại dịch vụ quản lý trên cổng `8080` trơn tru.

#### 3. Actual Behavior (Hành vi thực tế)
* Máy trạm bật cửa sổ báo lỗi thiếu tệp thư viện phần cứng `CanBus_Interface.dll` khiến quá trình sạc xe không thể kích hoạt.
* Dịch vụ Node.js bị từ chối quyền truy cập ghi log (`EACCES`).
* Cổng mạng `8080` bị giữ bởi tiến trình treo của phiên bản cũ, dẫn đến tiến trình mới bị crash vòng lặp (Crash Loop).

#### 4. Logs & Error Details (Chi tiết Log lỗi)
```text
[Crash Log - Driver Load Failure]
Failed to load driver 'CanBus_Interface.dll'. Error 126: The specified module could not be found.

[File Permission Error]
Error: EACCES: permission denied, open 'C:\VoltLogs\charge.log'
    at Object.openSync (fs.js:498:3)
    at Object.writeFileSync (fs.js:1529:35)

[Network Binding Failure]
events.js:377
throw er; // Unhandled 'error' event
Error: listen EADDRINUSE: address already in use :::8080
```

#### 5. Action Items (Yêu cầu hành động tức thì từ NOC)
1. **DỪNG NGAY LẬP TỨC** toàn bộ cơ chế Auto-update v2.4.2 trên Cloud / OTA Server.
2. Đẩy lệnh **Rollback khẩn cấp về bản v2.4.1** cho toàn bộ máy trạm tại 20 trạm sạc.
3. Bổ sung tệp `CanBus_Interface.dll` vào package cài đặt và cấp quyền thư mục `C:\VoltLogs` trong bản build vá lỗi v2.4.3.
