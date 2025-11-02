# DATABASE SCHEMA - HỆ THỐNG QUẢN LÝ THƯ VIỆN

## 📊 ERD (Entity Relationship Diagram)

```
┌─────────────┐         ┌──────────────┐         ┌─────────────┐
│    User     │────────▶│   Profile    │         │    Role     │
│             │         │              │         │             │
│ - id        │         │ - user_id    │         │ - id        │
│ - username  │         │ - phone      │         │ - name      │
│ - email     │         │ - avatar     │         │ - code      │
│ - password  │         │ - address    │         └─────────────┘
│ - role_id   │─────────│ - reader_type│               │
└─────────────┘         └──────────────┘               │
       │                                                │
       │                                                │
       └────────────────────────────────────────────────┘

┌─────────────┐         ┌──────────────┐         ┌─────────────┐
│    Book     │────────▶│   Category   │         │   Author    │
│             │         │              │         │             │
│ - id        │         │ - id         │         │ - id        │
│ - title     │         │ - name       │         │ - name      │
│ - isbn      │         │ - code       │         │ - bio       │
│ - quantity  │         │ - parent_id  │         └─────────────┘
│ - available │         └──────────────┘               │
│ - category  │────┐                                   │
│ - author_id │────┼───────────────────────────────────┘
│ - publisher │    │
│ - location  │    │    ┌──────────────┐
│ - qr_code   │    └───▶│  Publisher   │
└─────────────┘         │              │
       │                │ - id         │
       │                │ - name       │
       │                │ - address    │
       │                └──────────────┘
       │
       │
       ▼
┌─────────────────┐     ┌──────────────────┐
│ BorrowRecord    │────▶│ BorrowDetail     │
│                 │     │                  │
│ - id            │     │ - id             │
│ - reader_id     │     │ - borrow_id      │
│ - librarian_id  │     │ - book_id        │
│ - borrow_date   │     │ - due_date       │
│ - return_date   │     │ - return_date    │
│ - status        │     │ - fine           │
│ - total_fine    │     │ - status         │
└─────────────────┘     │ - extension_count│
       │                └──────────────────┘
       │
       ▼
┌─────────────────┐
│   Violation     │
│                 │
│ - id            │
│ - reader_id     │
│ - borrow_detail │
│ - type          │
│ - fine_amount   │
│ - description   │
│ - resolved      │
└─────────────────┘

┌─────────────────┐     ┌──────────────────┐
│  Notification   │     │   SystemConfig   │
│                 │     │                  │
│ - id            │     │ - id             │
│ - user_id       │     │ - key            │
│ - title         │     │ - value          │
│ - message       │     │ - description    │
│ - type          │     │ - data_type      │
│ - read          │     └──────────────────┘
│ - created_at    │
└─────────────────┘

┌─────────────────┐     ┌──────────────────┐
│   ActivityLog   │     │   IoTDevice      │
│                 │     │                  │
│ - id            │     │ - id             │
│ - user_id       │     │ - device_code    │
│ - action        │     │ - name           │
│ - model         │     │ - location       │
│ - object_id     │     │ - ip_address     │
│ - changes       │     │ - status         │
│ - ip_address    │     │ - last_active    │
│ - timestamp     │     └──────────────────┘
└─────────────────┘
```

---

## 📋 CHI TIẾT CÁC BẢNG

### 1. **auth_user** (Django default - extended)
Bảng người dùng mặc định của Django

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | INT | PK, AUTO_INCREMENT | ID người dùng |
| username | VARCHAR(150) | UNIQUE, NOT NULL | Tên đăng nhập |
| email | VARCHAR(254) | UNIQUE, NOT NULL | Email |
| password | VARCHAR(128) | NOT NULL | Mật khẩu (hashed) |
| first_name | VARCHAR(150) | NULL | Tên |
| last_name | VARCHAR(150) | NULL | Họ |
| is_active | BOOLEAN | DEFAULT TRUE | Trạng thái hoạt động |
| is_staff | BOOLEAN | DEFAULT FALSE | Là nhân viên |
| is_superuser | BOOLEAN | DEFAULT FALSE | Là quản trị viên |
| date_joined | DATETIME | NOT NULL | Ngày tham gia |
| last_login | DATETIME | NULL | Lần đăng nhập cuối |

