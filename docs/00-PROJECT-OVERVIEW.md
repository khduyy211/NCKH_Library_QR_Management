# HỆ THỐNG QUẢN LÝ THƯ VIỆN - TỔNG QUAN DỰ ÁN

## 📋 THÔNG TIN DỰ ÁN

- **Tên dự án**: Library Management System (LMS)
- **Thời gian**: 3 tháng (12 tuần)
- **Nhân lực**: 3 developers (làm không công)
- **Ngân sách**: 5 triệu VNĐ
- **Mục tiêu**: Triển khai qua Internet, có responsive

---

## 🏗️ KIẾN TRÚC CÔNG NGHỆ

### Backend
- **Framework**: Django 5.0 (Python 3.11+)
- **API**: Django REST Framework
- **Database**: MySQL (Production) + SQLite (Development)
- **Authentication**: JWT (djangorestframework-simplejwt)
- **Task Queue**: Celery + Redis
- **Storage**: AWS S3 / MinIO (media files)

### Frontend
- **Framework**: React 18 + TypeScript
- **State Management**: Redux Toolkit / React Query
- **UI Library**: Bootstrap 5 + React-Bootstrap
- **Routing**: React Router v6
- **HTTP Client**: Axios
- **QR Scanner**: html5-qrcode
- **Charts**: Chart.js / Recharts
- **i18n**: react-i18next

### DevOps
- **Web Server**: Nginx
- **App Server**: Gunicorn
- **Containerization**: Docker + Docker Compose
- **CI/CD**: GitHub Actions (optional)
- **Hosting**: VPS (DigitalOcean/Vultr)
- **SSL**: Let's Encrypt
- **Monitoring**: Sentry

---

## 👥 NGƯỜI DÙNG HỆ THỐNG

### 1. Quản trị viên (Administrator)
- Quản lý thủ thư
- Quản lý độc giả
- Quản lý sách
- Phân quyền người dùng
- Xem tất cả báo cáo

### 2. Thủ thư (Librarian)
- Quản lý sách (CRUD)
- Quản lý độc giả (CRUD)
- Quản lý phiếu mượn/trả
- Scan QR/Barcode
- Thống kê và báo cáo
- Xử lý vi phạm

### 3. Độc giả (Reader)
**Loại 3a: Sinh viên (Student)**
**Loại 3b: Giảng viên/CBNV (Staff)**

- Xem danh sách sách
- Tìm kiếm sách
- Xem chi tiết sách
- Xem lịch sử mượn/trả
- Yêu cầu gia hạn
- Cập nhật thông tin cá nhân
- Đổi mật khẩu
- Xem thông báo

---

## 🎯 CHỨC NĂNG CHÍNH

### Module 1: Authentication & Authorization
- Đăng nhập/Đăng xuất
- Phân quyền theo role (Admin/Librarian/Reader)
- Đổi mật khẩu
- Quên mật khẩu (email reset)

### Module 2: Quản lý Sách
- CRUD sách
- Quản lý thể loại, tác giá, nhà xuất bản
- Upload ảnh bìa
- Import/Export Excel
- Generate QR Code cho mỗi sách
- Quản lý vị trí sách (kệ, ngăn)

### Module 3: Quản lý Độc giả
- CRUD độc giả (Admin, Thủ thư)
- Phân loại: Sinh viên / Giảng viên/CBNV
- Quản lý thông tin cá nhân
- Lịch sử mượn/trả

### Module 4: Quản lý Mượn/Trả
- Tạo phiếu mượn
- Trả sách
- Gia hạn sách
- Scan QR/Barcode để mượn/trả
- Tính phí phạt tự động
- Quản lý vi phạm

### Module 5: Tìm kiếm
- Tìm kiếm theo tên sách, tác giả, ISBN, thể loại
- Filter nâng cao
- Scan QR/Barcode để tìm sách
- Hiển thị vị trí sách trong thư viện

### Module 6: Thống kê & Báo cáo
- Thống kê tổng quan (Dashboard)
- Báo cáo sách mượn nhiều nhất
- Báo cáo độc giả tích cực
- Báo cáo vi phạm
- Báo cáo theo thời gian
- Export PDF/Excel

### Module 7: Thông báo
- Thông báo sách sắp đến hạn
- Thông báo sách quá hạn
- Gửi email tự động
- Thông báo trên hệ thống

### Module 8: Cấu hình Hệ thống
- Cấu hình thời gian mượn
- Cấu hình phí phạt
- Cấu hình số sách mượn tối đa
- Sao lưu và phục hồi dữ liệu
- Quản lý log hoạt động

### Module 9: IoT (Tìm sách bằng đèn LED)
- Kích hoạt đèn LED từ web
- LED nóc kệ + dải LED ô sách
- Nhận feedback từ nút nhấn
- Quản lý thiết bị IoT

