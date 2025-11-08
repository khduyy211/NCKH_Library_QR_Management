# 👨‍💻 DEVELOPER 3 - FULLSTACK DEVELOPER

**Role:** Fullstack Developer (Support)  
**Focus:** Backend Support, Frontend Support, Testing, Documentation  
**Duration:** 3 months (12 weeks)  
**Responsibility:** 20% Backend + 20% Frontend + Support tasks

---

## 📋 TỔNG QUAN NHIỆM VỤ

### **Vai trò chính:**
- ✅ Support Dev 1 với backend tasks
- ✅ Support Dev 2 với frontend tasks
- ✅ Viết tests (unit, integration)
- ✅ Viết documentation
- ✅ Code review
- ✅ Bug fixing
- ✅ Implement minor features

### **Tech Stack:**
- Backend: Django, DRF (support Dev 1)
- Frontend: React, TypeScript (support Dev 2)
- Testing: Pytest, Jest/React Testing Library
- Documentation: Markdown

---

## 📝 CHI TIẾT CÔNG VIỆC

### **A. BACKEND TESTING (68 Tests)**

#### **1. Model Tests (45 tests = 15 models × 3 tests each)**
```python
# tests/test_models.py

# User Model (3 tests)
test_user_creation() # Test create user with valid data
test_user_string_representation() # Test __str__ returns username
test_user_email_validation() # Test invalid email raises error

# Role Model (3 tests)
test_role_creation() # Test create role with name and code
test_role_unique_code() # Test duplicate code raises error
test_role_default_permissions() # Test role has default permissions

# Profile Model (3 tests)
test_profile_auto_create_on_user_save() # Profile created when user created
test_profile_one_to_one_with_user() # One user has one profile
test_profile_picture_upload() # Test image upload and path

# Category Model (3 tests)
test_category_tree_structure() # Test parent-child relationship
test_category_get_ancestors() # Get all parent categories
test_category_get_descendants() # Get all child categories

# Author Model (3 tests)
test_author_creation() # Create author with valid data
test_author_books_relationship() # author.books returns all books by author
test_author_search() # Search author by name

# Publisher Model (3 tests)
test_publisher_creation() # Create publisher with valid data
test_publisher_books_count() # Count books by publisher
test_publisher_contact_validation() # Phone/email validation

# Book Model (3 tests)
test_book_creation() # Create book with all required fields
test_book_available_count() # available = quantity - borrowed
test_book_qr_auto_generate() # QR code auto-generated on save

# BorrowRecord Model (3 tests)
test_borrow_record_creation() # Create borrow with reader and books
test_borrow_record_status_update() # Status changes correctly
test_borrow_record_fine_calculation() # total_fine calculated correctly

# BorrowDetail Model (3 tests)
test_borrow_detail_creation() # Create detail for each book
test_borrow_detail_due_date() # due date = borrow_date + days
test_borrow_detail_extension() # Extend due date logic

# Violation Model (3 tests)
test_violation_auto_create_on_overdue() # Violation created when overdue
test_violation_fine_calculation() # fine = days × rate
test_violation_payment() # Paid status after payment

# Notification Model (3 tests)
test_notification_creation() # Create notification
test_notification_mark_as_read() # Mark notification as read
test_notification_auto_send() # Notification sent on events

# SystemConfig Model (3 tests)
test_config_singleton() # Only one config instance
test_config_defaults() # Default values set correctly
test_config_update() # Update config values

# ActivityLog Model (3 tests)
test_activity_log_creation() # Log created on actions
test_activity_log_user_tracking() # Log stores user info
test_activity_log_search() # Search logs by action/user

# IoTDevice Model (3 tests)
test_iot_device_creation() # Create IoT device
test_iot_device_status() # Device online/offline status
test_iot_device_linked_to_book() # Device linked to book location

# IoTCommand Model (3 tests)
test_iot_command_creation() # Create command for device
test_iot_command_execution() # Command status changes
test_iot_command_locate_book() # Locate book sends command
```