---

### 2. **users_role**
Bảng phân quyền người dùng

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | INT | PK, AUTO_INCREMENT | ID role |
| name | VARCHAR(50) | NOT NULL | Tên quyền (VN) |
| code | VARCHAR(20) | UNIQUE, NOT NULL | Mã quyền (ADMIN/LIBRARIAN/READER) |
| description | TEXT | NULL | Mô tả quyền |
| created_at | DATETIME | AUTO | Ngày tạo |
| updated_at | DATETIME | AUTO | Ngày cập nhật |

**Data mẫu:**
```sql
INSERT INTO users_role (name, code, description) VALUES
('Quản trị viên', 'ADMIN', 'Toàn quyền hệ thống'),
('Thủ thư', 'LIBRARIAN', 'Quản lý sách và mượn trả'),
('Độc giả', 'READER', 'Người đọc sách');
```

---

### 3. **users_profile**
Bảng thông tin mở rộng người dùng

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | INT | PK, AUTO_INCREMENT | ID profile |
| user_id | INT | FK→auth_user.id, UNIQUE | ID người dùng |
| role_id | INT | FK→users_role.id | ID quyền |
| reader_type | VARCHAR(20) | NULL | Loại độc giả (STUDENT/STAFF) |
| phone | VARCHAR(20) | NULL | Số điện thoại |
| avatar | VARCHAR(255) | NULL | Đường dẫn ảnh đại diện |
| address | TEXT | NULL | Địa chỉ |
| student_id | VARCHAR(20) | NULL | Mã sinh viên/CBNV |
| faculty | VARCHAR(100) | NULL | Khoa/Phòng ban |
| date_of_birth | DATE | NULL | Ngày sinh |
| created_at | DATETIME | AUTO | Ngày tạo |
| updated_at | DATETIME | AUTO | Ngày cập nhật |

**Index:**
- `idx_user_id` (user_id)
- `idx_reader_type` (reader_type)

---

### 4. **books_category**
Bảng thể loại sách (hỗ trợ cây phân cấp)

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | INT | PK, AUTO_INCREMENT | ID thể loại |
| name | VARCHAR(100) | NOT NULL | Tên thể loại |
| code | VARCHAR(50) | UNIQUE, NOT NULL | Mã thể loại |
| parent_id | INT | FK→books_category.id, NULL | ID thể loại cha |
| description | TEXT | NULL | Mô tả |
| created_at | DATETIME | AUTO | Ngày tạo |
| updated_at | DATETIME | AUTO | Ngày cập nhật |

**Index:**
- `idx_parent_id` (parent_id)

---

### 5. **books_author**
Bảng tác giả

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | INT | PK, AUTO_INCREMENT | ID tác giả |
| name | VARCHAR(200) | NOT NULL | Tên tác giả |
| bio | TEXT | NULL | Tiểu sử |
| nationality | VARCHAR(100) | NULL | Quốc tịch |
| created_at | DATETIME | AUTO | Ngày tạo |
| updated_at | DATETIME | AUTO | Ngày cập nhật |

---

### 6. **books_publisher**
Bảng nhà xuất bản

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | INT | PK, AUTO_INCREMENT | ID NXB |
| name | VARCHAR(200) | NOT NULL | Tên NXB |
| address | TEXT | NULL | Địa chỉ |
| phone | VARCHAR(20) | NULL | Số điện thoại |
| email | VARCHAR(100) | NULL | Email |
| website | VARCHAR(200) | NULL | Website |
| created_at | DATETIME | AUTO | Ngày tạo |
| updated_at | DATETIME | AUTO | Ngày cập nhật |

---

