# THÁNG 2: CORE FEATURES & USER PORTAL

**Thời gian**: Tuần 5-8  
**Mục tiêu**: Implement hệ thống mượn/trả sách, QR scanner, reader portal, và notification system

---

## 📅 TUẦN 5: BORROWING SYSTEM (Ngày 29-35)

### 🎯 Mục tiêu tuần
- Implement hệ thống mượn/trả sách
- Tính toán phí phạt tự động
- Gia hạn sách
- Lịch sử mượn/trả
- Quản lý vi phạm

---

### **Dev 1 - Backend (Ngày 29-35)**

##### **Ngày 29-30: Borrowing API - Part 1**
- [ ] Implement BorrowRecord API:
  - `GET /api/borrow-records/` - List (filter, search, pagination)
  - `POST /api/borrow-records/` - Create phiếu mượn
  - `GET /api/borrow-records/{id}/` - Detail
  - `PUT /api/borrow-records/{id}/` - Update
  - `DELETE /api/borrow-records/{id}/` - Cancel (chỉ nếu chưa trả)
- [ ] Business logic cho create:
  - Check số sách mượn tối đa (từ SystemConfig)
  - Check sách còn available
  - Giảm available count
  - Set due_date từ config
  - Tạo BorrowDetail cho từng sách
- [ ] Filter by:
  - Reader, Librarian
  - Status (BORROWED, RETURNED, OVERDUE)
  - Date range

##### **Ngày 31: Borrowing API - Part 2**
- [ ] Return book functionality:
  - `POST /api/borrow-records/{id}/return/` - Trả sách
  - Check tình trạng sách
  - Tính phí phạt nếu trễ
  - Tăng available count
  - Update status
- [ ] Return multiple books:
  - `POST /api/borrow-details/{id}/return/` - Trả từng sách riêng
- [ ] Business rules:
  - Phí phạt = số ngày trễ × phí/ngày (từ config)
  - Tạo Violation record nếu quá hạn
  - Gửi notification

##### **Ngày 32: Extension & Violation**
- [ ] Extension (Gia hạn):
  - `POST /api/borrow-details/{id}/extend/` - Gia hạn
  - Check số lần gia hạn tối đa
  - Check không có vi phạm
  - Extend due_date (thêm số ngày từ config)
  - Gửi notification
- [ ] Violation API:
  - `GET /api/violations/` - List violations
  - `GET /api/violations/{id}/` - Detail
  - `POST /api/violations/` - Create manual violation
  - `PUT /api/violations/{id}/resolve/` - Giải quyết vi phạm
  - Auto-create violation khi trả trễ
- [ ] Statistics endpoint:
  - `GET /api/borrow-records/statistics/` - Thống kê tổng quan

##### **Ngày 33: Celery Tasks for Auto-checks**
- [ ] Setup Celery Beat (scheduled tasks)
- [ ] Tạo tasks:
  - `check_overdue_books()` - Chạy hàng ngày
    - Find books quá hạn
    - Update status to OVERDUE
    - Create violations
    - Send notifications
  - `send_due_soon_reminders()` - Chạy hàng ngày
    - Find books sắp đến hạn (3 days before)
    - Send email reminders
  - `calculate_daily_fines()` - Chạy hàng ngày
    - Update fine amount cho overdue books
- [ ] Configure celery beat schedule
- [ ] Test tasks locally

##### **Ngày 34: History & Reports**
- [ ] Borrowing history:
  - `GET /api/users/{id}/borrow-history/` - Lịch sử mượn của user
  - Include returned and current borrows
  - Pagination
- [ ] Reader statistics:
  - `GET /api/users/{id}/statistics/` - Thống kê cá nhân
    - Total books borrowed
    - Current borrows
    - Violations count
- [ ] Book borrowing history:
  - `GET /api/books/{id}/borrow-history/` - Lịch sử mượn của sách

##### **Ngày 35: Testing**
- [ ] Test create borrow record
- [ ] Test return book với và không trễ
- [ ] Test phí phạt calculation
- [ ] Test extension logic
- [ ] Test violation creation
- [ ] Test Celery tasks
- [ ] Fix bugs

---

### **Dev 2 - Frontend (Ngày 29-35)**

##### **Ngày 29-30: Borrow Record Management**
- [ ] Tạo types:
  - `BorrowRecord`, `BorrowDetail`, `Violation`
