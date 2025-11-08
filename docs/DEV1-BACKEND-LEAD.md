# 👨‍💻 DEVELOPER 1 - BACKEND LEAD

**Role:** Backend Lead Developer  
**Focus:** Django, REST API, Database, Authentication, Core Business Logic  
**Duration:** 3 months (12 weeks)  
**Responsibility:** 40% Backend core features

---

## 📋 TỔNG QUAN NHIỆM VỤ

### **Vai trò chính:**
- ✅ Thiết lập và cấu hình Django backend
- ✅ Thiết kế và implement database models
- ✅ Xây dựng REST API endpoints
- ✅ Implement authentication & authorization
- ✅ Business logic cho borrowing system
- ✅ Backend cho reports và statistics
- ✅ Review code của Dev 2 và Dev 3 cho backend tasks

### **Tech Stack:**
- Django 5.0
- Django REST Framework
- JWT Authentication
- MySQL/SQLite
- Celery + Redis
- Python qrcode library

---

## 📝 CHI TIẾT CÔNG VIỆC

### **A. DATABASE MODELS (15 Models)**

#### **1. Authentication & Users Module**
```python
# users/models.py
- User (extend Django User)
  * username, email, password, first_name, last_name
  * is_active, is_staff, date_joined
  
- Role
  * id, name (ADMIN/LIBRARIAN/READER)
  * code, description, permissions
  
- Profile
  * user (OneToOne)
  * role (ForeignKey)
  * phone, address, date_of_birth
  * student_id, faculty, grade (for readers)
  * profile_picture
  * created_at, updated_at
```

#### **2. Books Module**
```python
# books/models.py
- Category
  * id, name, code
  * parent (self ForeignKey - tree structure)
  * description, icon
  * created_at, updated_at
  
- Author
  * id, name, bio
  * nationality, birth_year
  * created_at, updated_at
  
- Publisher
  * id, name, address
  * phone, email, website
  * country
  * created_at, updated_at
  
- Book
  * id, title, isbn, barcode
  * author (ForeignKey), publisher (ForeignKey)
  * category (ForeignKey)
  * publication_year, language, pages
  * description, cover_image
  * quantity, available
  * location_shelf, location_row
  * qr_code (ImageField - auto-generate)
  * price, status (ACTIVE/INACTIVE)
  * created_at, updated_at
```

#### **3. Borrowing Module**
```python
# borrowing/models.py
- BorrowRecord
  * id, reader (ForeignKey User)
  * librarian (ForeignKey User)
  * borrow_date, due_date, return_date
  * status (BORROWED/RETURNED/OVERDUE)
  * total_fine
  * notes
  * created_at, updated_at
  
- BorrowDetail
  * id, borrow_record (ForeignKey)
  * book (ForeignKey)
  * due_date, return_date
  * extension_count
  * fine_amount
  * status (BORROWED/RETURNED/OVERDUE/LOST)
  * condition_borrow, condition_return
  * notes
  * created_at, updated_at
  
- Violation
  * id, borrow_detail (ForeignKey)
  * violation_type (OVERDUE/DAMAGED/LOST)
  * fine_amount
  * paid_amount, paid_date
  * payment_status (UNPAID/PARTIAL/PAID)
  * description
  * resolved, resolved_by, resolved_at
  * created_at, updated_at
```

#### **4. Notifications Module**
```python
# notifications/models.py
- Notification
  * id, user (ForeignKey)
  * title, message
  * notification_type (DUE_SOON/OVERDUE/AVAILABLE/ANNOUNCEMENT)
  * link
  * is_read, read_at
  * created_at
```

#### **5. System Module**
```python
# system/models.py
- SystemConfig
  * id, key, value
  * data_type (STRING/INT/FLOAT/BOOLEAN/JSON)
  * description
  * updated_by, updated_at
  
- ActivityLog
  * id, user (ForeignKey)
  * action (CREATE/UPDATE/DELETE/LOGIN/LOGOUT)
  * model_name, object_id
  * changes (JSONField)
  * ip_address, user_agent
  * created_at
```

#### **6. IoT Module (Phase 2)**
```python
# iot/models.py
- IoTDevice
  * id, device_id, name
  * device_type (ESP8266/ESP32)
  * location_shelf, location_row
  * ip_address, mac_address
  * status (ONLINE/OFFLINE)
  * last_ping
  * created_at, updated_at
  
- IoTCommand
  * id, device (ForeignKey)
  * command_type (BLINK/OFF)
  * book_location
  * status (PENDING/SENT/COMPLETED/FAILED)
  * sent_at, completed_at
  * created_at
```

---

### **B. REST API ENDPOINTS (48+ Endpoints)**