#### **2. API Endpoint Tests (48+ tests)**
```python
# tests/test_api.py

# Authentication APIs (7 tests)
test_register_success() # POST /api/auth/register/ with valid data
test_register_duplicate_username() # Existing username fails
test_login_success() # POST /api/auth/login/ with valid credentials
test_login_invalid_credentials() # Wrong password fails
test_token_refresh() # POST /api/auth/refresh/ with refresh token
test_logout() # POST /api/auth/logout/ blacklists token
test_get_current_user() # GET /api/auth/me/ returns user info

# User Management APIs (5 tests)
test_list_users_as_admin() # GET /api/users/ returns user list
test_create_user_as_admin() # POST /api/users/ creates user
test_update_user() # PUT /api/users/{id}/ updates user
test_delete_user() # DELETE /api/users/{id}/ deletes user
test_user_permissions() # Only admin can access

# Book APIs (12 tests)
test_list_books() # GET /api/books/ returns books
test_search_books() # GET /api/books/?search=query
test_filter_books_by_category() # GET /api/books/?category=1
test_create_book() # POST /api/books/ with image upload
test_update_book() # PUT /api/books/{id}/
test_delete_book() # DELETE /api/books/{id}/
test_generate_qr_code() # GET /api/books/{id}/qr-code/
test_scan_qr_code() # POST /api/books/scan-qr/ returns book
test_locate_book() # POST /api/books/{id}/locate/
test_list_categories() # GET /api/books/categories/
test_list_authors() # GET /api/books/authors/
test_list_publishers() # GET /api/books/publishers/

# Borrowing APIs (8 tests)
test_borrow_books() # POST /api/borrows/ creates borrow record
test_borrow_max_books_limit() # Fails if > max books
test_borrow_with_unpaid_fines() # Fails if has fines
test_return_books() # POST /api/borrows/return/ returns books
test_return_with_fine() # Calculates fine correctly
test_extend_borrow() # POST /api/borrows/details/{id}/extend/
test_list_my_borrows() # GET /api/borrows/my-borrows/
test_list_overdue_borrows() # GET /api/borrows/overdue/

# Notification APIs (4 tests)
test_list_notifications() # GET /api/notifications/
test_get_notification_detail() # GET /api/notifications/{id}/
test_mark_notification_as_read() # POST /api/notifications/{id}/read/
test_mark_all_as_read() # POST /api/notifications/mark-all-read/

# Report APIs (8 tests)
test_dashboard_stats() # GET /api/reports/dashboard/
test_books_by_category() # GET /api/reports/books-by-category/
test_most_borrowed_books() # GET /api/reports/most-borrowed/
test_overdue_stats() # GET /api/reports/overdue-stats/
test_reader_activity() # GET /api/reports/reader-activity/
test_librarian_performance() # GET /api/reports/librarian-performance/
test_revenue_report() # GET /api/reports/revenue/
test_export_report() # GET /api/reports/export/?type=csv

# System APIs (4 tests)
test_get_config() # GET /api/system/config/
test_update_config() # PUT /api/system/config/
test_activity_log() # GET /api/system/logs/
test_health_check() # GET /api/system/health/
```

#### **3. Integration Tests (10 tests)**
```python
# tests/test_integration.py

test_complete_borrow_flow()
# Test: Reader login → Browse books → Borrow → Receive notification

test_complete_return_flow()
# Test: Librarian login → Search borrow → Return book → Fine calculation

test_overdue_notification_flow()
# Test: Celery task → Find overdue → Create violation → Send email

test_book_lifecycle()
# Test: Create book → Generate QR → Borrow → Return → Update availability

test_user_registration_to_borrow()
# Test: Register → Verify email → Login → Borrow book

test_fine_payment_flow()
# Test: Overdue → Calculate fine → Pay fine → Clear violation

test_admin_report_generation()
# Test: Admin login → Generate report → Export CSV

test_librarian_daily_workflow()
# Test: Login → View pending → Process returns → Check overdue

test_iot_locate_book()
# Test: Search book → Click locate → Send command → LED blinks

test_concurrent_borrow_same_book()
# Test: 2 users borrow last copy → One succeeds, one fails
```

#### **4. Performance Tests (5 tests)**
```python
# tests/test_performance.py

test_book_search_speed() # Search 10,000+ books < 1s
test_pagination_large_dataset() # Paginate 10,000 records efficiently
test_concurrent_borrows() # 50 simultaneous borrow requests
test_report_generation_speed() # Generate report with 1 year data < 5s
test_qr_scan_response_time() # QR scan response < 500ms
```

---

### **B. FRONTEND TESTING (20+ Component Tests)**

```typescript
// tests/components.test.tsx

// Common Components (5 tests)
test('Button renders correctly and handles click')
test('Input validates email format')
test('Modal opens and closes correctly')
test('Toast shows and auto-dismisses after timeout')
test('Pagination changes pages correctly')

// Book Components (5 tests)
test('BookCard displays book information correctly')
test('BookForm validates required fields before submit')
test('QRScanner requests camera permission')
test('BookSearch filters books by search query')
test('CategoryTree expands and collapses nodes')

// Borrowing Components (5 tests)
test('BorrowForm adds books to borrow list')
test('ReturnForm calculates fine for overdue books')
test('FineCalculator shows correct fine amount')
test('ReaderSelector searches and selects readers')
test('ExtendBorrowModal extends due date correctly')

// Layout Components (5 tests)
test('Sidebar shows role-based menu items')
test('Header displays user information and logout')
test('Breadcrumb shows correct navigation path')
test('NotificationDropdown shows unread count badge')
test('Footer displays copyright and links')
```

---

### **C. DOCUMENTATION TASKS (100+ Pages)**