- [ ] Tạo `borrowService.ts`
- [ ] Implement `BorrowListPage.tsx`:
  - Table với columns:
    - ID, Reader, Borrow date, Due date, Status
    - Actions: View, Return
  - Filter by status, date range, reader
  - Search by reader name, ID
  - Color coding for overdue books

##### **Ngày 31: Create Borrow Form**
- [ ] Tạo `CreateBorrowPage.tsx`:
  - Step 1: Select reader
    - Search reader by name/student ID
    - Display reader info
    - Check violation status
  - Step 2: Select books
    - Search books
    - Add multiple books
    - Show available quantity
    - Remove books
  - Step 3: Confirm
    - Review borrow details
    - Set due date (auto from config)
    - Notes field
  - Submit and create

##### **Ngày 32: Return Books**
- [ ] Tạo `ReturnBooksPage.tsx`:
  - Option 1: Scan QR/Barcode (placeholder for now)
  - Option 2: Search by borrow ID
  - Display borrow details
  - Check boxes for books to return
  - Input book condition
  - Calculate fine nếu overdue
  - Show fine amount
  - Confirm return
- [ ] Return confirmation modal

##### **Ngày 33: Borrow Detail View**
- [ ] Tạo `BorrowDetailPage.tsx`:
  - Show full borrow record info
  - List of books in borrow
  - Status của từng sách
  - Timeline (borrowed, extended, returned dates)
  - Violation info nếu có
  - Fine calculation
  - Actions:
    - Return individual book
    - Extend (if eligible)
    - Print receipt

##### **Ngày 34: Extension & Violations**
- [ ] Extension modal:
  - Show current due date
  - Show new due date
  - Confirm extend
- [ ] Violation management:
  - `ViolationListPage.tsx`
  - Filter by reader, type, resolved status
  - Resolve violation modal
  - Fine payment tracking
- [ ] Implement extension logic

##### **Ngày 35: Testing & Polish**
- [ ] Test end-to-end borrow flow
- [ ] Test return flow
- [ ] Test extension
- [ ] Test violation display
- [ ] Responsive testing
- [ ] Bug fixes
- [ ] Add loading states & error handling

---

### **Dev 3 - Support (Ngày 29-35)**

##### **Ngày 29-31: System Configuration UI**
- [ ] Tạo `SystemConfigPage.tsx` (Admin only):
  - List all configurations
  - Editable fields:
    - Max borrow books (student/staff)
    - Borrow duration days
    - Max extension count
    - Fine per day
    - Reminder before due days
  - Save changes
  - Reset to defaults
- [ ] Implement config API calls
- [ ] Form validation

##### **Ngày 32-33: Testing Borrowing System**
- [ ] Create test scenarios:
  - Normal borrow & return
  - Overdue borrow
  - Extension
  - Violation creation
- [ ] Execute test cases
- [ ] Document bugs
- [ ] Verify business logic

##### **Ngày 34-35: Documentation**
- [ ] Update API documentation
- [ ] Create `BORROWING-GUIDE.md`:
  - How to create borrow
  - How to return books
  - How to extend
  - How fines are calculated
- [ ] Create video demo
- [ ] Update project board

---

## 📅 TUẦN 6: QR CODE & ADVANCED SEARCH (Ngày 36-42)

### 🎯 Mục tiêu tuần
- Implement QR/Barcode scanner
- Tìm kiếm nâng cao
- Filter và sort
- Quick borrow/return với QR

---

### **Dev 1 - Backend (Ngày 36-42)**

##### **Ngày 36-37: QR/Barcode Search API**
- [ ] QR code scan endpoints:
  - `POST /api/books/scan-qr/` - Scan QR code
    - Input: QR code data
    - Output: Book details
  - `POST /api/books/scan-barcode/` - Scan barcode
    - Input: Barcode
    - Output: Book details
- [ ] Quick actions với QR:
  - `POST /api/borrow-records/quick-borrow/`
    - Input: Reader QR + Book QR
    - Create borrow instantly
  - `POST /api/borrow-records/quick-return/`
    - Input: Book QR
    - Find active borrow & return
- [ ] Validation logic

##### **Ngày 38: Advanced Search API**
- [ ] Enhance book search:
  - Full-text search (title, author, publisher)
  - Multiple filters combine
  - Fuzzy search
  - Search suggestions
- [ ] Search endpoint improvements:
  - `GET /api/books/search/`
    - Query params: q, category, author, publisher, language, year_from, year_to, available_only
    - Sort by: title, author, publication_year, created_at, popular
    - Pagination