#### **1. Authentication APIs (7 endpoints)**
```python
# users/urls.py

POST   /api/auth/register/
  Request: {username, email, password, first_name, last_name, 
            phone, student_id}
  Response: {id, username, email, token}
  
POST   /api/auth/login/
  Request: {username, password}
  Response: {access, refresh, user: {id, username, email, role}}
  
POST   /api/auth/refresh/
  Request: {refresh}
  Response: {access}
  
POST   /api/auth/logout/
  Request: {refresh}
  Response: {message}
  
GET    /api/auth/me/
  Headers: Authorization: Bearer <token>
  Response: {id, username, email, profile: {...}}
  
PUT    /api/auth/profile/
  Request: {phone, address, profile_picture}
  Response: {profile data}
  
POST   /api/auth/change-password/
  Request: {old_password, new_password}
  Response: {message}
```

#### **2. User Management APIs (5 endpoints)**
```python
# users/urls.py

GET    /api/users/
  Params: ?role=READER&search=John&page=1
  Response: {count, results: [{id, username, email, role}]}
  
POST   /api/users/
  Request: {username, email, password, role_id, profile_data}
  Response: {user data}
  
GET    /api/users/{id}/
  Response: {id, username, email, profile: {...}}
  
PUT    /api/users/{id}/
  Request: {email, first_name, last_name, profile_data}
  Response: {updated user}
  
DELETE /api/users/{id}/
  Response: {message}
```

#### **3. Book Management APIs (12 endpoints)**
```python
# books/urls.py

GET    /api/books/
  Params: ?category=1&author=2&search=Python&available_only=true
          &ordering=-created_at&page=1
  Response: {count, results: [{id, title, isbn, author_name, 
            available, cover_image_url}]}
  
POST   /api/books/
  Request: FormData {
    title, isbn, author_id, publisher_id, category_id,
    publication_year, language, pages, description,
    cover_image, quantity, location_shelf, location_row, price
  }
  Response: {book data with qr_code_url}
  
GET    /api/books/{id}/
  Response: {full book data, author_detail, publisher_detail,
            category_detail, qr_code_url}
  
PUT    /api/books/{id}/
  Request: FormData {fields to update}
  Response: {updated book}
  
DELETE /api/books/{id}/
  Response: {message}
  
GET    /api/books/{id}/qr_code/
  Response: {qr_code_url, qr_data: "BOOK:123"}
  
POST   /api/books/scan_qr/
  Request: {qr_data: "BOOK:123"}
  Response: {book data}
  
POST   /api/books/{id}/locate/
  Response: {message, location: {shelf, row}}
  
POST   /api/books/generate_qr_batch/
  Request: {book_ids: [1,2,3]}
  Response: {generated_count, qr_codes: [{book_id, url}]}
  
GET    /api/categories/
  Response: [{id, name, code, children: []}]
  
GET    /api/authors/?search=John
  Response: [{id, name, nationality}]
  
GET    /api/publishers/?search=Penguin
  Response: [{id, name, address}]
```

#### **4. Borrowing APIs (8 endpoints)**
```python
# borrowing/urls.py

POST   /api/borrowing/borrow/
  Request: {
    reader_id: 1,
    book_ids: [1, 2, 3],
    notes: "..."
  }
  Response: {
    borrow_record: {id, reader, due_date, details: [...]}
  }
  
POST   /api/borrowing/return/
  Request: {
    borrow_detail_ids: [1, 2],
    conditions: ["GOOD", "DAMAGED"],
    notes: "Book cover torn"
  }
  Response: {
    returned_count: 2,
    fines: [{detail_id, fine_amount}]
  }
  
POST   /api/borrowing/extend/
  Request: {borrow_detail_id: 1}
  Response: {new_due_date, extension_count}
  
GET    /api/borrowing/my-borrows/
  Params: ?status=BORROWED&page=1
  Response: {count, results: [{id, books, due_date, status}]}
  
GET    /api/borrowing/all/
  Params: ?reader=1&status=OVERDUE&from_date=2024-01-01
  Response: {count, results: [{id, reader_name, books, status}]}
  
GET    /api/borrowing/{id}/
  Response: {full borrow record, details, violations}
  
GET    /api/borrowing/overdue/
  Response: [{borrow_id, reader, overdue_days, fine}]
  
POST   /api/borrowing/pay-fine/
  Request: {violation_id: 1, amount: 50000}
  Response: {remaining_amount}
```

#### **5. Notification APIs (4 endpoints)**
```python
# notifications/urls.py

GET    /api/notifications/
  Params: ?is_read=false&page=1
  Response: {count, results: [{id, title, message, type, is_read}]}
  
GET    /api/notifications/{id}/
  Response: {notification detail}
  
PUT    /api/notifications/{id}/read/
  Response: {message}
  
POST   /api/notifications/mark-all-read/
  Response: {updated_count}
```

