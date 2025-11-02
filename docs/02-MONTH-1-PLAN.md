# THÁNG 1: FOUNDATION & CORE SETUP

**Thời gian**: Tuần 1-4  
**Mục tiêu**: Xây dựng nền tảng hệ thống, authentication và quản lý sách cơ bản

---

## 📅 TUẦN 1: PROJECT SETUP & DATABASE DESIGN (Ngày 1-7)

### 🎯 Mục tiêu tuần
- Setup môi trường development cho Backend và Frontend
- Thiết kế và implement Database
- Cấu hình Git workflow
- Chọn và setup hosting

### 📋 Tasks chi tiết

#### **Dev 1 - Backend Setup**

##### **Ngày 1-2: Django Project Setup**
- [ ] Cài đặt Python 3.11+ và thiết lập virtual environment
- [ ] Khởi tạo Django project: `django-admin startproject library_backend`
- [ ] Cài đặt dependencies:
  ```bash
  pip install django==5.0
  pip install djangorestframework
  pip install djangorestframework-simplejwt
  pip install django-cors-headers
  pip install mysqlclient
  pip install python-decouple
  pip install pillow
  pip install celery
  pip install redis
  pip install django-celery-beat
  pip install django-filter
  pip install drf-yasg  # Swagger documentation
  ```
- [ ] Tạo file `requirements.txt`
- [ ] Cấu hình `settings.py`:
  - Database MySQL + SQLite fallback
  - INSTALLED_APPS (DRF, CORS, Celery, etc.)
  - CORS settings
  - JWT settings
  - Static & Media files
- [ ] Tạo `.env` file cho environment variables
- [ ] Tạo `.gitignore`

##### **Ngày 3-4: Database Models**
- [ ] Tạo Django apps:
  ```bash
  python manage.py startapp users
  python manage.py startapp books
  python manage.py startapp borrowing
  python manage.py startapp notifications
  python manage.py startapp system
  python manage.py startapp iot
  ```
- [ ] Implement models theo schema:
  - `users/models.py`: Role, Profile
  - `books/models.py`: Category, Author, Publisher, Book
  - `borrowing/models.py`: BorrowRecord, BorrowDetail, Violation
  - `notifications/models.py`: Notification
  - `system/models.py`: SystemConfig, ActivityLog
  - `iot/models.py`: Device, Command
- [ ] Tạo migrations:
  ```bash
  python manage.py makemigrations
  python manage.py migrate
  ```
- [ ] Tạo superuser: `python manage.py createsuperuser`
- [ ] Setup Django Admin cho tất cả models

##### **Ngày 5: Database Seeding**
- [ ] Tạo management commands:
  - `python manage.py seed_roles`
  - `python manage.py seed_categories`
  - `python manage.py seed_config`
- [ ] Tạo data mẫu:
  - 3 roles (Admin, Librarian, Reader)
  - 10+ categories
  - System configs
  - 5 users mỗi loại
  - 50+ books mẫu

##### **Ngày 6-7: API Foundation**
- [ ] Setup Django REST Framework
- [ ] Tạo serializers cơ bản cho User, Profile
- [ ] Tạo viewsets cơ bản
- [ ] Configure URL routing
- [ ] Setup Swagger documentation
- [ ] Test API với Postman/Thunder Client

---

#### **Dev 2 - Frontend Setup**

##### **Ngày 1-2: React Project Setup**
- [ ] Cài đặt Node.js 18+ và npm/yarn
- [ ] Khởi tạo React project với TypeScript:
  ```bash
  npx create-react-app library-frontend --template typescript
  cd library-frontend
  ```
- [ ] Cài đặt dependencies:
  ```bash
  npm install react-router-dom
  npm install axios
  npm install @reduxjs/toolkit react-redux
  npm install bootstrap react-bootstrap
  npm install react-i18next i18next
  npm install @tanstack/react-query
  npm install react-hook-form
  npm install html5-qrcode
  npm install chart.js react-chartjs-2
  npm install date-fns
  npm install react-toastify
  npm install @fortawesome/fontawesome-free
  ```