- [ ] Search history (optional):
  - Track popular searches

##### **Ngày 39: Location & IoT Preparation**
- [ ] Location management:
  - `GET /api/locations/shelves/` - List shelves
  - `GET /api/locations/shelves/{shelf}/rows/` - List rows in shelf
- [ ] IoT device API (preparation cho future):
  - `GET /api/iot/devices/` - List devices
  - `POST /api/iot/devices/` - Register device
  - `POST /api/iot/commands/` - Send command
  - `GET /api/iot/commands/{id}/status/` - Check command status
- [ ] Stub implementation (return mock data for now)

##### **Ngày 40-42: Performance Optimization**
- [ ] Add database indexes:
  - Book search fields
  - Date fields for queries
- [ ] Optimize queries:
  - Use select_related, prefetch_related
  - Add caching with Redis (optional)
- [ ] API response time optimization
- [ ] Test performance
- [ ] Load testing (optional)

---

### **Dev 2 - Frontend (Ngày 36-42)**

##### **Ngày 36-37: QR Scanner Component**
- [ ] Install `html5-qrcode`
- [ ] Tạo `QRScanner.tsx` component:
  - Camera permission handling
  - Start/Stop scanner
  - Scan callback
  - Error handling
  - Support mobile & desktop
- [ ] Tạo `BarcodeScannerModal.tsx`:
  - QR scanner modal
  - Manual input fallback
  - Result display

##### **Ngày 38: QR Quick Actions**
- [ ] Integrate QR scanner trong borrow flow:
  - Scan reader QR to select reader
  - Scan book QR to add books
- [ ] Integrate QR scanner trong return flow:
  - Scan book QR to return
  - Auto-find borrow record
- [ ] Quick borrow page (Librarian):
  - `QuickBorrowPage.tsx`
  - Scan reader → Scan books → Submit
- [ ] Quick return page (Librarian):
  - `QuickReturnPage.tsx`
  - Scan book → Confirm return

##### **Ngày 39: Advanced Search UI**
- [ ] Enhance `BookListPage.tsx`:
  - Advanced search panel
  - Multiple filters:
    - Category (tree select)
    - Author (searchable dropdown)
    - Publisher (dropdown)
    - Language (dropdown)
    - Publication year range
    - Available only checkbox
  - Sort options:
    - Title A-Z/Z-A
    - Newest/Oldest
    - Most borrowed
  - Apply filters button
  - Clear filters button
- [ ] Search suggestions (autocomplete)

##### **Ngày 40: Location Display**
- [ ] Book location component:
  - Visual shelf/row indicator
  - Map view (optional - simple image with highlight)
- [ ] Location in book detail page
- [ ] Location search filter

##### **Ngày 41-42: Testing & Polish**
- [ ] Test QR scanner on different devices
- [ ] Test camera permissions
- [ ] Test quick borrow/return flow
- [ ] Test advanced search với nhiều filters
- [ ] Mobile responsive testing
- [ ] Performance testing
- [ ] Bug fixes

---

### **Dev 3 - QR & Testing (Ngày 36-42)**

##### **Ngày 36-38: QR Code Enhancement**
- [ ] Generate reader QR codes:
  - Add QR generation for users
  - Store QR in profile
  - Display QR in profile page
  - Print QR card feature
- [ ] QR code format standardization:
  - Define QR data format
  - Book QR: `BOOK:{book_id}`
  - Reader QR: `READER:{user_id}`
- [ ] Create QR code printing templates

##### **Ngày 39-40: End-to-End Testing**
- [ ] Test scenarios:
  - Complete borrow flow với QR
  - Complete return flow với QR
  - Advanced search với nhiều filters
  - Mobile QR scanning
- [ ] Cross-device testing
- [ ] Document test results

##### **Ngày 41-42: Documentation & Demo**
- [ ] Update API documentation
- [ ] Create `QR-CODE-GUIDE.md`:
  - QR format specification
  - How to generate QR
  - How to scan QR
  - Troubleshooting
- [ ] Create demo videos:
  - QR borrow/return flow
  - Advanced search
- [ ] Update project board

---

## 📅 TUẦN 7: READER PORTAL (Ngày 43-49)

### 🎯 Mục tiêu tuần
- Xây dựng portal cho độc giả
- Dashboard cho reader
- Catalog browsing
- Personal borrowing history
- Extension requests

---

### **Dev 1 - Backend (Ngày 43-49)**