#### **6. Reports & Statistics APIs (8 endpoints)**
```python
# system/urls.py

GET    /api/reports/dashboard-stats/
  Params: ?from_date=2024-01-01&to_date=2024-12-31
  Response: {
    total_books, total_users, total_borrows,
    active_borrows, overdue_borrows,
    books_by_category: [{category, count}],
    borrowing_trend: [{date, count}]
  }
  
GET    /api/reports/books-by-category/
  Response: [{category_name, book_count, borrow_count}]
  
GET    /api/reports/most-borrowed/
  Params: ?limit=10
  Response: [{book_id, title, borrow_count}]
  
GET    /api/reports/overdue-stats/
  Response: {
    total_overdue, total_fine,
    overdue_by_reader: [{reader, count}]
  }
  
GET    /api/reports/user-activity/
  Params: ?user_id=1&from_date=...
  Response: [{date, action, description}]
  
GET    /api/reports/borrowing-report/
  Params: ?from_date=...&to_date=...&format=json
  Response: [{borrow_id, reader, books, dates, status}]
  
GET    /api/reports/export/
  Params: ?report_type=borrowing&format=csv&from_date=...
  Response: CSV file download
  
GET    /api/reports/library-statistics/
  Response: {
    total_books, total_value, 
    books_by_language, books_by_year,
    average_books_per_borrow
  }
```

#### **7. System Configuration APIs (4 endpoints)**
```python
# system/urls.py

GET    /api/system/config/
  Response: {
    BORROW_DURATION_DAYS: 14,
    MAX_BORROW_BOOKS: 5,
    MAX_EXTENSION_COUNT: 2,
    FINE_PER_DAY: 5000,
    REMINDER_BEFORE_DUE_DAYS: 3
  }
  
PUT    /api/system/config/
  Request: {key: value pairs}
  Response: {updated configs}
  
GET    /api/system/activity-log/
  Params: ?user=1&action=CREATE&page=1
  Response: [{user, action, model, changes, timestamp}]
  
GET    /api/system/health/
  Response: {
    status: "healthy",
    database: "connected",
    redis: "connected",
    celery: "running"
  }
```

---

### **C. BUSINESS LOGIC IMPLEMENTATIONS**

#### **1. Book Availability Management**
```python
# books/models.py - Book model
def reduce_availability(quantity):
    """Reduce available count when borrowed"""
    if self.available >= quantity:
        self.available -= quantity
        self.save()
        return True
    return False

def increase_availability(quantity):
    """Increase available count when returned"""
    self.available = min(self.available + quantity, self.quantity)
    self.save()
```

#### **2. QR Code Generation**
```python
# books/models.py - Book model
def generate_qr_code():
    """Auto-generate QR code for book"""
    qr_data = f"BOOK:{self.id}"
    qr = qrcode.QRCode(version=1, box_size=10, border=5)
    qr.add_data(qr_data)
    qr.make(fit=True)
    img = qr.make_image(fill_color="black", back_color="white")
    # Save to ImageField
```

#### **3. Borrowing Rules Validation**
```python
# borrowing/views.py
def validate_borrow_request(reader, books):
    """Validate borrowing rules"""
    # Check: Max books limit
    current_borrows = reader.active_borrows.count()
    if current_borrows + len(books) > MAX_BORROW_BOOKS:
        raise ValidationError("Exceeded maximum borrow limit")
    
    # Check: Has overdue books
    if reader.has_overdue_books():
        raise ValidationError("Cannot borrow: you have overdue books")
    
    # Check: Has unpaid fines
    if reader.has_unpaid_fines():
        raise ValidationError("Please pay outstanding fines first")
    
    # Check: Book availability
    for book in books:
        if book.available < 1:
            raise ValidationError(f"Book '{book.title}' is not available")
```

#### **4. Fine Calculation**
```python
# borrowing/models.py - BorrowDetail model
def calculate_fine():
    """Calculate overdue fine"""
    if self.status != 'RETURNED':
        days_late = (date.today() - self.due_date).days
    else:
        days_late = (self.return_date.date() - self.due_date).days
    
    if days_late > 0:
        fine_per_day = SystemConfig.get_value('FINE_PER_DAY', 5000)
        return days_late * fine_per_day
    return 0

def extend_due_date():
    """Extend due date (max 2 times)"""
    if self.extension_count >= MAX_EXTENSION_COUNT:
        raise ValidationError("Maximum extensions reached")
    
    extension_days = SystemConfig.get_value('BORROW_DURATION_DAYS', 14)
    self.due_date += timedelta(days=extension_days)
    self.extension_count += 1
    self.save()
```