### 7. **books_book**
Bảng sách

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | INT | PK, AUTO_INCREMENT | ID sách |
| title | VARCHAR(255) | NOT NULL | Tên sách |
| isbn | VARCHAR(20) | UNIQUE, NULL | Mã ISBN |
| author_id | INT | FK→books_author.id | ID tác giả |
| publisher_id | INT | FK→books_publisher.id | ID NXB |
| category_id | INT | FK→books_category.id | ID thể loại |
| publication_year | INT | NULL | Năm xuất bản |
| language | VARCHAR(50) | DEFAULT 'Vietnamese' | Ngôn ngữ |
| pages | INT | NULL | Số trang |
| description | TEXT | NULL | Mô tả |
| cover_image | VARCHAR(255) | NULL | Ảnh bìa |
| quantity | INT | DEFAULT 0 | Số lượng |
| available | INT | DEFAULT 0 | Số lượng khả dụng |
| location_shelf | VARCHAR(50) | NULL | Kệ sách |
| location_row | VARCHAR(50) | NULL | Ngăn sách |
| qr_code | VARCHAR(255) | NULL | Đường dẫn QR code |
| barcode | VARCHAR(50) | NULL | Barcode |
| price | DECIMAL(10,2) | NULL | Giá sách |
| status | VARCHAR(20) | DEFAULT 'ACTIVE' | Trạng thái (ACTIVE/INACTIVE) |
| created_at | DATETIME | AUTO | Ngày tạo |
| updated_at | DATETIME | AUTO | Ngày cập nhật |

**Index:**
- `idx_isbn` (isbn)
- `idx_title` (title)
- `idx_category` (category_id)
- `idx_author` (author_id)
- `idx_barcode` (barcode)

---

### 8. **borrowing_record**
Bảng phiếu mượn sách

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | INT | PK, AUTO_INCREMENT | ID phiếu mượn |
| reader_id | INT | FK→auth_user.id, NOT NULL | ID độc giả |
| librarian_id | INT | FK→auth_user.id, NOT NULL | ID thủ thư tạo phiếu |
| borrow_date | DATETIME | NOT NULL | Ngày mượn |
| due_date | DATE | NOT NULL | Ngày hết hạn |
| return_date | DATETIME | NULL | Ngày trả thực tế |
| status | VARCHAR(20) | DEFAULT 'BORROWED' | Trạng thái (BORROWED/RETURNED/OVERDUE) |
| total_fine | DECIMAL(10,2) | DEFAULT 0 | Tổng tiền phạt |
| notes | TEXT | NULL | Ghi chú |
| created_at | DATETIME | AUTO | Ngày tạo |
| updated_at | DATETIME | AUTO | Ngày cập nhật |

**Index:**
- `idx_reader_id` (reader_id)
- `idx_status` (status)
- `idx_borrow_date` (borrow_date)
- `idx_due_date` (due_date)

---

### 9. **borrowing_detail**
Bảng chi tiết sách trong phiếu mượn

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | INT | PK, AUTO_INCREMENT | ID chi tiết |
| borrow_record_id | INT | FK→borrowing_record.id | ID phiếu mượn |
| book_id | INT | FK→books_book.id | ID sách |
| due_date | DATE | NOT NULL | Ngày hết hạn |
| return_date | DATETIME | NULL | Ngày trả thực tế |
| extension_count | INT | DEFAULT 0 | Số lần gia hạn |
| fine_amount | DECIMAL(10,2) | DEFAULT 0 | Tiền phạt |
| status | VARCHAR(20) | DEFAULT 'BORROWED' | Trạng thái |
| condition_borrow | VARCHAR(50) | NULL | Tình trạng lúc mượn |
| condition_return | VARCHAR(50) | NULL | Tình trạng lúc trả |
| notes | TEXT | NULL | Ghi chú |
| created_at | DATETIME | AUTO | Ngày tạo |
| updated_at | DATETIME | AUTO | Ngày cập nhật |

**Index:**
- `idx_borrow_record` (borrow_record_id)
- `idx_book_id` (book_id)
- `idx_status` (status)

---

### 10. **borrowing_violation**
Bảng vi phạm

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | INT | PK, AUTO_INCREMENT | ID vi phạm |
| reader_id | INT | FK→auth_user.id | ID độc giả |
| borrow_detail_id | INT | FK→borrowing_detail.id, NULL | ID chi tiết mượn |
| violation_type | VARCHAR(50) | NOT NULL | Loại vi phạm |
| description | TEXT | NULL | Mô tả |
| fine_amount | DECIMAL(10,2) | DEFAULT 0 | Số tiền phạt |
| paid_amount | DECIMAL(10,2) | DEFAULT 0 | Số tiền đã đóng |
| is_resolved | BOOLEAN | DEFAULT FALSE | Đã xử lý |
| resolved_by | INT | FK→auth_user.id, NULL | Người xử lý |
| resolved_date | DATETIME | NULL | Ngày xử lý |
| notes | TEXT | NULL | Ghi chú |
| created_at | DATETIME | AUTO | Ngày tạo |
| updated_at | DATETIME | AUTO | Ngày cập nhật |