##### **Ngày 43: Reader API Enhancements**
- [ ] Reader dashboard API:
  - `GET /api/reader/dashboard/` - Dashboard data
    - Current borrows count
    - Overdue count
    - Violations count
    - Recent activity
- [ ] Reader permissions check
- [ ] Rate limiting cho reader APIs

##### **Ngày 44: Book Catalog API for Readers**
- [ ] Public book catalog:
  - `GET /api/catalog/books/` - Browse books
    - Hide internal fields (quantity details, etc.)
    - Show only public info
    - Filter by category, author
    - Show availability status only
- [ ] Book detail for readers:
  - `GET /api/catalog/books/{id}/` - Public book detail
    - Reviews/ratings (if implemented)

##### **Ngày 45: My Borrowings API**
- [ ] Reader borrowing endpoints:
  - `GET /api/reader/my-borrowings/` - Current borrows
  - `GET /api/reader/my-borrowings/history/` - History
  - `GET /api/reader/my-borrowings/{id}/` - Detail
- [ ] Extension request:
  - `POST /api/reader/my-borrowings/{id}/extend-request/`
    - Check eligibility
    - Create request or auto-approve
- [ ] Notifications for readers:
  - `GET /api/reader/notifications/` - My notifications

##### **Ngày 46-49: Additional Features**
- [ ] Favorites/Wishlist (optional):
  - `POST /api/reader/favorites/` - Add to favorites
  - `GET /api/reader/favorites/` - List favorites
  - `DELETE /api/reader/favorites/{id}/` - Remove
- [ ] Review/Rating (optional - simple):
  - `POST /api/books/{id}/reviews/` - Add review
  - `GET /api/books/{id}/reviews/` - List reviews
- [ ] Testing all reader APIs
- [ ] Bug fixes

---

### **Dev 2 - Frontend (Ngày 43-49)**

##### **Ngày 43: Reader Layout & Dashboard**
- [ ] Tạo `ReaderLayout.tsx`:
  - Simple header (no sidebar)
  - User menu
  - Notifications icon
  - Language switcher
  - Logout
- [ ] Tạo `ReaderDashboardPage.tsx`:
  - Welcome message
  - Quick stats cards:
    - Current borrows
    - Overdue books (warning)
    - Violations (alert)
  - Recent activity list
  - Quick links (Browse, My Books, Profile)

##### **Ngày 44: Book Catalog**
- [ ] Tạo `CatalogPage.tsx`:
  - Grid/List view
  - Book cards:
    - Cover image
    - Title, Author
    - Availability badge
    - View detail button
  - Category filter sidebar
  - Search bar
  - Pagination
- [ ] Responsive design for mobile

##### **Ngày 45: Book Detail for Readers**
- [ ] Tạo `CatalogBookDetailPage.tsx`:
  - Large cover image
  - Book information
  - Availability status
  - Location (shelf/row) nếu available
  - Description
  - Reviews section (if implemented)
  - Similar books (optional)
- [ ] Add to favorites button (optional)

##### **Ngày 46: My Borrowings**
- [ ] Tạo `MyBorrowingsPage.tsx`:
  - Tabs:
    - Current Borrows
    - History
  - Current borrows list:
    - Book info
    - Due date (with countdown)
    - Extend button (if eligible)
    - Overdue warning
  - History list:
    - Book info
    - Borrowed date, Returned date
    - Filter by date range

##### **Ngày 47: Extension Request**
- [ ] Extension request modal:
  - Show current due date
  - Show new due date
  - Eligibility check
  - Reason field (optional)
  - Submit request
- [ ] Extension status indicator
- [ ] Extension history

##### **Ngày 48: Reader Profile**
- [ ] Tạo `ReaderProfilePage.tsx`:
  - View profile info
  - Edit profile:
    - Phone, Address
    - Upload avatar
  - Change password
  - Display reader QR code
  - Download QR card
- [ ] Statistics:
  - Total books borrowed
  - Current borrows
  - Violations

##### **Ngày 49: Polish & Testing**
- [ ] Test full reader flow
- [ ] Mobile responsive testing
- [ ] UI/UX improvements
- [ ] Bug fixes
- [ ] Add loading states

---

### **Dev 3 - Support (Ngày 43-49)**

##### **Ngày 43-45: Reader Testing**
- [ ] Create test reader accounts
- [ ] Test reader dashboard
- [ ] Test book browsing
- [ ] Test borrowing history
- [ ] Test extension request
- [ ] Mobile testing