#### **5. Borrow & Return Flow**
```python
# borrowing/views.py

@action(detail=False, methods=['post'])
def borrow_books(request):
    """Process book borrowing"""
    reader = request.data.get('reader_id')
    book_ids = request.data.get('book_ids')
    
    # Validate
    validate_borrow_request(reader, books)
    
    # Create BorrowRecord
    borrow_record = BorrowRecord.objects.create(
        reader=reader,
        librarian=request.user,
        due_date=date.today() + timedelta(days=14)
    )
    
    # Create BorrowDetails
    for book in books:
        BorrowDetail.objects.create(
            borrow_record=borrow_record,
            book=book,
            due_date=borrow_record.due_date
        )
        book.reduce_availability(1)
    
    # Send notification
    send_borrow_confirmation_email.delay(borrow_record.id)
    
    return Response({borrow_record data})

@action(detail=False, methods=['post'])
def return_books(request):
    """Process book return"""
    detail_ids = request.data.get('borrow_detail_ids')
    conditions = request.data.get('conditions')
    
    for detail_id, condition in zip(detail_ids, conditions):
        detail = BorrowDetail.objects.get(id=detail_id)
        detail.status = 'RETURNED'
        detail.return_date = timezone.now()
        detail.condition_return = condition
        
        # Calculate fine
        fine = detail.calculate_fine()
        if fine > 0:
            Violation.objects.create(
                borrow_detail=detail,
                violation_type='OVERDUE',
                fine_amount=fine
            )
        
        # Increase availability
        detail.book.increase_availability(1)
        detail.save()
    
    return Response({return data})
```

---

### **D. CELERY SCHEDULED TASKS**

#### **Task 1: Check Overdue Books (Daily at 8 AM)**
```python
# borrowing/tasks.py

@shared_task
def check_overdue_books():
    """Check and update overdue books daily"""
    today = date.today()
    
    # Find overdue borrow details
    overdue_details = BorrowDetail.objects.filter(
        due_date__lt=today,
        status='BORROWED'
    )
    
    for detail in overdue_details:
        # Update status
        detail.status = 'OVERDUE'
        detail.fine_amount = detail.calculate_fine()
        detail.save()
        
        # Update borrow record
        detail.borrow_record.status = 'OVERDUE'
        detail.borrow_record.save()
        
        # Create notification
        Notification.objects.create(
            user=detail.borrow_record.reader,
            title='Sách quá hạn',
            message=f'Sách "{detail.book.title}" đã quá hạn.',
            notification_type='OVERDUE'
        )
        
        # Send email
        send_overdue_email.delay(detail.id)
    
    return f"Processed {overdue_details.count()} overdue books"
```

#### **Task 2: Send Due Soon Reminders (Daily at 9 AM)**
```python
@shared_task
def send_due_soon_reminders():
    """Send reminders for books due soon"""
    reminder_days = SystemConfig.get_value('REMINDER_BEFORE_DUE_DAYS', 3)
    reminder_date = date.today() + timedelta(days=reminder_days)
    
    # Find books due soon
    due_soon_details = BorrowDetail.objects.filter(
        due_date=reminder_date,
        status='BORROWED'
    )
    
    for detail in due_soon_details:
        # Create notification
        Notification.objects.create(
            user=detail.borrow_record.reader,
            title='Sách sắp đến hạn',
            message=f'Sách "{detail.book.title}" sẽ đến hạn vào {detail.due_date}.',
            notification_type='DUE_SOON'
        )
        
        # Send email
        send_due_soon_email.delay(detail.id)
    
    return f"Sent {due_soon_details.count()} reminders"
```

#### **Task 3: Generate Reports (Weekly on Monday)**
```python
@shared_task
def generate_weekly_report():
    """Generate weekly library activity report"""
    last_week = date.today() - timedelta(days=7)
    
    report_data = {
        'total_borrows': BorrowRecord.objects.filter(
            created_at__gte=last_week
        ).count(),
        'total_returns': BorrowDetail.objects.filter(
            return_date__gte=last_week
        ).count(),
        'new_books': Book.objects.filter(
            created_at__gte=last_week
        ).count(),
        'active_users': Profile.objects.filter(
            user__last_login__gte=last_week
        ).count()
    }
    
    # Send report email to admin
    send_weekly_report_email.delay(report_data)
    
    return report_data
```

---

### **E. EMAIL TEMPLATES**