- [ ] Cài đặt dev dependencies:
  ```bash
  npm install -D @types/react @types/node
  npm install -D sass
  npm install -D prettier eslint
  ```
- [ ] Cấu hình project structure:
  ```
  src/
  ├── api/              # API calls
  ├── components/       # Reusable components
  ├── pages/           # Page components
  ├── layouts/         # Layout components
  ├── hooks/           # Custom hooks
  ├── store/           # Redux store
  ├── types/           # TypeScript types
  ├── utils/           # Utility functions
  ├── constants/       # Constants
  ├── i18n/            # Internationalization
  └── styles/          # Global styles
  ```
- [ ] Setup `.env` file
- [ ] Setup `.gitignore`

##### **Ngày 3-4: Core Configuration**
- [ ] Setup React Router:
  - Public routes (Login, etc.)
  - Protected routes (Dashboard, etc.)
  - Role-based routing
- [ ] Setup Redux Toolkit:
  - Auth slice
  - User slice
  - App slice (loading, notifications)
- [ ] Setup Axios:
  - Base configuration
  - Request/Response interceptors
  - JWT token handling
- [ ] Setup React i18next:
  - Configure language files (vi, en)
  - Language switcher component

##### **Ngày 5-6: UI Foundation**
- [ ] Setup Bootstrap 5 theme
- [ ] Tạo layouts:
  - `PublicLayout.tsx` (cho login page)
  - `AdminLayout.tsx` (sidebar + header)
  - `ReaderLayout.tsx` (simple header)
- [ ] Tạo common components:
  - `Navbar.tsx`
  - `Sidebar.tsx`
  - `Footer.tsx`
  - `Loader.tsx`
  - `Alert.tsx`
  - `Modal.tsx`
  - `Pagination.tsx`
- [ ] Setup global styles với SCSS

##### **Ngày 7: Testing Connection**
- [ ] Test API connection với backend
- [ ] Test CORS settings
- [ ] Verify routing works
- [ ] Test language switching

---

#### **Dev 3 - DevOps & Documentation**

##### **Ngày 1-2: Git & Project Management**
- [ ] Tạo Git repository (GitHub/GitLab)
- [ ] Setup Git workflow:
  - `main` branch (production)
  - `develop` branch (development)
  - Feature branches: `feature/feature-name`
  - Hotfix branches: `hotfix/bug-name`
- [ ] Tạo `.gitignore` cho cả backend và frontend
- [ ] Commit initial code
- [ ] Setup branch protection rules
- [ ] Tạo project board (Trello/GitHub Projects)
- [ ] Tạo issues/tasks cho 3 tháng

##### **Ngày 3-4: Documentation**
- [ ] Review và hoàn thiện `docs/00-PROJECT-OVERVIEW.md`
- [ ] Review và hoàn thiện `docs/01-DATABASE-SCHEMA.md`
- [ ] Tạo `docs/02-API-DOCUMENTATION.md` (template)
- [ ] Tạo `docs/03-FRONTEND-STRUCTURE.md`
- [ ] Tạo `README.md` cho cả backend và frontend
- [ ] Tạo `CONTRIBUTING.md`

##### **Ngày 5-6: Hosting Research**
- [ ] Research và chọn VPS provider:
  - DigitalOcean ($12/month - 2GB RAM)
  - Vultr ($10/month - 2GB RAM)
  - AWS Lightsail ($10/month)
- [ ] Đăng ký tên miền (.com hoặc .vn)
- [ ] Setup Cloudflare (free plan)
- [ ] Research email service:
  - SendGrid (Free tier: 100 emails/day)
  - Mailgun (Free tier)
  - Amazon SES
- [ ] Tạo tài liệu deployment plan

##### **Ngày 7: Docker Setup (Optional)**
- [ ] Tạo `Dockerfile` cho backend
- [ ] Tạo `Dockerfile` cho frontend
- [ ] Tạo `docker-compose.yml`
- [ ] Test local deployment với Docker

---

### 📦 Deliverables Tuần 1

1. ✅ **Backend**:
   - Django project đầy đủ với 6 apps
   - Database models hoàn chỉnh
   - Migrations applied
   - Django Admin working
   - Data seeding scripts
   - Basic API endpoints