#### **1. API Documentation (30 pages)**
```yaml
File: docs/api/API_REFERENCE.md
Format: OpenAPI 3.0 (Swagger)

Content:
  Authentication APIs:
    - POST /api/auth/register/
    - POST /api/auth/login/
    - POST /api/auth/refresh/
    - POST /api/auth/logout/
    - GET /api/auth/me/
    - PUT /api/auth/profile/
    - POST /api/auth/change-password/
    
  User Management APIs: (5 endpoints)
  Book APIs: (12 endpoints)
  Borrowing APIs: (8 endpoints)
  Notification APIs: (4 endpoints)
  Report APIs: (8 endpoints)
  System APIs: (4 endpoints)

Each endpoint includes:
  - Method and URL
  - Description
  - Request parameters
  - Request body (JSON schema)
  - Response codes (200, 400, 401, 404)
  - Response examples
  - Authentication requirements
  - Permission requirements
  - curl examples

Deliverable:
  - Swagger UI at /api/docs/
  - OpenAPI JSON/YAML export
  - Try It Out feature enabled
```

#### **2. User Manuals (40 pages)**

##### **Admin User Manual (15 pages)**
```yaml
File: docs/user-manual/ADMIN_MANUAL.md

Sections:
  1. Getting Started (2 pages):
     - Login to admin panel
     - Dashboard overview
     - Navigation menu
     
  2. User Management (3 pages):
     - View all users (table view, search, filter)
     - Create new user (Admin/Librarian/Reader)
     - Edit user information
     - Deactivate/Delete user
     - Reset user password
     
  3. Book Management (4 pages):
     - View book catalog (grid/table view)
     - Add new book (with image upload)
     - Edit book details
     - Delete book (soft delete)
     - Generate QR codes (individual/batch)
     - Manage categories (tree structure)
     - Manage authors and publishers
     
  4. System Configuration (2 pages):
     - Update system settings
     - Configure borrow rules (max books, loan period)
     - Set fine rates
     - Email template customization
     
  5. Reports & Analytics (3 pages):
     - View dashboard statistics
     - Generate custom reports (date range)
     - Most borrowed books report
     - Overdue statistics
     - Revenue reports
     - Export data (CSV, PDF)
     
  6. Activity Logs (1 page):
     - View system activity logs
     - Filter logs (by user, action, date)
     - Search logs
```

##### **Librarian User Manual (12 pages)**
```yaml
File: docs/user-manual/LIBRARIAN_MANUAL.md

Sections:
  1. Getting Started (2 pages):
     - Login and dashboard
     - Daily tasks overview
     - Understanding your role
     
  2. Processing Borrows (3 pages):
     - Search for reader (by ID, name, student ID)
     - Scan QR code or manually select books
     - Check reader eligibility (max books, fines)
     - Complete borrow transaction
     - Print borrow receipt
     
  3. Processing Returns (3 pages):
     - Search borrow record (by reader, book)
     - Check book condition (damaged, lost)
     - Calculate fines (if overdue)
     - Process fine payment
     - Complete return transaction
     - Print return receipt
     
  4. Managing Fines (2 pages):
     - View all unpaid fines
     - Filter fines (by reader, date, amount)
     - Process fine payment
     - Waive fines (if authorized)
     - Print payment receipt
     
  5. Reader Management (2 pages):
     - View reader information
     - Check borrow history
     - Extend borrow period
     - View violations
```

##### **Reader User Manual (8 pages)**
```yaml
File: docs/user-manual/READER_MANUAL.md

Sections:
  1. Getting Started (2 pages):
     - Register new account
     - Verify email
     - Login to system
     - Update profile (photo, contact info)
     
  2. Browsing Books (2 pages):
     - Search books (by title, author, ISBN)
     - Filter books (by category, language)
     - View book details (description, availability)
     - Check book location
     
  3. Borrowing Books (2 pages):
     - View current borrowed books
     - Check due dates
     - Request extend borrow period
     - Limitations (max books, overdue restrictions)
     
  4. Notifications (1 page):
     - View notifications (due reminders, overdue)
     - Mark as read
     - Email notifications
     
  5. My Profile (1 page):
     - View profile information
     - Update personal information
     - Change password
     - View borrow history
     - View fine history
```

#### **3. Developer Documentation (20 pages)**
```yaml
docs/developer/SETUP.md (5 pages):
  - Prerequisites (Python, Node.js, MySQL, Redis)
  - Clone repository
  - Backend setup (virtualenv, pip install, migrations)
  - Frontend setup (npm install)
  - Environment variables (.env file)
  - Database setup (create DB, run migrations)
  - Create superuser
  - Run development servers
  - Common setup issues

docs/developer/ARCHITECTURE.md (5 pages):
  - System overview diagram
  - Backend architecture (Django apps, models, APIs)
  - Frontend architecture (React components, Redux store)
  - Database schema (ER diagram)
  - API design principles (RESTful, versioning)
  - Authentication flow (JWT tokens)
  - State management (Redux slices)
  - File structure

docs/developer/CONTRIBUTING.md (3 pages):
  - Git workflow (feature branches, pull requests)
  - Branch naming conventions
  - Commit message format (Conventional Commits)
  - Code review process
  - Pull request template
  - Code style guidelines (PEP 8, ESLint)

docs/developer/DEPLOYMENT.md (4 pages):
  - Production environment setup
  - Environment configuration
  - Database migration in production
  - Static files (collectstatic)
  - Nginx configuration
  - SSL certificate (Let's Encrypt)
  - Monitoring and logging (Sentry)
  - Backup and restore

docs/developer/TROUBLESHOOTING.md (3 pages):
  - Common errors and solutions
  - Debug tips (Django Debug Toolbar, React DevTools)
  - FAQ for developers
  - Performance troubleshooting
  - Database issues
```

