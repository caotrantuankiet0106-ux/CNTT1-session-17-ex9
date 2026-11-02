# BÁO CÁO: TỐI ƯU HÓA KHÔNG GIAN BÀN TRÀM KHÁM CAREPULSE

---

## I. MÔ HÌNH ĐA PROFILE (DATA ISOLATION)

### 1. Nguyên nhân gây "văng phiên đăng nhập" (Session Conflict)
* **Dùng chung User Data Directory:** Nhiều tài khoản lưu chung Cookies, Cache tại `AppData\...\Chrome\User Data\Default`.
* **Ghi đè Auth Token:** Việc truy cập các trang cá nhân (PTIT, GitHub) gửi Cookie mới, gây ghi đè và làm vô hiệu hóa Session Token/JWT của CarePulse.
* **Xung đột Cloud Sync:** Tài khoản Google cá nhân đồng bộ đè dữ liệu làm mất hiệu lực phiên làm việc y tế.

### 2. Thiết kế Mô hình 3 Profile Chuyên Biệt

| Tiêu chí | Work (Chuyên môn Y tế) | Personal (Hành chính) | Sandbox / Test (Thực tập) |
| :--- | :--- | :--- | :--- |
| **Đối tượng** | Bác sĩ trực chính, Điều dưỡng | Nhân viên trạm | Sinh viên, Bác sĩ thực tập |
| **Mục đích** | Thao tác CarePulse, Cổng BHYT | Tra cứu hành chính, thư công | Học tập (PTIT, GitHub), test tính năng |
| **Nhận diện** | **Xanh dương (Medical Blue)** | **Xanh lá / Xám neutral** | **Cam sáng / Đỏ cảnh báo** |
| **Dữ liệu** | Cô lập 100% Cookies/Session y tế | Độc lập hoàn toàn | Tự động dọn dẹp, không lưu thông tin |
| **Extension** | Tiện ích gõ tiếng Việt, bảo mật | Tiện ích văn phòng | Không cài extension lạ |

---

## II. KIẾN TRÚC BOOKMARK VÀ QUY TRÌNH AN TOÀN

### 1. Cây Thư Mục Bookmark Chuẩn (Ctrl + Shift + O)
* 📁 **01_CarePulse_Clinic:** CarePulse Tiếp Nhận | CarePulse Kê Đơn | Cổng Tra Cứu BHYT
* 📁 **02_Pharma_Portal:** Dược Thư Quốc Gia | Danh Mục Thuốc BHYT | CSDL Dược Quốc Gia
* 📁 **03_Medical_Tools:** Google Dịch Y Tế | Công Cụ BMI/eGFR | Telemedicine Tỉnh/TW

### 2. Kỹ Thuật Favicon-Only (Tiết kiệm không gian x3)
* **Thao tác:** Chuột phải Bookmark $\rightarrow$ **Edit** $\rightarrow$ Xóa toàn bộ tên trong ô **Name** $\rightarrow$ **Save**.
* **Hiệu quả:** Chỉ hiển thị logo (Favicon), chứa hơn 30 liên kết không lo tràn màn hình, nhận diện trực quan.

### 3. Quy Trình An Toàn & Cảnh Báo
* **Tình huống kiểm tra 30 phút:** Kích hoạt **Guest Profile (Chế độ Khách)**. Đóng cửa sổ sẽ tự động dọn sạch mọi dữ liệu, không ảnh hưởng đến Profile chính (khác với Incognito vẫn dùng chung bộ khung Profile).
* **Rủi ro Cloud Sync cá nhân:** 
  1. Đồng bộ mật khẩu, lịch sử cá nhân lên máy dùng chung.
  2. Dễ bị chiếm đoạt tài khoản Google cá nhân.
  3. Trộn lẫn Bookmark và Extension cá nhân vào không gian y tế.