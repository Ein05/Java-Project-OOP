# Walkthrough: Cài đặt FashionSupplyChainManagementSystem với XAMPP

## Mục tiêu
Chạy project Java JavaFX `FashionSupplyChainManagementSystem` kết nối MySQL thông qua XAMPP,
trong khi máy đã có sẵn MySQL riêng (tránh xung đột port).

---

## Thông tin môi trường

| Thông số | Giá trị |
|---|---|
| XAMPP cài tại | `D:\App\xampp\` |
| MySQL gốc (port mặc định) | `3306` |
| XAMPP MySQL (sau khi đổi) | `3307` |
| Project Java | `e:\Phen\OOP\FashionSupplyChainManagementSystem\` |
| Database cần tạo | `fashion_db` |

---

## Các thay đổi đã thực hiện

### 1. `D:\App\xampp\mysql\bin\my.ini` — Sửa port + đường dẫn

**Vấn đề:** File `my.ini` có 2 lỗi nghiêm trọng:
- Port mặc định `3306` xung đột với MySQL gốc đang chạy trên máy
- Toàn bộ đường dẫn trỏ sai vào `C:/xampp/...` trong khi XAMPP thực sự nằm ở `D:/App/xampp/`

**Các chỉnh sửa:**

```diff
 [client]
-port=3306
+port=3307
-socket="C:/xampp/mysql/mysql.sock"
+socket="D:/App/xampp/mysql/mysql.sock"

 [mysqld]
-port=3306
+port=3307
-socket="C:/xampp/mysql/mysql.sock"
-basedir="C:/xampp/mysql"
-tmpdir="C:/xampp/tmp"
-datadir="C:/xampp/mysql/data"
+socket="D:/App/xampp/mysql/mysql.sock"
+basedir="D:/App/xampp/mysql"
+tmpdir="D:/App/xampp/tmp"
+datadir="D:/App/xampp/mysql/data"

-plugin_dir="C:/xampp/mysql/lib/plugin/"
+plugin_dir="D:/App/xampp/mysql/lib/plugin/"

-innodb_data_home_dir="C:/xampp/mysql/data"
-innodb_log_group_home_dir="C:/xampp/mysql/data"
+innodb_data_home_dir="D:/App/xampp/mysql/data"
+innodb_log_group_home_dir="D:/App/xampp/mysql/data"
```

---

### 2. `database.java` — Sửa port kết nối

**File:** `e:\Phen\OOP\FashionSupplyChainManagementSystem\fashionSupplyChainManagementSystem\src\fashionsupplychainmanagementsystem\database.java`

**Vấn đề:** Code Java kết nối mặc định vào port 3306, nhưng XAMPP MySQL đã được đổi sang 3307.

```diff
-Connection connect = DriverManager.getConnection("jdbc:mysql://localhost/cafe", "root", "");
+Connection connect = DriverManager.getConnection("jdbc:mysql://localhost:3307/cafe", "root", "");
```

---

### 3. File log bị treo — Đã xóa thủ công

Các file sau được xóa để tránh MySQL bị treo khi khởi động do log cũ không tương thích:

- `D:\App\xampp\mysql\data\ib_logfile0` ✅ Đã xóa
- `D:\App\xampp\mysql\data\ib_logfile1` ✅ Đã xóa
- `D:\App\xampp\mysql\data\mysql.pid` ✅ Đã xóa

> [!NOTE]
> Các file `ib_logfile*` sẽ được MySQL tự tạo lại khi khởi động lần tiếp theo. Không cần lo lắng.

---

### 4. Chuyển đổi từ Cafe Shop sang Fashion Supply Chain Management

Để biến ứng dụng thành hệ thống quản lý chuỗi cung ứng thời trang, các thay đổi sau đã được thực hiện so với mã nguồn gốc:

- **Giao diện & Nhãn hiển thị (`mainForm.fxml` và `FXMLDocument.fxml`):**
  - Đổi tiêu đề ứng dụng từ `"Cafe Shop Management System"` thành `"Fashion Supply Chain"`.
  - Thay đổi biểu tượng cốc cà phê (`COFFEE`) ở màn hình đăng nhập thành túi mua sắm (`SHOPPING_BAG`).
  - Đổi tên các nút điều hướng bên trái: `"Menu"` $\rightarrow$ `"Orders"`, `"Customers"` $\rightarrow$ `"Sales History"`.
  - Đổi tên các nhãn trong Dashboard: `"Number of Customer"` $\rightarrow$ `"Today's Orders"`, `"Number of Sold Products"` $\rightarrow$ `"Items Sold"`, `"Income's Chart"` $\rightarrow$ `"Revenue Chart"`, `"Customer's Chart"` $\rightarrow$ `"Orders Chart"`.
  - Đổi tên các cột và nhãn quản lý sản phẩm: `"Product ID"` $\rightarrow$ `"Item ID"`, `"Product Name"` $\rightarrow$ `"Item Name"`, `"Type"` $\rightarrow$ `"Category"`.

- **Mở rộng tính năng sản phẩm (Size & Color):**
  - **Dữ liệu (`productData.java`):** Thêm thuộc tính `size` (Kích thước) và `color` (Màu sắc), mở rộng constructor và cung cấp các phương thức getter tương ứng.
  - **Giao diện (`mainForm.fxml`):** Thêm ComboBox `inventory_size` và TextField `inventory_color` vào form quản lý sản phẩm. Bổ sung 2 cột hiển thị kích thước và màu sắc vào bảng TableView sản phẩm.
  - **Xử lý logic (`mainFormController.java`):**
    - Khai báo và kết nối các thành phần FXML mới (`inventory_size`, `inventory_color`, `inventory_col_size`, `inventory_col_color`).
    - Cập nhật danh sách loại sản phẩm thời trang (`typeList`) gồm: `Tops`, `Bottoms`, `Dresses`, `Footwear`, `Accessories`.
    - Định nghĩa danh sách size (`sizeList`) gồm: `S`, `M`, `L`, `XL`, `XXL` và tải lên ComboBox khi mở tab Inventory.
    - Cập nhật hàm `inventoryAddBtn()` và `inventoryUpdateBtn()` để ghi nhận, xác thực và lưu thêm thông tin Size và Color vào CSDL.
    - Cập nhật hàm hiển thị bảng và chọn sản phẩm để hiển thị/điền tự động Size và Color.

- **Thay đổi CSS Theme (`mainFormDesign.css` & `loginDesign.css`):**
  - Thay đổi toàn bộ bảng màu từ tông cam/nâu của quán cà phê sang tông màu thời trang cao cấp: Slate Dark (`#0f172a`, `#1e1b4b`) phối cùng Indigo Violet (`#4f46e5`, `#6366f1`).