#### **4. Testing Documentation (5 pages)**
```yaml
File: docs/TESTING.md

Sections:
  1. Running Tests (1 page):
     - Backend: pytest commands
     - Frontend: npm test commands
     - Run specific tests
     - Coverage reports (coverage html)
     
  2. Writing Tests (2 pages):
     - Test file structure and naming
     - Using fixtures and factories
     - Mocking external services
     - Assertions and best practices
     - Test organization
     
  3. CI/CD Pipeline (1 page):
     - GitHub Actions workflow
     - Automated testing on push
     - Code coverage reports
     - Deployment pipeline
     
  4. Test Coverage Goals (1 page):
     - Backend: > 75%
     - Frontend: > 60%
     - Critical paths: 100% (auth, borrowing)
     - Coverage reports and monitoring
```

#### **5. FAQ Document (5 pages, 30+ questions)**
```yaml
File: docs/FAQ.md

General Questions (5):
  - How do I reset my password?
  - Who can borrow books?
  - How long is the loan period?
  - What happens if I return late?
  - Can I extend my borrow period?

For Readers (10):
  - How many books can I borrow at once?
  - How do I know when my books are due?
  - What if I lost a book?
  - Can I reserve a book that's currently borrowed?
  - How do I pay fines?
  - Why can't I borrow books?
  - How do I update my profile?
  - Can I see my borrow history?
  - What are the late fees?
  - How do I contact a librarian?

For Librarians (10):
  - How do I process a book borrow?
  - How do I scan QR codes?
  - What if QR code doesn't work?
  - How do I calculate fines?
  - Can I waive a fine for a reader?
  - How do I extend a borrow period?
  - What if a book is damaged?
  - How do I find a book's location?
  - How do I generate reports?
  - What if the system is slow?

For Admins (5):
  - How do I add new users?
  - How do I add new books in bulk?
  - How do I change system settings?
  - How do I view activity logs?
  - How do I backup data?
```

---

### **D. SUPPORT TASKS (Week-by-Week, 72 tasks)**

#### **Week 1-2: Environment & Auth Support (12 tasks)**
```yaml
Tasks:
  - Help DEV1 setup Django project structure
  - Verify MySQL database connection
  - Test Redis connection for Celery
  - Review User, Role, Profile models
  - Write tests for authentication models (3 tests × 3 = 9)
  - Test JWT token generation and refresh
  - Help DEV2 setup React + TypeScript project
  - Verify Bootstrap 5 integration
  - Test Redux Toolkit store setup
  - Review authentication components
  - Test login/register forms
  - Setup pytest and Jest configurations
```

#### **Week 3-4: Book Module Support (12 tasks)**
```yaml
Tasks:
  - Review Book, Category, Author, Publisher models
  - Write model tests for book module (12 tests)
  - Test QR code generation functionality
  - Help DEV1 implement book availability logic
  - Test book search API with various filters
  - Test image upload functionality
  - Help DEV2 implement BookCard component
  - Test BookForm validation
  - Review and test CategoryTree component
  - Test book list pagination
  - Help integrate book APIs with frontend
  - Write integration tests for book CRUD
```

#### **Week 5-6: Borrowing Module Support (12 tasks)**
```yaml
Tasks:
  - Review BorrowRecord and BorrowDetail models
  - Write tests for borrowing models (6 tests)
  - Test borrowing business logic (validation rules)
  - Help DEV1 implement fine calculation algorithm
  - Test borrow limits (max books per reader)
  - Test extend borrow functionality
  - Help DEV2 implement BorrowForm component
  - Test QRScanner component with camera
  - Test ReturnForm with fine display
  - Write integration tests for borrow flow
  - Write integration tests for return flow
  - Performance test concurrent borrows
```

#### **Week 7-8: Notification & Report Support (12 tasks)**
```yaml
Tasks:
  - Review Notification model
  - Test notification creation on events
  - Help DEV1 setup Celery scheduled tasks
  - Test overdue notification emails
  - Test Celery task: check_overdue_books
  - Test Celery task: send_due_soon_reminders
  - Help DEV2 implement NotificationDropdown component
  - Test notification real-time updates
  - Review all report APIs (8 endpoints)
  - Help DEV2 integrate Chart.js for reports
  - Test dashboard statistics accuracy
  - Write tests for report export (CSV, PDF)
```

#### **Week 9-10: IoT & Advanced Features (12 tasks)**
```yaml
Tasks:
  - Review IoTDevice and IoTCommand models
  - Test locate book functionality (LED control)
  - Help integrate ESP32 with backend API
  - Test LED control commands via MQTT/HTTP
  - Implement Category model tree methods
  - Help DEV2 implement advanced search filters
  - Test export functionality (CSV, PDF)
  - Optimize database queries (select_related, prefetch_related)
  - Add database indexes for performance
  - Test search performance with large dataset
  - Help implement fine payment flow
  - Write tests for violation model
```