**Loại vi phạm:**
- `OVERDUE` - Trả trễ
- `DAMAGED` - Làm hỏng sách
- `LOST` - Mất sách

**Index:**
- `idx_reader_id` (reader_id)
- `idx_resolved` (is_resolved)

---

### 11. **notifications_notification**
Bảng thông báo

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | INT | PK, AUTO_INCREMENT | ID thông báo |
| user_id | INT | FK→auth_user.id | ID người nhận |
| title | VARCHAR(255) | NOT NULL | Tiêu đề |
| message | TEXT | NOT NULL | Nội dung |
| notification_type | VARCHAR(50) | NOT NULL | Loại thông báo |
| is_read | BOOLEAN | DEFAULT FALSE | Đã đọc |
| link | VARCHAR(255) | NULL | Link liên quan |
| created_at | DATETIME | AUTO | Ngày tạo |

**Loại thông báo:**
- `DUE_SOON` - Sắp đến hạn
- `OVERDUE` - Quá hạn
- `RETURNED` - Đã trả
- `APPROVED` - Đã duyệt
- `SYSTEM` - Hệ thống

**Index:**
- `idx_user_id` (user_id)
- `idx_is_read` (is_read)
- `idx_created` (created_at)

---

### 12. **system_config**
Bảng cấu hình hệ thống

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | INT | PK, AUTO_INCREMENT | ID cấu hình |
| key | VARCHAR(100) | UNIQUE, NOT NULL | Key cấu hình |
| value | TEXT | NOT NULL | Giá trị |
| description | TEXT | NULL | Mô tả |
| data_type | VARCHAR(20) | DEFAULT 'string' | Kiểu dữ liệu |
| created_at | DATETIME | AUTO | Ngày tạo |
| updated_at | DATETIME | AUTO | Ngày cập nhật |

**Data mẫu:**
```sql
INSERT INTO system_config (key, value, description, data_type) VALUES
('MAX_BORROW_BOOKS_STUDENT', '5', 'Số sách tối đa sinh viên mượn', 'integer'),
('MAX_BORROW_BOOKS_STAFF', '10', 'Số sách tối đa giảng viên mượn', 'integer'),
('BORROW_DURATION_DAYS', '14', 'Số ngày mượn sách', 'integer'),
('MAX_EXTENSION_COUNT', '2', 'Số lần gia hạn tối đa', 'integer'),
('FINE_PER_DAY', '5000', 'Phí phạt mỗi ngày (VNĐ)', 'decimal'),
('REMINDER_BEFORE_DUE_DAYS', '3', 'Nhắc nhở trước hạn (ngày)', 'integer'),
('LIBRARY_NAME', 'Thư viện Trường XYZ', 'Tên thư viện', 'string'),
('LIBRARY_EMAIL', 'library@example.com', 'Email thư viện', 'string');
```

---

### 13. **activity_log**
Bảng log hoạt động

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | INT | PK, AUTO_INCREMENT | ID log |
| user_id | INT | FK→auth_user.id, NULL | ID người thực hiện |
| action | VARCHAR(50) | NOT NULL | Hành động |
| model_name | VARCHAR(100) | NULL | Tên model |
| object_id | INT | NULL | ID object |
| changes | JSON | NULL | Thay đổi (JSON) |
| ip_address | VARCHAR(45) | NULL | IP address |
| user_agent | TEXT | NULL | User agent |
| timestamp | DATETIME | AUTO | Thời gian |

**Actions:**
- `CREATE`, `UPDATE`, `DELETE`, `LOGIN`, `LOGOUT`, `BORROW`, `RETURN`, `EXTEND`

**Index:**
- `idx_user_id` (user_id)
- `idx_timestamp` (timestamp)
- `idx_action` (action)

---