#### **1. Borrow Confirmation Email**
```html
<!-- templates/emails/borrow_confirmation.html -->
Subject: Xác nhận mượn sách

Xin chào {{reader_name}},

Bạn đã mượn thành công các sách sau:
{% for detail in borrow_details %}
  - {{detail.book.title}} (Hạn trả: {{detail.due_date}})
{% endfor %}

Tổng số sách: {{total_books}}
Ngày mượn: {{borrow_date}}
Ngày hết hạn: {{due_date}}

Vui lòng trả sách đúng hạn để tránh phí phạt.
```

#### **2. Overdue Notice Email**
```html
<!-- templates/emails/overdue_notice.html -->
Subject: Thông báo sách quá hạn

Xin chào {{reader_name}},

Sách "{{book_title}}" đã quá hạn {{days_overdue}} ngày.

Hạn trả: {{due_date}}
Phí phạt hiện tại: {{fine_amount}} VNĐ

Vui lòng trả sách sớm nhất có thể.
```

#### **3. Due Soon Reminder Email**
```html
<!-- templates/emails/due_soon_reminder.html -->
Subject: Nhắc nhở sách sắp đến hạn

Xin chào {{reader_name}},

Sách "{{book_title}}" sẽ đến hạn vào {{due_date}}.

Bạn có thể:
- Trả sách trước hạn
- Gia hạn (tối đa 2 lần)

Liên hệ thư viện nếu cần hỗ trợ.
```

---

### **F. PERMISSIONS & SECURITY**

#### **Custom Permission Classes**
```python
# users/permissions.py

class IsAdmin(BasePermission):
    def has_permission(self, request, view):
        return request.user.profile.role.code == 'ADMIN'

class IsLibrarian(BasePermission):
    def has_permission(self, request, view):
        return request.user.profile.role.code in ['ADMIN', 'LIBRARIAN']

class IsReader(BasePermission):
    def has_permission(self, request, view):
        return request.user.profile.role.code == 'READER'

class IsOwnerOrAdmin(BasePermission):
    def has_object_permission(self, request, view, obj):
        return (obj.user == request.user or 
                request.user.profile.role.code == 'ADMIN')
```

#### **API Security Measures**
```python
# settings.py

REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ],
    'DEFAULT_THROTTLE_CLASSES': [
        'rest_framework.throttling.AnonRateThrottle',
        'rest_framework.throttling.UserRateThrottle'
    ],
    'DEFAULT_THROTTLE_RATES': {
        'anon': '100/hour',
        'user': '1000/hour'
    }
}
```

---

## 📊 TỔNG KẾT CÔNG VIỆC BACKEND

| Category | Count | Details |
|----------|-------|---------|
| **Models** | 15 | User, Role, Profile, Category, Author, Publisher, Book, BorrowRecord, BorrowDetail, Violation, Notification, SystemConfig, ActivityLog, IoTDevice, IoTCommand |
| **API Endpoints** | 48+ | Auth (7), Users (5), Books (12), Borrowing (8), Notifications (4), Reports (8), System (4) |
| **Celery Tasks** | 3 | Check overdue, Send reminders, Weekly reports |
| **Email Templates** | 3 | Borrow confirmation, Overdue notice, Due reminder |
| **Permissions** | 4 | IsAdmin, IsLibrarian, IsReader, IsOwnerOrAdmin |
| **Business Logic** | 5+ | Availability, QR generation, Validation, Fine calculation, Borrow/Return flow |

---

## 📅 TIMELINE CHI TIẾT

### **THÁNG 1: FOUNDATION & AUTHENTICATION**

#### **Week 1: Project Setup (5 days)**
**Branch:** `feature/backend-setup`

**Monday - Tuesday (2 days):**
- [ ] Setup Python virtual environment
- [ ] Install Django 5.0, DRF, dependencies
- [ ] Create Django project structure
- [ ] Configure `settings.py` (dev/prod)
- [ ] Setup `.env` file
- [ ] Configure MySQL database
- [ ] Create apps: `users`, `books`, `borrowing`, `notifications`, `system`, `iot`

```bash
# Commands
python -m venv venv
.\venv\Scripts\activate
pip install django djangorestframework djangorestframework-simplejwt
pip install mysqlclient python-decouple django-cors-headers
pip install celery redis pillow qrcode django-filter

django-admin startproject library_backend
cd library_backend
python manage.py startapp users
python manage.py startapp books
python manage.py startapp borrowing
python manage.py startapp notifications
python manage.py startapp system
python manage.py startapp iot
```

**Wednesday - Thursday (2 days):**
- [ ] Configure `INSTALLED_APPS`
- [ ] Setup CORS headers
- [ ] Configure static/media files
- [ ] Setup logging
- [ ] Create base model classes
- [ ] Setup Django REST Framework settings
- [ ] Configure JWT authentication
- [ ] Create requirements.txt