#### **Week 11: Integration Testing & Bug Fixing (12 tasks)**
```yaml
Tasks:
  - Run full integration test suite
  - Test complete user workflows (end-to-end)
  - Identify and document bugs
  - Fix critical bugs (P0, P1)
  - Performance testing (load testing)
  - Security testing (XSS, CSRF, SQL injection)
  - Cross-browser testing (Chrome, Firefox, Safari)
  - Mobile responsive testing (iOS, Android)
  - Accessibility testing (WCAG guidelines)
  - Test error handling and edge cases
  - Code coverage analysis and improvement
  - Regression testing after bug fixes
```

#### **Week 12: Documentation & Deployment (12 tasks)**
```yaml
Tasks:
  - Complete API documentation (Swagger UI)
  - Finish Admin user manual (15 pages)
  - Finish Librarian user manual (12 pages)
  - Finish Reader user manual (8 pages)
  - Write deployment guide (4 pages)
  - Create FAQ document (30+ questions)
  - Setup CI/CD pipeline (GitHub Actions)
  - Configure production environment
  - Deploy to production server
  - Final testing on production
  - Performance monitoring setup
  - Prepare training materials/presentation
```

---

### **E. QUALITY ASSURANCE CHECKLIST (44 items)**

#### **Code Quality (10 items)**
```markdown
- [ ] Code follows PEP 8 (Python) and ESLint rules (TypeScript)
- [ ] No hardcoded credentials or sensitive data
- [ ] Proper error handling (try-except blocks, error boundaries)
- [ ] Input validation on all forms (frontend and backend)
- [ ] SQL injection prevention (use parameterized queries)
- [ ] XSS prevention (sanitize user input, escape output)
- [ ] CSRF protection enabled (Django default)
- [ ] Proper logging for errors and activities
- [ ] Code comments for complex logic
- [ ] No console.log statements in production code
```

#### **Testing Coverage (8 items)**
```markdown
- [ ] All models have unit tests (15 models × 3 = 45 tests)
- [ ] All API endpoints have tests (48+ endpoints)
- [ ] Critical business logic 100% covered
- [ ] Integration tests for main workflows (10 tests)
- [ ] Frontend components have basic tests (20 tests)
- [ ] Overall backend coverage > 75%
- [ ] Overall frontend coverage > 60%
- [ ] All tests pass in CI/CD pipeline
```

#### **Performance (6 items)**
```markdown
- [ ] Page load time < 3 seconds
- [ ] API response time < 1 second (average)
- [ ] Database queries optimized (use select_related, prefetch_related)
- [ ] Images optimized and compressed
- [ ] Pagination implemented for large lists (> 20 items)
- [ ] Caching enabled for frequently accessed data
```

#### **Security (8 items)**
```markdown
- [ ] Authentication required for protected routes
- [ ] Role-based access control (RBAC) implemented correctly
- [ ] Passwords hashed using Django default (PBKDF2)
- [ ] JWT tokens expire after reasonable time (15 min access, 7 days refresh)
- [ ] HTTPS enforced in production
- [ ] CORS configured correctly (allowed origins)
- [ ] Rate limiting on sensitive endpoints (login, register)
- [ ] Security headers set (CSP, X-Frame-Options, X-Content-Type-Options)
```

#### **User Experience (7 items)**
```markdown
- [ ] Responsive design works on mobile, tablet, desktop
- [ ] Loading states for async operations (spinners, skeletons)
- [ ] Error messages are user-friendly and actionable
- [ ] Success feedback for actions (toast notifications)
- [ ] Form validation with clear error messages
- [ ] Accessibility features (ARIA labels, keyboard navigation)
- [ ] Consistent UI/UX across all pages
```

#### **Documentation (5 items)**
```markdown
- [ ] API documentation complete with examples (Swagger)
- [ ] User manuals for all roles complete (Admin, Librarian, Reader)
- [ ] Setup and deployment guide complete
- [ ] FAQ covers common questions (30+ questions)
- [ ] Code has comments explaining complex logic
```

---

## 📊 TỔNG KẾT CÔNG VIỆC DEV3

| Category | Count | Details |
|----------|-------|---------|
| **Backend Tests** | 68 | Model (45), API (48+), Integration (10), Performance (5) |
| **Frontend Tests** | 20 | Component tests for critical UI elements |
| **Documentation Pages** | 100+ | API (30), User manuals (40), Developer (20), Testing (5), FAQ (5) |
| **Support Tasks** | 72 | 12 weeks × 6 tasks per week average |
| **QA Checklist Items** | 44 | Code (10), Testing (8), Performance (6), Security (8), UX (7), Docs (5) |
| **FAQ Questions** | 30+ | General (5), Readers (10), Librarians (10), Admins (5) |
| **Integration Tests** | 10 | Complete user workflows and business processes |
| **User Manuals** | 3 | Admin (15 pages), Librarian (12 pages), Reader (8 pages) |
| **Test Coverage Goal** | 75%+ | Backend models, APIs, business logic |
| **CI/CD Pipeline** | 1 | GitHub Actions with automated testing |