---

## Database Schema (MySQL)

Truy cập **http://localhost/phpmyadmin**, tạo database tên `fashion_db`, sau đó chạy SQL script tương ứng bên dưới:

### Trường hợp 1: Tạo mới cơ sở dữ liệu từ đầu
Chạy script SQL sau để tạo tất cả các bảng:

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

### Trường hợp 2: Nếu đã tạo database của CafeShop từ trước
Nếu bạn đã tạo cơ sở dữ liệu ở phiên làm việc trước, chỉ cần chạy câu lệnh SQL sau để cập nhật cấu trúc bảng `product`:

```sql
ALTER TABLE product ADD COLUMN size VARCHAR(20) AFTER date;
ALTER TABLE product ADD COLUMN color VARCHAR(30) AFTER size;
```

---

## Hướng dẫn chạy project

1. **Mở XAMPP Control Panel** → Start **MySQL** (port 3307)
2. **Truy cập phpMyAdmin**: http://localhost/phpmyadmin → Tạo database `fashion_db` + các bảng (SQL trên)
3. **Mở dự án trên VS Code hoặc NetBeans**:
   - **Với VS Code (Khuyên dùng)**:
     1. Mở thư mục `fashionSupplyChainManagementSystem` trong VS Code.
     2. Cài đặt extension **Extension Pack for Java**.
     3. Cuộn xuống tìm mục **Java Projects** ở sidebar bên trái, chọn **Referenced Libraries**, nhấp chuột vào dấu `+` và chọn toàn bộ các file `.jar` từ thư mục `dist/lib/` để liên kết thư viện.
     4. Mở file `src/fashionsupplychainmanagementsystem/FashionSupplyChainManagementSystem.java` và click nút **Run** hiện ra phía trên hàm `main` (hoặc nhấn **F5**).
   - **Với NetBeans**:
     1. Chọn **File** ➔ **Open Project** ➔ chọn thư mục `fashionSupplyChainManagementSystem`.
     2. Đảm bảo các JAR trong `dist/lib/` được thêm vào Libraries của project.
     3. Chọn **Clean & Build** để biên dịch lại.
     4. Nhấn **F6** để chạy.
4. Đăng ký tài khoản mới rồi đăng nhập sử dụng.

---

## Lưu ý quan trọng

> [!IMPORTANT]
> Project này yêu cầu **JDK 8** vì dùng JavaFX tích hợp sẵn. JDK 11+ đã tách JavaFX ra riêng và sẽ gây lỗi khi build.

> [!WARNING]
> Nếu xóa nhầm file `ibdata1` (không phải `ib_logfile`), toàn bộ data trong XAMPP MySQL sẽ mất. Chỉ xóa `ib_logfile0` và `ib_logfile1`.

| MySQL gốc | XAMPP MySQL |
|---|---|
| Port **3306** — không thay đổi | Port **3307** — dùng cho project này |
| Quản lý bởi Windows Service | Quản lý bởi XAMPP Control Panel |
| Không bị ảnh hưởng | ✅ |
