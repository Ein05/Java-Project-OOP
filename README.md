# Fashion Supply Chain Management System (JavaFX)

Hệ thống Quản lý Chuỗi Cung ứng Thời trang được chuyển đổi và nâng cấp từ dự án gốc **CafeShopManagementSystem**. 

Dự án này sử dụng khung JavaFX và kết nối cơ sở dữ liệu MySQL, được thiết kế lại giao diện tối cao cấp (Slate Dark & Indigo Violet) và bổ sung các tính năng quản lý quần áo chuyên sâu (như lựa chọn **Kích thước/Size**, **Màu sắc/Color** và phân loại ngành hàng thời trang).

---

## 🚀 Hướng dẫn Cài đặt & Khởi chạy dự án

### Bước 1: Chuẩn bị Môi trường
1. **JDK 8 (Java SE Development Kit 8):** Dự án yêu cầu JDK 8 vì đây là phiên bản Java tích hợp sẵn thư viện JavaFX. Biên dịch bằng JDK cao hơn (11+) sẽ bị lỗi trừ khi bạn cấu hình thư viện JavaFX rời.
2. **VS Code** (hoặc **NetBeans IDE**): Trình biên tập và chạy mã nguồn.
3. **XAMPP:** Dùng để khởi tạo MySQL Server.

---

### Bước 2: Cấu hình XAMPP MySQL tránh xung đột
Nếu máy tính của bạn đã cài sẵn một máy chủ MySQL gốc (chạy trên cổng mặc định `3306`), hãy cấu hình MySQL của XAMPP chạy trên cổng `3307` để tránh xung đột theo hướng dẫn sau:

1. Mở file cấu hình `my.ini` trong XAMPP (đường dẫn thường gặp: `[Thư mục cài XAMPP]\mysql\bin\my.ini`).
2. Tìm và sửa tất cả các giá trị cổng `port=3306` thành `port=3307`.
3. Sửa các đường dẫn `C:/xampp/...` trỏ về đúng thư mục cài đặt thực tế của XAMPP trên máy của bạn (ví dụ: `D:/App/xampp/...`).
4. Khởi động MySQL từ **XAMPP Control Panel**.

---

### Bước 3: Thiết lập Cơ sở dữ liệu (MySQL)
1. Truy cập **phpMyAdmin** thông qua trình duyệt: [http://localhost/phpmyadmin](http://localhost/phpmyadmin).
2. Tạo mới một cơ sở dữ liệu có tên là: `fashion_db`.
3. Chọn database `fashion_db`, chọn tab **SQL**, sao chép đoạn script dưới đây và nhấn **Go** để tạo các bảng:

```sql
CREATE TABLE employee (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    password VARCHAR(50) NOT NULL,
    question VARCHAR(100),
    answer VARCHAR(100),
    date DATE
);

CREATE TABLE product (
    id INT AUTO_INCREMENT PRIMARY KEY,
    prod_id VARCHAR(20) NOT NULL,
    prod_name VARCHAR(100) NOT NULL,
    type VARCHAR(50),
    stock INT,
    price DOUBLE,
    status VARCHAR(20),
    image VARCHAR(500),
    date DATE,
    size VARCHAR(20),
    color VARCHAR(30)
);

CREATE TABLE customer (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    prod_id VARCHAR(20) NOT NULL,
    prod_name VARCHAR(100) NOT NULL,
    type VARCHAR(50),
    quantity INT,
    price DOUBLE,
    date DATE,
    image VARCHAR(500),
    em_username VARCHAR(50)
);

CREATE TABLE receipt (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    total DOUBLE NOT NULL,
    date DATE,
    em_username VARCHAR(50)
);
```

---

### Bước 4: Khởi chạy dự án

#### Lựa chọn A: Sử dụng VS Code (Khuyên dùng)
1. Mở thư mục dự án `fashionSupplyChainManagementSystem` trong **VS Code**.
2. Cài đặt bộ công cụ extension **Extension Pack for Java** (của Microsoft).
3. Cấu hình JDK 8 làm Runtime mặc định cho VS Code.
4. Liên kết các thư viện hỗ trợ (Referenced Libraries):
   - Ở thanh Sidebar bên trái của VS Code, cuộn xuống tìm mục **Java Projects**.
   - Tìm đến mục **Referenced Libraries**, nhấp chuột vào dấu **`+`** và chọn toàn bộ các tệp thư viện `.jar` nằm trong thư mục `dist/lib/` để tích hợp.
5. Mở tệp chạy chính: `src/fashionsupplychainmanagementsystem/FashionSupplyChainManagementSystem.java`.
6. Click vào dòng chữ **Run** hiện ra ở ngay phía trên hàm `main` (hoặc nhấn **F5**) để chạy ứng dụng.

#### Lựa chọn B: Sử dụng NetBeans IDE
1. Mở **NetBeans IDE**.
2. Chọn **File** ➔ **Open Project** và chọn thư mục:
   `fashionSupplyChainManagementSystem` (nằm bên trong thư mục dự án của bạn).
3. Đảm bảo tất cả các file thư viện `.jar` trong thư mục `dist/lib/` đã được thêm đầy đủ vào mục **Libraries** của dự án trong NetBeans.
4. Nhấp chuột phải vào dự án và chọn **Clean & Build** để biên dịch lại mã nguồn theo cấu trúc package mới.
5. Nhấn **F6** (hoặc chọn Run) để khởi chạy ứng dụng. Đăng ký tài khoản mới ở màn hình Login và đăng nhập để sử dụng.

---

## 🛠️ Danh sách các thay đổi so với Repository gốc

Dự án này đã được refactor sâu sắc so với repo gốc `CafeShop` để phù hợp với đề tài Quản lý Chuỗi cung ứng Thời trang:

1. **Giao diện thời trang cao cấp:** CSS được viết lại toàn bộ ([mainFormDesign.css](fashionSupplyChainManagementSystem/src/fashionsupplychainmanagementsystem/mainFormDesign.css), [loginDesign.css](fashionSupplyChainManagementSystem/src/fashionsupplychainmanagementsystem/loginDesign.css)) đổi từ màu cam/nâu cafe sang tông màu tối lịch lãm kết hợp cùng màu tím Indigo của các nhãn hàng thời trang cao cấp.
2. **Bổ sung thuộc tính Size & Color:** Tích hợp trường chọn kích thước (Size) và màu sắc (Color) vào luồng quản lý sản phẩm từ database, lớp dữ liệu Java ([productData.java](fashionSupplyChainManagementSystem/src/fashionsupplychainmanagementsystem/productData.java)), form nhập liệu cho đến bảng hiển thị TableView.
3. **Phân loại hàng hóa:** Chuyển đổi danh mục ẩm thực (`Meals`, `Drinks`) sang các ngành hàng thời trang: `Tops` (Áo), `Bottoms` (Quần/Chân váy), `Dresses` (Váy liền), `Footwear` (Giày dép), `Accessories` (Phụ kiện).
4. **Việt hóa tài liệu:** Bổ sung file hướng dẫn chạy dự án chi tiết bằng tiếng Việt ([SETUP_GUIDE.md](SETUP_GUIDE.md)) giúp quá trình cài đặt cơ sở dữ liệu và cấu hình XAMPP cổng 3307 diễn ra mượt mà nhất.
5. **An toàn bảo mật:** Cấu hình kết nối cơ sở dữ liệu sử dụng tài khoản cục bộ mặc định và không lưu trữ thông tin nhạy cảm của hệ thống, hoàn toàn an toàn khi đẩy mã nguồn lên GitHub.