2. ✅ **Frontend**:
   - React + TypeScript project
   - Routing setup
   - Redux store setup
   - Axios configuration
   - i18n setup
   - Basic layouts & components
   - Bootstrap theme applied

3. ✅ **DevOps**:
   - Git repository với branching strategy
   - Project management board
   - Documentation đầy đủ
   - Hosting plan
   - Docker setup (optional)

4. ✅ **Testing**:
   - Backend API accessible
   - Frontend connects to backend
   - CORS working
   - All developers có môi trường dev hoạt động

---

## 📅 TUẦN 2-3: AUTHENTICATION & USER MANAGEMENT (Ngày 8-21)

### 🎯 Mục tiêu
- Hoàn thiện hệ thống authentication với JWT
- Implement phân quyền (Admin, Librarian, Reader)
- CRUD operations cho User, Thủ thư, Độc giả
- Activity logging

---

### **TUẦN 2: Backend Authentication API**

#### **Dev 1 - Backend (Ngày 8-14)**

##### **Ngày 8-9: JWT Authentication**
- [ ] Setup JWT với `djangorestframework-simplejwt`
- [ ] Tạo custom User serializer
- [ ] Implement API endpoints:
  - `POST /api/auth/register/` - Đăng ký (chỉ admin/librarian tạo reader)
  - `POST /api/auth/login/` - Đăng nhập (trả về access + refresh token)
  - `POST /api/auth/logout/` - Đăng xuất (blacklist token)
  - `POST /api/auth/refresh/` - Refresh token
  - `POST /api/auth/change-password/` - Đổi mật khẩu
  - `POST /api/auth/forgot-password/` - Quên mật khẩu
  - `POST /api/auth/reset-password/` - Reset mật khẩu
  - `GET /api/auth/me/` - Lấy thông tin user hiện tại
- [ ] Test tất cả endpoints với Postman

##### **Ngày 10-11: Permissions & Authorization**
- [ ] Tạo custom permissions:
  - `IsAdmin` - Chỉ admin
  - `IsLibrarian` - Admin + Librarian
  - `IsReader` - Tất cả authenticated users
  - `IsOwnerOrAdmin` - Owner hoặc admin
- [ ] Tạo mixins cho permission checking
- [ ] Áp dụng permissions cho các viewsets
- [ ] Test authorization logic

##### **Ngày 12: User Management API**
- [ ] API quản lý User/Profile:
  - `GET /api/users/` - List users (phân trang, filter)
  - `POST /api/users/` - Tạo user mới
  - `GET /api/users/{id}/` - Chi tiết user
  - `PUT /api/users/{id}/` - Cập nhật user
  - `DELETE /api/users/{id}/` - Xóa user (soft delete)
  - `GET /api/users/me/` - Profile của user hiện tại
  - `PUT /api/users/me/` - Cập nhật profile
- [ ] Filter theo role, reader_type, faculty
- [ ] Search theo username, email, student_id

##### **Ngày 13: Activity Logging**
- [ ] Tạo middleware để log activities
- [ ] Implement signal handlers:
  - Log khi tạo/sửa/xóa user
  - Log khi login/logout
  - Log khi thay đổi quan trọng
- [ ] API xem logs:
  - `GET /api/logs/` - List logs (admin only)
  - Filter theo user, action, date range
- [ ] Test logging system

##### **Ngày 14: Email Integration**
- [ ] Setup email backend (SMTP/SendGrid)
- [ ] Tạo email templates:
  - Welcome email
  - Reset password email
  - Account created email
- [ ] Tạo Celery tasks cho email:
  - `send_welcome_email(user_id)`
  - `send_reset_password_email(user_id, token)`
- [ ] Test email sending

---

### **TUẦN 2: Frontend Authentication**

#### **Dev 2 - Frontend (Ngày 8-14)**

##### **Ngày 8-9: Authentication Pages**
- [ ] Tạo types/interfaces:
  - `User`, `Profile`, `LoginCredentials`, `RegisterData`