---

## 📊 PHÂN BỔ CÔNG VIỆC

### Developer 1 - Backend Lead
- Setup Django project
- Database design & models
- REST API development
- Authentication & Authorization
- Celery tasks (email, notifications)
- Deployment & DevOps

### Developer 2 - Frontend Lead
- Setup React project
- UI/UX implementation
- Component development
- State management
- Integration with API
- Responsive design

### Developer 3 - Full-stack Support
- QR Code generation & scanning
- Report generation (PDF/Excel)
- Email templates
- Testing (Unit + Integration)
- i18n implementation
- Bug fixes & support

---

## 💰 NGÂN SÁCH CHI TIẾT

### Chi phí Hosting (3 tháng)
| Hạng mục | Chi phí | Ghi chú |
|----------|---------|---------|
| Domain (.com) | 300,000đ | Năm đầu |
| VPS 2GB RAM | 900,000đ | $12/tháng x 3 |
| Email Service | 0đ | SendGrid free tier |
| SSL Certificate | 0đ | Let's Encrypt |
| Database Backup Storage | 150,000đ | 50k/tháng x 3 |
| CDN (Cloudflare) | 0đ | Free plan |
| **Tổng Web** | **1,350,000đ** | |

### Chi phí IoT (Optional - sau 3 tháng)
| Hạng mục | Số lượng | Đơn giá | Thành tiền |
|----------|----------|---------|------------|
| ESP8266 NodeMCU | 10 | 50,000đ | 500,000đ |
| LED 12V (nóc kệ) | 10 | 10,000đ | 100,000đ |
| Dải LED 1m | 10 | 50,000đ | 500,000đ |
| Nút nhấn | 10 | 5,000đ | 50,000đ |
| Nguồn 12V | 5 | 50,000đ | 250,000đ |
| Dây nối, linh kiện | - | - | 200,000đ |
| **Tổng IoT** | | | **1,600,000đ** |

### Dự phòng
| Hạng mục | Chi phí |
|----------|---------|
| Tools & Services | 500,000đ |
| Khẩn cấp | 1,550,000đ |
| **Tổng dự phòng** | **2,050,000đ** |

### **TỔNG NGÂN SÁCH: 5,000,000đ**

---

## ⚠️ RỦI RO VÀ GIẢI PHÁP

| Rủi ro | Mức độ | Ảnh hưởng | Giải pháp |
|--------|--------|-----------|-----------|
| Team thiếu kinh nghiệm React | Cao | Delay timeline | Học React intensively 1-2 tuần đầu, pair programming |
| Django REST API phức tạp | Trung bình | Chất lượng code | Follow best practices, code review |
| Không biết IoT | Cao | Không hoàn thành feature | Trì hoãn IoT sang giai đoạn 2 |
| Vượt ngân sách hosting | Trung bình | Dừng dự án | Dùng free tier, tối ưu resources |
| Không đủ thời gian | Cao | Không deploy | Cắt giảm features ít quan trọng |
| Bug production | Trung bình | User experience | Testing kỹ, setup monitoring |
| Database lỗi | Thấp | Mất dữ liệu | Backup tự động hàng ngày |
| CORS issues | Thấp | Frontend không hoạt động | Cấu hình django-cors-headers |

---

## 📅 TIMELINE TỔNG QUÁT

```
Tháng 1 (Tuần 1-4): Setup & Foundation
├── Tuần 1: Project setup, database design
├── Tuần 2: Authentication & User management
├── Tuần 3: Book management (Backend)
└── Tuần 4: Book management (Frontend)

Tháng 2 (Tuần 5-8): Core Features
├── Tuần 5: Borrowing system
├── Tuần 6: QR Code & Search
├── Tuần 7: Reader portal
└── Tuần 8: Notifications & Email

Tháng 3 (Tuần 9-12): Advanced & Deployment
├── Tuần 9: Reports & Statistics
├── Tuần 10: i18n & Polish
├── Tuần 11: Testing & Security
└── Tuần 12: Deployment & Training
```

---

## 📚 TÀI LIỆU THAM KHẢO

- Django Documentation: https://docs.djangoproject.com/
- Django REST Framework: https://www.django-rest-framework.org/
- React Documentation: https://react.dev/
- Bootstrap 5: https://getbootstrap.com/docs/5.3/
- Docker: https://docs.docker.com/
- Celery: https://docs.celeryq.dev/

---

## 📞 LIÊN HỆ & HỖ TRỢ

- **Repository**: [GitHub Link]
- **Project Management**: [Trello/Jira Link]
- **Communication**: [Discord/Slack]
- **Documentation**: [Notion/Confluence]

---

**Ngày tạo**: 02/11/2025
**Phiên bản**: 1.0
**Trạng thái**: Draft