### 14. **iot_device**
Bảng thiết bị IoT

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | INT | PK, AUTO_INCREMENT | ID thiết bị |
| device_code | VARCHAR(50) | UNIQUE, NOT NULL | Mã thiết bị |
| name | VARCHAR(100) | NOT NULL | Tên thiết bị |
| location_shelf | VARCHAR(50) | NULL | Kệ sách |
| location_row | VARCHAR(50) | NULL | Ngăn sách |
| ip_address | VARCHAR(45) | NULL | Địa chỉ IP |
| mac_address | VARCHAR(17) | NULL | Địa chỉ MAC |
| device_type | VARCHAR(50) | DEFAULT 'ESP8266' | Loại thiết bị |
| status | VARCHAR(20) | DEFAULT 'INACTIVE' | Trạng thái |
| last_active | DATETIME | NULL | Lần hoạt động cuối |
| firmware_version | VARCHAR(20) | NULL | Phiên bản firmware |
| created_at | DATETIME | AUTO | Ngày tạo |
| updated_at | DATETIME | AUTO | Ngày cập nhật |

**Index:**
- `idx_device_code` (device_code)
- `idx_location` (location_shelf, location_row)

---

### 15. **iot_command**
Bảng lệnh IoT

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | INT | PK, AUTO_INCREMENT | ID lệnh |
| device_id | INT | FK→iot_device.id | ID thiết bị |
| command | VARCHAR(50) | NOT NULL | Lệnh (LED_ON/LED_OFF) |
| book_id | INT | FK→books_book.id, NULL | ID sách |
| requested_by | INT | FK→auth_user.id | Người yêu cầu |
| status | VARCHAR(20) | DEFAULT 'PENDING' | Trạng thái |
| executed_at | DATETIME | NULL | Thời gian thực thi |
| feedback | TEXT | NULL | Phản hồi |
| created_at | DATETIME | AUTO | Ngày tạo |

**Index:**
- `idx_device_id` (device_id)
- `idx_status` (status)

---

## 🔗 RELATIONSHIPS

### One-to-One
- `auth_user` ←→ `users_profile`

### One-to-Many
- `users_role` → `users_profile`
- `books_category` → `books_book`
- `books_author` → `books_book`
- `books_publisher` → `books_book`
- `auth_user` (reader) → `borrowing_record`
- `auth_user` (librarian) → `borrowing_record`
- `borrowing_record` → `borrowing_detail`
- `books_book` → `borrowing_detail`
- `auth_user` → `borrowing_violation`
- `auth_user` → `notifications_notification`
- `iot_device` → `iot_command`

### Self-referencing
- `books_category` → `books_category` (parent_id)

---

## 📊 INDEXES & OPTIMIZATION

### Composite Indexes
```sql
-- Tìm kiếm sách
CREATE INDEX idx_book_search ON books_book(title, author_id, category_id);

-- Lịch sử mượn
CREATE INDEX idx_borrow_history ON borrowing_record(reader_id, borrow_date DESC);

-- Sách quá hạn
CREATE INDEX idx_overdue_books ON borrowing_detail(status, due_date);

-- Thông báo chưa đọc
CREATE INDEX idx_unread_notifications ON notifications_notification(user_id, is_read, created_at DESC);
```

---

## 🔒 CONSTRAINTS & BUSINESS RULES

### Check Constraints
```sql
-- Số lượng sách khả dụng <= tổng số lượng
ALTER TABLE books_book ADD CONSTRAINT chk_available 
CHECK (available <= quantity AND available >= 0);

-- Ngày trả >= ngày mượn
ALTER TABLE borrowing_record ADD CONSTRAINT chk_dates
CHECK (return_date IS NULL OR return_date >= borrow_date);

-- Số tiền phạt >= 0
ALTER TABLE borrowing_violation ADD CONSTRAINT chk_fine
CHECK (fine_amount >= 0 AND paid_amount >= 0 AND paid_amount <= fine_amount);
```

### Triggers
```sql
-- Tự động giảm available khi mượn sách
-- Tự động tăng available khi trả sách
-- Tự động tính phí phạt khi trả trễ
-- Tự động tạo thông báo khi gần đến hạn
```

---

**Lưu ý**: Schema này có thể điều chỉnh trong quá trình phát triển.