**Friday:**
- [ ] Test server runs successfully
- [ ] Create initial migration
- [ ] Document setup process
- [ ] Code review với team
- [ ] Commit và push: `feat(setup): initialize Django backend project`

**Deliverables:**
- ✅ Django project chạy được
- ✅ Database connection thành công
- ✅ Documentation về setup

---

#### **Week 2-3: Authentication System (10 days)**
**Branch:** `feature/jwt-backend`

**Week 2 - Monday-Wednesday (3 days):**
- [ ] Extend Django User model
- [ ] Create `Role` model (ADMIN, LIBRARIAN, READER)
- [ ] Create `Profile` model
- [ ] Implement custom user manager
- [ ] Create migrations

**Models to create:**
```python
# users/models.py
- Role (id, name, code, description)
- Profile (user, role, phone, address, student_id, etc.)
```

**Week 2 - Thursday-Friday (2 days):**
- [ ] Create User serializers (UserSerializer, ProfileSerializer)
- [ ] Implement JWT login endpoint
- [ ] Implement JWT refresh endpoint
- [ ] Implement logout (blacklist token)
- [ ] Add password change endpoint

**Week 3 - Monday-Wednesday (3 days):**
- [ ] Implement role-based permissions
- [ ] Create custom permission classes
- [ ] Add user registration endpoint (for readers)
- [ ] Add user CRUD endpoints (admin only)
- [ ] Add profile update endpoint

**Week 3 - Thursday-Friday (2 days):**
- [ ] Write unit tests for authentication
- [ ] Test JWT token flow
- [ ] Test permissions
- [ ] Document API endpoints
- [ ] Code review
- [ ] Commit: `feat(auth): implement JWT authentication and user management`

**API Endpoints:**
```
POST   /api/auth/register/
POST   /api/auth/login/
POST   /api/auth/refresh/
POST   /api/auth/logout/
GET    /api/auth/me/
PUT    /api/auth/profile/
POST   /api/auth/change-password/

GET    /api/users/         (admin)
POST   /api/users/         (admin)
GET    /api/users/{id}/    (admin)
PUT    /api/users/{id}/    (admin)
DELETE /api/users/{id}/    (admin)
```

**Deliverables:**
- ✅ Authentication system hoạt động
- ✅ JWT tokens được issue và validate
- ✅ Role-based permissions
- ✅ API documentation

---

#### **Week 4: Book Management Backend (5 days)**
**Branch:** `feature/book-models`

**Monday-Tuesday (2 days):**
- [ ] Create `Category` model với parent relationship
- [ ] Create `Author` model
- [ ] Create `Publisher` model
- [ ] Create `Book` model với all fields
- [ ] Add QR code auto-generation trong Book model
- [ ] Create migrations

**Wednesday-Thursday (2 days):**
- [ ] Create serializers (BookSerializer, CategorySerializer, etc.)
- [ ] Implement ViewSets for all book-related models
- [ ] Add filtering (DjangoFilter)
- [ ] Add search functionality
- [ ] Add pagination

**Friday:**
- [ ] Add custom action: `generate_qr_code()`
- [ ] Add custom action: `scan_qr()`
- [ ] Test CRUD operations
- [ ] Write unit tests
- [ ] Document endpoints
- [ ] Commit: `feat(books): implement book management backend`

**API Endpoints:**
```
GET    /api/books/
POST   /api/books/
GET    /api/books/{id}/
PUT    /api/books/{id}/
DELETE /api/books/{id}/
GET    /api/books/{id}/qr_code/
POST   /api/books/scan_qr/

GET    /api/categories/
POST   /api/categories/
GET    /api/authors/
POST   /api/authors/
GET    /api/publishers/
POST   /api/publishers/
```

**Deliverables:**
- ✅ Book CRUD API
- ✅ QR code generation
- ✅ Search và filter

---

### **THÁNG 2: CORE FEATURES**

#### **Week 5: Borrowing System Backend (5 days)**
**Branch:** `feature/borrow-logic`

**Monday-Tuesday (2 days):**
- [ ] Create `BorrowRecord` model
- [ ] Create `BorrowDetail` model
- [ ] Create `Violation` model
- [ ] Implement business logic:
  - Check book availability
  - Maximum borrow limit
  - Calculate due date
- [ ] Create migrations

**Wednesday-Thursday (2 days):**
- [ ] Create serializers
- [ ] Implement BorrowViewSet
- [ ] Add custom action: `borrow_books()`
- [ ] Add custom action: `return_books()`
- [ ] Add custom action: `extend_due_date()`
- [ ] Implement fine calculation logic