- [ ] Tạo API service `authService.ts`:
  - login, logout, register, refresh, changePassword, etc.
- [ ] Implement Redux auth slice:
  - States: user, token, isAuthenticated, loading
  - Actions: login, logout, updateProfile
  - Async thunks
- [ ] Tạo pages:
  - `LoginPage.tsx`
  - `ForgotPasswordPage.tsx`
  - `ResetPasswordPage.tsx`

##### **Ngày 10: Login & Logout**
- [ ] Design Login form với validation
- [ ] Implement login logic:
  - Call API
  - Store token in localStorage
  - Redirect based on role
- [ ] Implement logout logic:
  - Clear token
  - Clear Redux state
  - Redirect to login
- [ ] Add remember me functionality
- [ ] Add loading states & error handling

##### **Ngày 11: Protected Routes**
- [ ] Tạo `ProtectedRoute.tsx` component
- [ ] Tạo `RoleBasedRoute.tsx` component
- [ ] Setup routing:
  ```typescript
  /login -> LoginPage (public)
  /forgot-password -> ForgotPasswordPage (public)
  /reset-password -> ResetPasswordPage (public)
  
  /admin/* -> AdminLayout (admin only)
  /librarian/* -> LibrarianLayout (admin + librarian)
  /reader/* -> ReaderLayout (reader only)
  ```
- [ ] Test routing với different roles

##### **Ngày 12: Profile Management**
- [ ] Tạo `ProfilePage.tsx`:
  - View profile information
  - Edit profile (phone, address, avatar)
  - Upload avatar
- [ ] Tạo `ChangePasswordPage.tsx`:
  - Current password
  - New password with validation
  - Confirm password
- [ ] Form validation với `react-hook-form`

##### **Ngày 13-14: User Management UI (Admin/Librarian)**
- [ ] Tạo `UserListPage.tsx`:
  - Table với pagination
  - Filter by role, reader type
  - Search box
  - Actions: Edit, Delete, View
- [ ] Tạo `UserFormPage.tsx` (Create/Edit):
  - Form fields theo role
  - Validation
  - Submit logic
- [ ] Tạo `UserDetailPage.tsx`:
  - View full information
  - Borrowing history preview
- [ ] Implement CRUD operations với API

---

### **TUẦN 3: Polish & Testing**

#### **Dev 3 - Testing & Documentation (Ngày 8-14)**

##### **Ngày 8-10: API Testing**
- [ ] Tạo Postman collection cho Auth APIs
- [ ] Test tất cả endpoints:
  - Valid cases
  - Invalid cases
  - Edge cases
- [ ] Test permissions & authorization
- [ ] Document test results
- [ ] Report bugs nếu có

##### **Ngày 11-12: Frontend Testing**
- [ ] Test login/logout flow
- [ ] Test protected routes
- [ ] Test role-based access
- [ ] Test form validations
- [ ] Test responsive design (mobile, tablet)
- [ ] Cross-browser testing

##### **Ngày 13-14: Documentation**
- [ ] Update `docs/02-API-DOCUMENTATION.md`:
  - Auth endpoints
  - User endpoints
  - Request/Response examples
  - Error codes
- [ ] Tạo `docs/USER-ROLES-PERMISSIONS.md`
- [ ] Tạo video demo Auth system
- [ ] Update project board

---

#### **All Devs (Ngày 15-21): Integration & Bug Fixes**

##### **Ngày 15-16: Integration Testing**
- [ ] End-to-end testing của auth flow
- [ ] Test backend + frontend integration
- [ ] Fix CORS issues nếu có
- [ ] Fix JWT token issues nếu có

##### **Ngày 17-18: Bug Fixes**
- [ ] Review bugs từ testing
- [ ] Fix critical bugs
- [ ] Refactor code nếu cần
- [ ] Code review lẫn nhau

##### **Ngày 19-20: Polish UI/UX**
- [ ] Improve form validation messages
- [ ] Add loading indicators
- [ ] Add success/error toasts
- [ ] Improve error handling
- [ ] Add helpful tooltips