---

## 📅 TIMELINE CHI TIẾT

### **THÁNG 1: SETUP & DOCUMENTATION**

#### **Week 1: Project Documentation (5 days)**
**Branch:** `docs/project-documentation`

**Monday-Tuesday (2 days):**
- [ ] Đọc và hiểu toàn bộ project requirements
- [ ] Học Django, DRF basics
- [ ] Học React, TypeScript basics
- [ ] Setup development environment
- [ ] Clone repository
- [ ] Run backend và frontend locally

**Wednesday-Friday (3 days):**
- [ ] Tạo file README.md cho project
- [ ] Document installation steps
- [ ] Document project structure
- [ ] Tạo CONTRIBUTING.md
- [ ] Tạo CODE_OF_CONDUCT.md
- [ ] Commit: `docs: add project documentation`

**Deliverables:**
- ✅ Environment setup
- ✅ Basic documentation

---

#### **Week 2-3: Authentication Testing (10 days)**
**Branch:** `test/auth-tests`

**Week 2 - Monday-Wednesday (3 days):**
- [ ] Học Pytest
- [ ] Setup testing environment
- [ ] Viết unit tests cho User model
- [ ] Viết unit tests cho Profile model
- [ ] Test model methods

**Week 2 - Thursday-Friday (2 days):**
- [ ] Viết API tests cho authentication endpoints:
  - Test register endpoint
  - Test login endpoint
  - Test refresh token
  - Test logout
- [ ] Test invalid inputs
- [ ] Test edge cases

**Week 3 - Monday-Wednesday (3 days):**
- [ ] Viết permission tests
- [ ] Test role-based access
- [ ] Test JWT token validation
- [ ] Create test fixtures
- [ ] Create test utilities

**Week 3 - Thursday-Friday (2 days):**
- [ ] Review test coverage
- [ ] Fix failing tests
- [ ] Document testing approach
- [ ] Commit: `test(auth): add authentication tests`

**Test Files:**
```python
tests/
├── test_models.py
├── test_serializers.py
├── test_views.py
├── test_permissions.py
└── fixtures.py
```

**Deliverables:**
- ✅ Auth tests complete
- ✅ Test coverage > 80%

---

#### **Week 4: Category Management (5 days)**
**Branch:** `feature/category-management`

**Monday-Tuesday (2 days):**
- [ ] Implement category tree structure backend
- [ ] Add category CRUD endpoints
- [ ] Add parent-child relationship
- [ ] Write tests for categories

**Wednesday-Thursday (2 days):**
- [ ] Create Category management UI
- [ ] Create CategoryTree component
- [ ] Add drag-and-drop (optional)
- [ ] Integrate with backend API

**Friday:**
- [ ] Test category operations
- [ ] Write tests
- [ ] Document category system
- [ ] Commit: `feat(categories): implement category management`

**Deliverables:**
- ✅ Category CRUD working
- ✅ Tree structure functional

---

### **THÁNG 2: FEATURES & TESTING**

#### **Week 5: Fine Calculation & Violation (5 days)**
**Branch:** `feature/fine-calculation`

**Monday-Tuesday (2 days):**
- [ ] Implement fine calculation logic
- [ ] Create Violation model
- [ ] Add fine calculation to BorrowDetail
- [ ] Add system config for fine rates
- [ ] Write unit tests

**Wednesday-Thursday (2 days):**
- [ ] Create Fine display component (frontend)
- [ ] Show fines in borrow history
- [ ] Add fine payment tracking
- [ ] Create fine payment form

**Friday:**
- [ ] Test fine calculation accuracy
- [ ] Test different scenarios
- [ ] Document fine rules
- [ ] Commit: `feat(borrowing): implement fine calculation`

**Deliverables:**
- ✅ Fine calculation accurate
- ✅ Violation tracking

---

#### **Week 6: Search & Filter Implementation (5 days)**
**Branch:** `feature/qr-search`

**Monday-Tuesday (2 days):**
- [ ] Improve book search backend
- [ ] Add advanced filters (DjangoFilter)
- [ ] Add full-text search (optional)
- [ ] Optimize search queries
- [ ] Add search suggestions

**Wednesday-Thursday (2 days):**
- [ ] Create AdvancedSearch component (frontend)
- [ ] Add filter dropdowns
- [ ] Add search suggestions UI
- [ ] Add search history
- [ ] Test search performance

**Friday:**
- [ ] Write search tests
- [ ] Test filter combinations
- [ ] Document search features
- [ ] Commit: `feat(search): implement advanced search and filters`

**Deliverables:**
- ✅ Advanced search working
- ✅ Filters functional

---

#### **Week 7: Notification System (5 days)**
**Branch:** `feature/notifications`