##### **Ngày 46-47: Documentation**
- [ ] Create `READER-USER-GUIDE.md`:
  - How to login
  - How to browse books
  - How to view borrowings
  - How to request extension
  - How to update profile
- [ ] Screenshots for guide
- [ ] Vietnamese translation

##### **Ngày 48-49: QR Card Design**
- [ ] Design QR card template:
  - Reader photo
  - Name, ID
  - QR code
  - Library info
- [ ] Implement print functionality
- [ ] Test printing

---

## 📅 TUẦN 8: NOTIFICATIONS & EMAIL (Ngày 50-56)

### 🎯 Mục tiêu tuần
- Implement notification system
- Email templates
- Auto email sending
- In-app notifications
- Email queue với Celery

---

### **Dev 1 - Backend (Ngày 50-56)**

##### **Ngày 50-51: Notification System**
- [ ] Enhance Notification model (nếu cần)
- [ ] Notification API:
  - `GET /api/notifications/` - List notifications
  - `GET /api/notifications/unread/` - Unread count
  - `PUT /api/notifications/{id}/mark-read/` - Mark as read
  - `PUT /api/notifications/mark-all-read/` - Mark all as read
  - `DELETE /api/notifications/{id}/` - Delete notification
- [ ] Notification types:
  - DUE_SOON, OVERDUE, RETURNED, APPROVED, SYSTEM
- [ ] Create notification helper functions

##### **Ngày 52: Email Templates**
- [ ] Setup email template system:
  - HTML email templates với Django templates
  - Support variables (name, book, date, etc.)
- [ ] Create templates:
  - `welcome_email.html` - Tài khoản mới
  - `due_soon_reminder.html` - Sách sắp đến hạn
  - `overdue_notice.html` - Sách quá hạn
  - `return_confirmation.html` - Xác nhận trả sách
  - `extension_approved.html` - Gia hạn được chấp thuận
  - `violation_notice.html` - Thông báo vi phạm
  - `password_reset.html` - Reset mật khẩu
- [ ] Email styling với inline CSS

##### **Ngày 53: Celery Email Tasks**
- [ ] Create Celery tasks:
  - `send_email_task(subject, template, context, recipients)`
  - `send_due_soon_reminders_task()` - Scheduled
  - `send_overdue_notices_task()` - Scheduled
  - `send_welcome_email_task(user_id)`
  - `send_return_confirmation_task(borrow_id)`
- [ ] Email queue management
- [ ] Error handling & retry logic
- [ ] Email send status tracking

##### **Ngày 54: Scheduled Tasks**
- [ ] Configure Celery Beat schedule:
  - Daily 8 AM: Check overdue books
  - Daily 9 AM: Send due soon reminders
  - Daily 10 AM: Send overdue notices
  - Daily 11 PM: Calculate fines
- [ ] Test scheduled tasks locally
- [ ] Logging cho scheduled tasks

##### **Ngày 55: Notification Triggers**
- [ ] Implement notification/email triggers:
  - On borrow create: Welcome notification
  - 3 days before due: Due soon notification + email
  - On overdue: Overdue notification + email
  - On return: Return confirmation + email
  - On extension: Extension notification + email
  - On violation: Violation notification + email
- [ ] Use Django signals hoặc call directly
- [ ] Test all triggers

##### **Ngày 56: Testing & Optimization**
- [ ] Test email sending
- [ ] Test notification creation
- [ ] Test scheduled tasks
- [ ] Email template testing (different clients)
- [ ] Performance testing
- [ ] Bug fixes

---

### **Dev 2 - Frontend (Ngày 50-56)**

##### **Ngày 50-51: Notification UI**
- [ ] Tạo notification components:
  - `NotificationBell.tsx` - Icon with badge count
  - `NotificationDropdown.tsx` - Dropdown list
  - `NotificationItem.tsx` - Single notification
- [ ] Integrate notification bell in header
- [ ] Implement notification list:
  - Unread vs read styling
  - Mark as read on click
  - Link to related page
  - Delete notification
  - Mark all as read
- [ ] Real-time update (polling hoặc WebSocket future)

##### **Ngày 52: Notification Page**
- [ ] Tạo `NotificationPage.tsx`:
  - Full notification list
  - Filter by type
  - Filter by read/unread
  - Date range filter
  - Bulk actions (mark all read, delete all)
  - Pagination
- [ ] Notification preferences (optional):
  - Email notifications on/off
  - Notification types selection