**Friday:**
- [ ] Add validation rules
- [ ] Test borrow/return flow
- [ ] Write unit tests
- [ ] Document business rules
- [ ] Commit: `feat(borrowing): implement borrowing system backend`

**API Endpoints:**
```
POST   /api/borrowing/borrow/
POST   /api/borrowing/return/
POST   /api/borrowing/extend/
GET    /api/borrowing/my-borrows/
GET    /api/borrowing/overdue/
GET    /api/borrowing/{id}/
```

**Deliverables:**
- ✅ Borrowing logic hoạt động
- ✅ Book availability tracking
- ✅ Fine calculation

---

#### **Week 6: QR Code Backend (5 days)**
**Branch:** `feature/qr-generation`

**Monday-Wednesday (3 days):**
- [ ] Improve QR code generation function
- [ ] Add batch QR generation endpoint
- [ ] Implement QR scanning validation
- [ ] Add QR code to borrowing flow
- [ ] Create QR history log

**Thursday-Friday (2 days):**
- [ ] Test QR generation for all books
- [ ] Test scanning flow
- [ ] Optimize QR image storage
- [ ] Write tests
- [ ] Commit: `feat(qr): implement QR code generation and scanning`

**API Endpoints:**
```
POST   /api/books/generate-qr-batch/
POST   /api/books/scan-qr/
GET    /api/books/{id}/download-qr/
```

**Deliverables:**
- ✅ QR codes for all books
- ✅ Scanning API works

---

#### **Week 7: Reader History & Backend Support (5 days)**
**Branch:** `feature/borrow-history`

**Monday-Wednesday (3 days):**
- [ ] Add endpoint: Get user's borrow history
- [ ] Add filtering by status, date range
- [ ] Add endpoint: Get user's notifications
- [ ] Add endpoint: Mark notification as read
- [ ] Optimize queries với `select_related`, `prefetch_related`

**Thursday-Friday (2 days):**
- [ ] Add statistics endpoints for reader
- [ ] Add wishlist functionality (optional)
- [ ] Write tests
- [ ] Commit: `feat(reader): add reader history and profile APIs`

**API Endpoints:**
```
GET    /api/readers/my-history/
GET    /api/readers/my-notifications/
PUT    /api/readers/notifications/{id}/read/
GET    /api/readers/my-stats/
```

---

#### **Week 8: Email System & Celery Tasks (5 days)**
**Branch:** `feature/email-system`

**Monday-Tuesday (2 days):**
- [ ] Configure Django email settings
- [ ] Create email templates (HTML)
- [ ] Implement send email function
- [ ] Test email sending

**Wednesday-Friday (3 days):**
- [ ] Setup Celery with Redis
- [ ] Create Celery tasks:
  - `check_overdue_books()`
  - `send_due_soon_reminders()`
  - `send_overdue_email()`
- [ ] Configure Celery Beat schedule
- [ ] Test scheduled tasks
- [ ] Monitor Celery worker
- [ ] Commit: `feat(notifications): implement email system and Celery tasks`

**Celery Tasks:**
```python
@shared_task
def check_overdue_books()

@shared_task
def send_due_soon_reminders()

@shared_task
def send_overdue_email(detail_id)
```

**Deliverables:**
- ✅ Email gửi được
- ✅ Celery tasks chạy tự động
- ✅ Scheduled reminders

---

### **THÁNG 3: REPORTS & DEPLOYMENT**

#### **Week 9: Reports & Statistics Backend (5 days)**
**Branch:** `feature/report-backend`

**Monday-Tuesday (2 days):**
- [ ] Create report models (nếu cần)
- [ ] Implement statistics calculations:
  - Total books, users, borrows
  - Books by category
  - Most borrowed books
  - Overdue statistics
  - User activity
- [ ] Add date range filtering

**Wednesday-Thursday (2 days):**
- [ ] Create report endpoints
- [ ] Add data aggregation với Django ORM
- [ ] Optimize queries
- [ ] Add caching cho reports

**Friday:**
- [ ] Add export to CSV/PDF functionality
- [ ] Test reports accuracy
- [ ] Write tests
- [ ] Commit: `feat(reports): implement statistics and reports backend`

**API Endpoints:**
```
GET    /api/reports/dashboard-stats/
GET    /api/reports/books-by-category/
GET    /api/reports/most-borrowed/
GET    /api/reports/overdue-stats/
GET    /api/reports/user-activity/
GET    /api/reports/export/?format=csv
```

**Deliverables:**
- ✅ Statistics API
- ✅ Data accuracy verified
- ✅ Export functionality

---

#### **Week 10: Internationalization Backend (5 days)**
**Branch:** `feature/i18n-backend`