##### **Ngày 21: Week 2-3 Review**
- [ ] Team meeting review tiến độ
- [ ] Demo features đã hoàn thành
- [ ] Update documentation
- [ ] Plan cho tuần 4

---

## 📅 TUẦN 4: BOOK MANAGEMENT (Ngày 22-28)

### 🎯 Mục tiêu
- Implement CRUD cho Sách, Thể loại, Tác giả, NXB
- Upload ảnh bìa sách
- Generate QR code cho sách
- Import/Export Excel
- Tìm kiếm cơ bản

---

### **Dev 1 - Backend (Ngày 22-28)**

##### **Ngày 22: Category, Author, Publisher APIs**
- [ ] Implement API cho Category:
  - `GET /api/categories/` - List (support tree structure)
  - `POST /api/categories/` - Create
  - `GET /api/categories/{id}/` - Detail
  - `PUT /api/categories/{id}/` - Update
  - `DELETE /api/categories/{id}/` - Delete
  - `GET /api/categories/tree/` - Get category tree
- [ ] Implement API cho Author:
  - Standard CRUD endpoints
  - Search by name
- [ ] Implement API cho Publisher:
  - Standard CRUD endpoints
  - Search by name

##### **Ngày 23-24: Book API**
- [ ] Implement Book API:
  - `GET /api/books/` - List với pagination, filter, search
  - `POST /api/books/` - Create
  - `GET /api/books/{id}/` - Detail
  - `PUT /api/books/{id}/` - Update
  - `DELETE /api/books/{id}/` - Soft delete
- [ ] Filter options:
  - Category, Author, Publisher
  - Language, Status
  - Available books only
- [ ] Search:
  - Title, ISBN, Barcode
  - Author name
- [ ] Sorting options

##### **Ngày 25: File Upload & QR Code**
- [ ] Setup media file handling:
  - Configure MEDIA_ROOT, MEDIA_URL
  - Image upload for book cover
  - Image validation (size, format)
- [ ] QR Code generation:
  - Install `qrcode` library
  - Generate QR code on book creation
  - Store QR code image
  - API: `GET /api/books/{id}/qr-code/`
- [ ] Barcode handling (optional)

##### **Ngày 26: Import/Export**
- [ ] Excel import:
  - Install `openpyxl`
  - `POST /api/books/import/` - Upload Excel
  - Parse Excel data
  - Validate data
  - Bulk create books
  - Return import results
- [ ] Excel export:
  - `GET /api/books/export/` - Download Excel
  - Include all book data
  - Apply filters before export
- [ ] Tạo Excel template mẫu

##### **Ngày 27-28: Testing & Optimization**
- [ ] Test tất cả Book APIs
- [ ] Test file upload
- [ ] Test QR code generation
- [ ] Test import/export
- [ ] Optimize queries (select_related, prefetch_related)
- [ ] Add indexes nếu cần

---

### **Dev 2 - Frontend (Ngày 22-28)**

##### **Ngày 22-23: Book List Page**
- [ ] Tạo types: `Book`, `Category`, `Author`, `Publisher`
- [ ] Tạo API services:
  - `bookService.ts`
  - `categoryService.ts`
  - `authorService.ts`
  - `publisherService.ts`
- [ ] Implement `BookListPage.tsx`:
  - Table/Grid view toggle
  - Pagination
  - Search box
  - Filter sidebar
  - Sort options
  - Actions: View, Edit, Delete

##### **Ngày 24: Book Form**
- [ ] Tạo `BookFormPage.tsx` (Create/Edit):
  - Form fields:
    - Title, ISBN, Barcode
    - Category (dropdown)
    - Author (dropdown/searchable)
    - Publisher (dropdown/searchable)
    - Description (textarea)
    - Publication year, Language, Pages
    - Quantity, Price
    - Location (Shelf, Row)
    - Cover image upload
  - Form validation
  - Image preview
  - Submit logic

##### **Ngày 25: Book Detail & QR**
- [ ] Tạo `BookDetailPage.tsx`:
  - Display all book information
  - Show cover image
  - Show QR code
  - Download QR code button
  - Edit button
  - Borrowing status
  - Borrowing history (preview)