**Monday-Tuesday (2 days):**
- [ ] Create Notification model
- [ ] Implement notification types:
  - Due soon
  - Overdue
  - Book available
  - System announcement
- [ ] Add notification creation logic
- [ ] Write tests

**Wednesday-Thursday (2 days):**
- [ ] Support Dev 2 với Notification UI
- [ ] Test notification flow
- [ ] Add notification preferences
- [ ] Test email notifications

**Friday:**
- [ ] Test notification system end-to-end
- [ ] Fix notification bugs
- [ ] Document notification types
- [ ] Commit: `feat(notifications): implement notification system`

**Deliverables:**
- ✅ Notification system complete
- ✅ All types working

---

#### **Week 8: Celery Tasks Support (5 days)**
**Branch:** `feature/celery-tasks`

**Monday-Wednesday (3 days):**
- [ ] Support Dev 1 với Celery setup
- [ ] Help test Celery tasks
- [ ] Monitor Celery worker logs
- [ ] Create Celery monitoring dashboard (optional)
- [ ] Document Celery configuration

**Thursday-Friday (2 days):**
- [ ] Write tests for Celery tasks
- [ ] Test scheduled tasks
- [ ] Test email sending
- [ ] Create task monitoring
- [ ] Commit: `feat(celery): add Celery task monitoring and tests`

**Deliverables:**
- ✅ Celery tasks tested
- ✅ Monitoring in place

---

### **THÁNG 3: POLISH & QUALITY**

#### **Week 9: Export & PDF Generation (5 days)**
**Branch:** `feature/export-pdf`

**Monday-Wednesday (3 days):**
- [ ] Install ReportLab (Python) or similar
- [ ] Create PDF export for reports
- [ ] Create CSV export
- [ ] Add export history endpoint
- [ ] Write tests

**Thursday-Friday (2 days):**
- [ ] Support Dev 2 với export UI
- [ ] Test PDF generation
- [ ] Test CSV export
- [ ] Optimize file size
- [ ] Commit: `feat(reports): add PDF and CSV export`

**Deliverables:**
- ✅ Export functionality working
- ✅ PDFs generated correctly

---

#### **Week 10: UI Polish & Accessibility (5 days)**
**Branch:** `feature/ui-polish`

**Monday-Wednesday (3 days):**
- [ ] Support Dev 2 với UI improvements
- [ ] Add loading states tất cả pages
- [ ] Add empty states
- [ ] Add error states
- [ ] Improve error messages

**Thursday-Friday (2 days):**
- [ ] Test accessibility (a11y)
- [ ] Add ARIA labels
- [ ] Test keyboard navigation
- [ ] Test screen reader compatibility
- [ ] Commit: `feat(ui): improve UI and accessibility`

**Deliverables:**
- ✅ Loading/empty/error states
- ✅ Accessibility improved

---

#### **Week 11: Comprehensive Testing (5 days)**
**Branch:** `bugfix/*` (multiple branches)

**Monday-Tuesday (2 days):**
- [ ] Manual testing toàn bộ system
- [ ] Test tất cả user flows:
  - Admin flow
  - Librarian flow
  - Reader flow
- [ ] Ghi lại tất cả bugs found
- [ ] Prioritize bugs

**Wednesday-Friday (3 days):**
- [ ] Fix critical bugs
- [ ] Fix medium bugs
- [ ] Create bug report document
- [ ] Retest fixed bugs
- [ ] Commit: `bugfix: fix multiple issues found in testing`

**Testing Checklist:**
- [ ] Authentication
- [ ] Book management
- [ ] Borrowing system
- [ ] QR scanning
- [ ] Notifications
- [ ] Reports
- [ ] Email sending
- [ ] Responsive design
- [ ] Cross-browser testing

**Deliverables:**
- ✅ Major bugs fixed
- ✅ System stable

---

#### **Week 12: User Manual & Final Documentation (5 days)**
**Branch:** `docs/user-manual`

**Monday-Wednesday (3 days):**
- [ ] Create User Manual:
  - For Admin
  - For Librarian
  - For Reader
- [ ] Add screenshots
- [ ] Create video tutorials (optional)
- [ ] Translate to Vietnamese

**Thursday-Friday (2 days):**
- [ ] Create API Documentation (if not done)
- [ ] Update all README files
- [ ] Create FAQ document
- [ ] Create TROUBLESHOOTING guide
- [ ] Final review
- [ ] Commit: `docs: add user manual and complete documentation`

**Documentation Created:**
```
docs/
├── USER_MANUAL_ADMIN.md
├── USER_MANUAL_LIBRARIAN.md
├── USER_MANUAL_READER.md
├── API_DOCUMENTATION.md
├── FAQ.md
├── TROUBLESHOOTING.md
└── DEPLOYMENT.md
```

**Deliverables:**
- ✅ User manuals complete
- ✅ All documentation updated

---

## 🧪 TESTING RESPONSIBILITIES

### **Unit Tests:**
```python
# Backend
tests/users/test_models.py
tests/books/test_models.py
tests/borrowing/test_models.py
tests/*/test_serializers.py
tests/*/test_views.py
```