##### **Ngày 53-54: Email Settings (Admin)**
- [ ] Tạo `EmailSettingsPage.tsx` (Admin only):
  - Configure email backend
  - Test email sending
  - View email queue (if tracked)
  - View email logs
  - Email templates preview
- [ ] Test email button

##### **Ngày 55: Integration**
- [ ] Integrate notifications across app:
  - Show notification badge
  - Auto-refresh notifications
  - Toast notifications for important events
- [ ] Test notification flow
- [ ] Polish UI

##### **Ngày 56: Testing & Polish**
- [ ] Test notification system
- [ ] Test email sending
- [ ] Cross-browser testing
- [ ] Mobile responsive testing
- [ ] Bug fixes
- [ ] UI improvements

---

### **Dev 3 - Testing & Documentation (Ngày 50-56)**

##### **Ngày 50-52: Email Testing**
- [ ] Setup email testing environment:
  - Mailtrap hoặc similar
  - Test SMTP settings
- [ ] Test all email templates:
  - Welcome email
  - Due soon reminder
  - Overdue notice
  - Return confirmation
  - Extension approved
  - Violation notice
  - Password reset
- [ ] Test email với different email clients:
  - Gmail, Outlook, Yahoo
  - Mobile email apps
- [ ] Document test results

##### **Ngày 53-54: Integration Testing**
- [ ] Test complete flows:
  - Borrow → Due soon email → Overdue email
  - Extension request → Approval email
  - Return → Confirmation email
- [ ] Test scheduled tasks:
  - Verify cron jobs run
  - Verify emails sent
- [ ] Test notification triggers
- [ ] Load testing (send nhiều emails)

##### **Ngày 55-56: Documentation & Demo**
- [ ] Update API documentation
- [ ] Create `EMAIL-NOTIFICATION-GUIDE.md`:
  - Email setup instructions
  - Notification types
  - Email templates guide
  - Troubleshooting
- [ ] Create `CELERY-SETUP-GUIDE.md`:
  - Install Redis
  - Configure Celery
  - Run Celery worker
  - Run Celery beat
- [ ] Create demo videos
- [ ] Update project board

---

### 📦 Deliverables Tháng 2

1. ✅ **Borrowing System hoàn chỉnh**:
   - Create/Return/Extend borrow records
   - Fine calculation
   - Violation management
   - Auto-check overdue books
   - Borrowing history

2. ✅ **QR Code System**:
   - QR generation for books & readers
   - QR scanner functionality
   - Quick borrow/return với QR
   - QR card printing

3. ✅ **Advanced Search**:
   - Full-text search
   - Multiple filters
   - Sort options
   - Location search

4. ✅ **Reader Portal**:
   - Reader dashboard
   - Book catalog browsing
   - My borrowings
   - Extension requests
   - Profile management
   - Reader QR code

5. ✅ **Notification & Email System**:
   - In-app notifications
   - Email templates
   - Auto email sending
   - Scheduled tasks
   - Notification UI
   - Email settings

6. ✅ **Testing & Documentation**:
   - All features tested
   - API documentation updated
   - User guides created
   - Demo videos

---

## 📊 Checklist cuối Tháng 2

### Technical
- [ ] Celery + Redis running stable
- [ ] Email sending works
- [ ] Scheduled tasks run correctly
- [ ] QR scanner works on mobile
- [ ] No performance issues
- [ ] All APIs tested

### Features
- [ ] Borrowing system works end-to-end
- [ ] Return system with fine calculation works
- [ ] Extension system works
- [ ] QR borrow/return works
- [ ] Advanced search works
- [ ] Reader portal fully functional
- [ ] Notifications display correctly
- [ ] Emails sent automatically

### Documentation
- [ ] API docs updated
- [ ] User guides created
- [ ] Technical guides created
- [ ] Demo videos recorded

### Team
- [ ] Team confident với codebase
- [ ] No major blockers
- [ ] Ready for final month

---

## ⚠️ Risks & Mitigation - Tháng 2

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Celery setup issues | Medium | High | Follow detailed guides, test early |
| Email delivery problems | Medium | Medium | Use reliable service, test thoroughly |
| QR scanner browser compatibility | Medium | Medium | Test multiple browsers, fallback manual input |
| Performance issues với scheduled tasks | Low | Medium | Optimize queries, use indexes |
| Complex business logic bugs | High | High | Write tests, thorough manual testing |

---

**Next**: Xem `docs/04-MONTH-3-PLAN.md` cho kế hoạch Tháng 3 (Final month)