- [ ] QR code display component

##### **Ngày 26: Import/Export**
- [ ] Tạo import UI:
  - Upload button
  - File validation
  - Progress indicator
  - Show import results
  - Error handling
- [ ] Tạo export UI:
  - Export button với filter
  - Download file
- [ ] Download Excel template button

##### **Ngày 27: Category/Author/Publisher Management**
- [ ] Tạo management pages:
  - `CategoryListPage.tsx` (với tree view)
  - `AuthorListPage.tsx`
  - `PublisherListPage.tsx`
- [ ] CRUD forms cho từng loại
- [ ] Simple modals cho quick add

##### **Ngày 28: Polish & Testing**
- [ ] Test tất cả CRUD operations
- [ ] Test file upload
- [ ] Test import/export
- [ ] Responsive testing
- [ ] Bug fixes

---

### **Dev 3 - Support & Documentation (Ngày 22-28)**

##### **Ngày 22-24: QR Code Research**
- [ ] Research QR code libraries cho backend:
  - `python-qrcode`
  - `qrcode-generator`
- [ ] Research QR code scanner cho frontend:
  - `html5-qrcode`
  - `react-qr-reader`
- [ ] Tạo POC (Proof of Concept)
- [ ] Document findings

##### **Ngày 25-26: Excel Template**
- [ ] Tạo Excel template mẫu cho import:
  - Columns: Title, ISBN, Author, Publisher, Category, etc.
  - Sample data
  - Instructions sheet
- [ ] Test import với template
- [ ] Create documentation

##### **Ngày 27-28: Testing & Documentation**
- [ ] Test Book management flow
- [ ] Update `docs/02-API-DOCUMENTATION.md`
- [ ] Tạo `docs/BOOK-IMPORT-GUIDE.md`
- [ ] Tạo video demo
- [ ] Update project board

---

### 📦 Deliverables Tháng 1

1. ✅ **Backend hoàn chỉnh**:
   - Authentication APIs với JWT
   - User management APIs
   - Book management APIs
   - Category, Author, Publisher APIs
   - File upload & QR code generation
   - Import/Export Excel
   - Activity logging
   - Email integration
   - Full API documentation

2. ✅ **Frontend hoàn chỉnh**:
   - Login/Logout pages
   - User management (Admin/Librarian)
   - Profile management
   - Book management pages
   - Category/Author/Publisher management
   - QR code display
   - Import/Export UI
   - Responsive design

3. ✅ **Testing**:
   - All APIs tested
   - Frontend tested
   - Integration tested
   - Documentation updated

4. ✅ **Ready for Tháng 2**:
   - Foundation solid
   - Team confident với tech stack
   - No blocking issues

---

## 📊 Checklist cuối Tháng 1

### Technical
- [ ] Django backend running stable
- [ ] React frontend running stable
- [ ] Database migrations up to date
- [ ] All APIs documented in Swagger
- [ ] All components documented
- [ ] Git commits có message rõ ràng
- [ ] No critical bugs

### Features
- [ ] Login/Logout works
- [ ] User CRUD works
- [ ] Book CRUD works
- [ ] QR code generation works
- [ ] Import/Export works
- [ ] File upload works
- [ ] Permission system works
- [ ] Activity logging works

### Documentation
- [ ] API documentation complete
- [ ] Frontend structure documented
- [ ] Database schema documented
- [ ] Deployment plan documented
- [ ] README files updated

### Team
- [ ] All devs comfortable với codebase
- [ ] Code review process established
- [ ] No major conflicts
- [ ] Ready for next phase

---

## ⚠️ Risks & Mitigation - Tháng 1

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Learning curve React/Django | High | High | Pair programming, daily standups |
| Database design issues | Medium | High | Review schema multiple times, get feedback |
| CORS problems | Medium | Medium | Setup cors-headers correctly from start |
| File upload issues | Low | Medium | Test early, use proven libraries |
| Time delay | High | High | Cut non-essential features, work extra hours |

---

**Next**: Xem `docs/03-MONTH-2-PLAN.md` cho kế hoạch Tháng 2