**Monday-Wednesday (3 days):**
- [ ] Setup Django i18n
- [ ] Mark strings for translation
- [ ] Create translation files (Vietnamese, English)
- [ ] Add language switch endpoint
- [ ] Configure locale middleware

**Thursday-Friday (2 days):**
- [ ] Test language switching
- [ ] Translate error messages
- [ ] Document i18n usage
- [ ] Commit: `feat(i18n): add internationalization support`

**Deliverables:**
- ✅ Multi-language support
- ✅ Vietnamese + English

---

#### **Week 11: Testing & Bug Fixes (5 days)**
**Branch:** `test/integration-tests`

**Monday-Wednesday (3 days):**
- [ ] Write integration tests
- [ ] Test complete user flows
- [ ] Test API error handling
- [ ] Fix bugs found during testing
- [ ] Improve error messages

**Thursday-Friday (2 days):**
- [ ] Performance testing
- [ ] Optimize slow queries
- [ ] Add database indexes
- [ ] Security audit
- [ ] Commit: `test: add integration tests and fix bugs`

**Test Coverage:**
- Authentication flow
- Book CRUD operations
- Borrowing complete flow
- QR code scanning
- Email sending
- Reports accuracy

---

#### **Week 12: Deployment Configuration (5 days)**
**Branch:** `feature/deployment-config`

**Monday-Tuesday (2 days):**
- [ ] Create production settings
- [ ] Configure Gunicorn
- [ ] Setup Nginx configuration
- [ ] Configure SSL with Let's Encrypt
- [ ] Setup environment variables

**Wednesday-Thursday (2 days):**
- [ ] Deploy to server
- [ ] Run migrations on production
- [ ] Setup Celery worker on server
- [ ] Setup Celery beat
- [ ] Test production deployment

**Friday:**
- [ ] Monitor logs
- [ ] Fix deployment issues
- [ ] Create deployment documentation
- [ ] Final commit: `chore: production deployment configuration`

**Deliverables:**
- ✅ Backend deployed successfully
- ✅ All services running
- ✅ Deployment docs

---

## 🔧 CÔNG CỤ & SETUP

### **Development Tools:**
```
- VS Code with Python extension
- Postman/Insomnia for API testing
- MySQL Workbench
- Redis Desktop Manager
- Git + GitHub Desktop (optional)
```

### **VS Code Extensions:**
```
- Python
- Pylance
- Django
- SQLite Viewer
- GitLens
- Thunder Client
```

### **Daily Workflow:**
```bash
# Morning routine
git checkout develop
git pull origin develop
git checkout -b feature/your-feature

# During work
# Code...
git add .
git commit -m "feat(module): description"

# Before lunch & end of day
git push origin feature/your-feature

# Create PR when feature complete
# Request review from Dev 2 or Dev 3
```

---

## 📊 KPI & METRICS

### **Week 1-4:**
- ✅ Backend setup complete
- ✅ Authentication working
- ✅ Book CRUD API functional
- ✅ 20+ API endpoints

### **Week 5-8:**
- ✅ Borrowing system complete
- ✅ QR code working
- ✅ Email sending functional
- ✅ Celery tasks running
- ✅ 35+ API endpoints total

### **Week 9-12:**
- ✅ Reports accurate
- ✅ Production deployed
- ✅ 45+ API endpoints total
- ✅ Test coverage > 70%

---

## 📝 CODE REVIEW RESPONSIBILITIES

Bạn sẽ review code của:
- **Dev 2:** Frontend components calling your APIs
- **Dev 3:** Database migrations, tests, notification logic

**Review checklist:**
- API usage đúng?
- Error handling có đủ?
- Security issues?
- Performance concerns?
- Code style consistent?

---

## 🆘 SUPPORT & COMMUNICATION

### **Daily Standup (10 phút mỗi sáng):**
- Hôm qua làm gì?
- Hôm nay làm gì?
- Có blockers không?

### **Weekly Sync (30 phút mỗi Friday):**
- Demo features hoàn thành
- Review progress
- Plan next week

### **Communication Channels:**
- **Urgent:** Gọi điện/meeting
- **Questions:** Team chat
- **Code review:** GitHub PR comments
- **Documentation:** Update docs/

---

## 📚 HỌC LIỆU THAM KHẢO

- [Django Documentation](https://docs.djangoproject.com/)
- [Django REST Framework](https://www.django-rest-framework.org/)
- [JWT Authentication](https://django-rest-framework-simplejwt.readthedocs.io/)
- [Celery Documentation](https://docs.celeryproject.org/)
- [Python QRCode](https://pypi.org/project/qrcode/)

---

**📝 Last Updated:** November 2, 2025  
**👤 Developer:** Backend Lead  
**📧 Contact:** Team Lead for questions