### **Integration Tests:**
```python
tests/integration/test_auth_flow.py
tests/integration/test_borrow_flow.py
tests/integration/test_email_flow.py
```

### **Frontend Tests (optional):**
```typescript
src/__tests__/components/
src/__tests__/pages/
src/__tests__/hooks/
```

### **Test Coverage Goals:**
- Backend: > 80%
- Frontend: > 60% (optional)
- Critical paths: 100%

---

## 📝 DOCUMENTATION RESPONSIBILITIES

### **Technical Documentation:**
- [ ] API Documentation
- [ ] Database Schema Documentation
- [ ] Architecture Documentation
- [ ] Deployment Guide
- [ ] Troubleshooting Guide

### **User Documentation:**
- [ ] User Manual (Admin)
- [ ] User Manual (Librarian)
- [ ] User Manual (Reader)
- [ ] FAQ
- [ ] Video Tutorials (optional)

### **Developer Documentation:**
- [ ] README.md
- [ ] CONTRIBUTING.md
- [ ] CODE_OF_CONDUCT.md
- [ ] CHANGELOG.md
- [ ] Setup Guide

---

## 🔧 CÔNG CỤ & SETUP

### **Development Tools:**
```
- VS Code (Backend & Frontend)
- Postman/Insomnia (API testing)
- Pytest (Backend testing)
- Jest (Frontend testing - optional)
- Markdown editors
- Screen recorder (for tutorials)
```

### **VS Code Extensions:**
```
- Python
- Pylance
- Django
- ES7+ React snippets
- Markdown All in One
- GitLens
- Better Comments
```

### **Daily Workflow:**
```bash
# Check what to work on
# Look at GitHub issues/project board

# Backend task
git checkout develop
git pull origin develop
git checkout -b feature/your-feature
# Work on backend...
git add .
git commit -m "feat(module): description"
git push

# Frontend task
# Switch to frontend folder
# Work on frontend...

# Testing
pytest  # Backend
npm test  # Frontend

# Documentation
# Edit markdown files
```

---

## 📊 KPI & METRICS

### **Week 1-4:**
- ✅ Documentation started
- ✅ Auth tests complete
- ✅ Category feature done

### **Week 5-8:**
- ✅ Fine calculation working
- ✅ Search improved
- ✅ Notifications functional
- ✅ Celery tasks tested

### **Week 9-12:**
- ✅ Export working
- ✅ All bugs fixed
- ✅ User manual complete
- ✅ Test coverage > 75%

---

## 🤝 COLLABORATION

### **With Dev 1 (Backend Lead):**
- Help với backend features
- Write tests for his code
- Review his PRs
- Document his APIs

### **With Dev 2 (Frontend Lead):**
- Help với frontend features
- Test UI components
- Review her PRs
- Document UI flows

### **Code Review Checklist:**
- Code quality good?
- Tests included?
- Documentation updated?
- No security issues?
- Performance OK?

---

## 🆘 SUPPORT & COMMUNICATION

### **Daily Standup (10 phút mỗi sáng):**
- Hôm qua làm gì?
- Hôm nay làm gì?
- Có blockers không?

### **Weekly Review:**
- Report test coverage
- Report bugs found
- Demo features completed
- Update documentation

### **Communication:**
- **Questions:** Ask Dev 1 or Dev 2
- **Bugs:** Create GitHub issues
- **Documentation:** Update docs/ folder
- **Testing:** Report in team chat

---

## 📚 HỌC LIỆU THAM KHẢO

### **Backend:**
- [Django Testing](https://docs.djangoproject.com/en/5.0/topics/testing/)
- [Pytest Django](https://pytest-django.readthedocs.io/)
- [DRF Testing](https://www.django-rest-framework.org/api-guide/testing/)

### **Frontend:**
- [Jest](https://jestjs.io/)
- [React Testing Library](https://testing-library.com/react)
- [Testing Best Practices](https://kentcdodds.com/blog/common-mistakes-with-react-testing-library)

### **Documentation:**
- [Markdown Guide](https://www.markdownguide.org/)
- [Writing Great Documentation](https://documentation.divio.com/)

### **General:**
- [Software Testing](https://martinfowler.com/testing/)
- [Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html)

---

**📝 Last Updated:** November 2, 2025  
**👤 Developer:** Fullstack Developer (Support)  
**📧 Contact:** Team Lead for questions

---

## 💡 TIPS FOR SUCCESS

1. **Hỏi khi không hiểu** - Đừng ngại hỏi Dev 1 hoặc Dev 2
2. **Test thoroughly** - Quality là ưu tiên
3. **Document everything** - Future you sẽ cảm ơn
4. **Stay organized** - Track tasks trong GitHub Projects
5. **Learn continuously** - Đây là cơ hội học fullstack
6. **Be proactive** - Tìm bugs trước khi users tìm ra
7. **Communicate well** - Update team về progress
8. **Help teammates** - Success của team là success của bạn

**Good luck! 🚀**
