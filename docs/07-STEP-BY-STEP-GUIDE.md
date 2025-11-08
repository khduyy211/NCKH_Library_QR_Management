# HƯỚNG DẪN TỪNG BƯỚC - CÁC CÔNG NGHỆ VÀ CÁCH SỬ DỤNG

## 📚 MỤC LỤC

1. [Giới thiệu các công nghệ](#giới-thiệu-các-công-nghệ)
2. [Cài đặt môi trường phát triển](#cài-đặt-môi-trường-phát-triển)
3. [Thiết lập Backend (Django)](#thiết-lập-backend-django)
4. [Thiết lập Frontend (React)](#thiết-lập-frontend-react)
5. [Kết nối Backend và Frontend](#kết-nối-backend-và-frontend)
6. [Triển khai tính năng cụ thể](#triển-khai-tính-năng-cụ-thể)
7. [Testing và Debugging](#testing-và-debugging)
8. [Deployment](#deployment)

---

## 1. GIỚI THIỆU CÁC CÔNG NGHỆ

### 🐍 **Backend: Django & Django REST Framework**

#### **Django là gì?**
- **Web framework** viết bằng Python
- **MVC pattern** (Model-View-Controller) → trong Django gọi là **MVT** (Model-View-Template)
- **ORM** (Object-Relational Mapping) để làm việc với database
- **Admin interface** có sẵn
- **Security features** tích hợp (CSRF, XSS protection)

#### **Tại sao dùng Django?**
✅ Nhanh, mạnh mẽ, an toàn  
✅ Documentation tốt  
✅ Community lớn  
✅ Nhiều packages có sẵn  
✅ Phù hợp cho dự án vừa và lớn  

#### **Django REST Framework (DRF) là gì?**
- **Thư viện** để xây dựng **RESTful API**
- **Serialization**: Chuyển đổi Python objects ↔ JSON
- **ViewSets**: Class-based views cho CRUD operations
- **Authentication**: JWT, Token, Session
- **Permissions**: Phân quyền chi tiết

#### **Vai trò trong dự án:**
```
Django Backend:
├── Xử lý business logic
├── Quản lý database (MySQL)
├── Cung cấp REST API cho Frontend
├── Authentication & Authorization
├── Background tasks (Celery)
└── Email sending
```

---

### ⚛️ **Frontend: React & TypeScript**

#### **React là gì?**
- **JavaScript library** để xây dựng UI
- **Component-based**: Chia UI thành các components nhỏ
- **Virtual DOM**: Render nhanh, hiệu quả
- **Declarative**: Code dễ đọc, dễ maintain
- **One-way data flow**: Dữ liệu chỉ đi một chiều

#### **Tại sao dùng React?**
✅ Popular, nhiều tài liệu  
✅ Component reusable  
✅ Performance tốt  
✅ React Native có thể dùng sau  
✅ Large ecosystem  

#### **TypeScript là gì?**
- **Superset of JavaScript** (JS có type)
- **Type safety**: Phát hiện lỗi lúc compile
- **Better IDE support**: Autocomplete, IntelliSense
- **Refactoring dễ hơn**

#### **Vai trò trong dự án:**
```
React Frontend:
├── Hiển thị UI cho users
├── Xử lý user interactions
├── Gọi API từ Backend
├── Quản lý state (Redux)
├── Routing giữa các pages
└── Responsive design
```

---

### 🗄️ **Database: MySQL**

#### **MySQL là gì?**
- **Relational Database Management System** (RDBMS)
- **SQL** (Structured Query Language)
- **ACID** compliance (Atomicity, Consistency, Isolation, Durability)
- **Open source**, free

#### **Tại sao dùng MySQL?**
✅ Phổ biến, dễ học  
✅ Performance tốt  
✅ Hỗ trợ tốt từ Django  
✅ Phù hợp cho production  
✅ Backup & restore dễ dàng  

#### **Vai trò trong dự án:**
```
MySQL:
├── Lưu trữ tất cả dữ liệu
│   ├── Users, Books, Borrowings
│   ├── Categories, Authors
│   └── Logs, Notifications
├── Relationships giữa tables
├── Indexes để tăng tốc queries
└── Transactions để đảm bảo data integrity
```

---

### 🔴 **Redis**

#### **Redis là gì?**
- **In-memory data store** (lưu trong RAM)
- **Key-value database**
- **Cache** và **Message broker**
- **Rất nhanh** (microsecond latency)

#### **Vai trò trong dự án:**
```
Redis:
├── Celery broker (message queue)
├── Cache frequently accessed data
├── Session storage (optional)
└── Real-time features (future)
```

---

### 🌿 **Celery**

#### **Celery là gì?**
- **Distributed task queue** (hàng đợi tasks)
- **Asynchronous processing** (xử lý bất đồng bộ)
- **Scheduled tasks** (tasks theo lịch)
- Dùng **Redis** hoặc **RabbitMQ** làm broker

#### **Tại sao cần Celery?**
Một số tasks không nên chạy trong request/response cycle:
- ❌ Gửi email → Lâu, user phải đợi
- ❌ Generate reports → Tốn thời gian
- ❌ Process files → Blocking

✅ **Celery giải quyết**: Chạy background, không block user

#### **Vai trò trong dự án:**
```
Celery:
├── Celery Workers (xử lý tasks)
│   ├── Send emails
│   ├── Generate QR codes
│   └── Process imports
└── Celery Beat (scheduled tasks)
    ├── Check overdue books (daily)
    ├── Send reminders (daily)
    └── Calculate fines (daily)
```

---

### 🔐 **JWT (JSON Web Token)**

#### **JWT là gì?**
- **Token-based authentication**
- **Stateless**: Server không lưu session
- **Format**: `header.payload.signature`
- **Self-contained**: Chứa user info

#### **Luồng JWT:**
```
1. User login (username + password)
   ↓
2. Backend verify → Tạo JWT token
   ↓
3. Frontend lưu token (localStorage)
   ↓
4. Mỗi request gửi token trong header:
   Authorization: Bearer <token>
   ↓
5. Backend verify token → Allow/Deny
```

#### **Tại sao dùng JWT?**
✅ Stateless (dễ scale)  
✅ Mobile-friendly  
✅ Secure  
✅ Expiration built-in  

---

### 🎨 **Bootstrap 5**

#### **Bootstrap là gì?**
- **CSS framework**
- **Responsive grid system**
- **Pre-built components** (buttons, forms, modals, etc.)
- **Utilities classes**

#### **Vai trò trong dự án:**
```
Bootstrap 5:
├── Grid system (responsive layout)
├── Components (buttons, cards, tables)
├── Forms (input, validation styles)
├── Utilities (spacing, colors, flex)
└── JavaScript components (modals, dropdowns)
```

---

### 🌍 **i18n (Internationalization)**

#### **i18n là gì?**
- **Đa ngôn ngữ** (Multi-language)
- **Localization** (L10n): Dịch nội dung
- **Formatting**: Date, time, currency theo locale

#### **Công nghệ:**
- **Backend**: Django i18n
- **Frontend**: react-i18next

#### **Vai trò trong dự án:**
```
i18n:
├── Hỗ trợ Vietnamese & English
├── Language switcher
├── Translate all UI text
├── Date/Time formatting
└── Number formatting
```

---

### 📦 **Các thư viện khác**

#### **Axios**
- **HTTP client** cho React
- **Promise-based**
- **Interceptors**: Xử lý request/response globally

#### **Redux Toolkit**
- **State management** cho React
- **Global state**: Chia sẻ data giữa components
- **Actions & Reducers**

#### **React Router**
- **Client-side routing**
- **Navigation** giữa pages
- **Protected routes**

#### **Chart.js**
- **Visualization library**
- **Charts**: Line, bar, pie, etc.
- **Interactive**

#### **html5-qrcode**
- **QR code scanner**
- **Camera access**
- **Multiple formats**

---

## 2. CÀI ĐẶT MÔI TRƯỜNG PHÁT TRIỂN

### 🔧 **Yêu cầu hệ thống**

#### **Phần mềm cần cài:**
1. **Python 3.11+** - Backend
2. **Node.js 18+** - Frontend
3. **MySQL 8.0+** - Database
4. **Redis 7.0+** - Cache & Queue
5. **Git** - Version control
6. **VS Code** - Code editor (khuyến nghị)

---

### 🐍 **Cài đặt Python & Django**

#### **Bước 1: Cài Python**
```powershell
# Windows - Download từ python.org
# Hoặc dùng Chocolatey:
choco install python --version=3.11

# Verify
python --version
# Output: Python 3.11.x
```

#### **Bước 2: Tạo Virtual Environment**
```powershell
# Navigate to project folder
cd D:\NCKH\demo

# Create virtual environment
python -m venv venv

# Activate (Windows)
.\venv\Scripts\activate

# Verify
(venv) PS D:\NCKH\demo>
```

**❓ Virtual Environment là gì?**
- **Môi trường Python riêng biệt** cho mỗi project
- **Dependencies không conflict** giữa các projects
- **Best practice** trong Python development

#### **Bước 3: Cài Django & Dependencies**
```powershell
# Activate venv trước
.\venv\Scripts\activate

# Upgrade pip
pip install --upgrade pip

# Install Django
pip install django==5.0

# Install DRF
pip install djangorestframework

# Install JWT
pip install djangorestframework-simplejwt

# Install CORS headers
pip install django-cors-headers

# Install MySQL client
pip install mysqlclient

# Install environment variables
pip install python-decouple

# Install Celery & Redis
pip install celery redis django-celery-beat

# Install image processing
pip install pillow python-qrcode

# Install Excel handling
pip install openpyxl

# Install PDF generation
pip install reportlab

# Install API documentation
pip install drf-yasg

# Save to requirements.txt
pip freeze > requirements.txt
```

**💡 Giải thích từng package:**
- `django`: Core framework
- `djangorestframework`: REST API
- `djangorestframework-simplejwt`: JWT authentication
- `django-cors-headers`: CORS handling
- `mysqlclient`: MySQL driver
- `python-decouple`: Environment variables
- `celery`: Task queue
- `redis`: Redis client
- `django-celery-beat`: Scheduled tasks
- `pillow`: Image processing
- `python-qrcode`: QR code generation
- `openpyxl`: Excel files
- `reportlab`: PDF generation
- `drf-yasg`: Swagger documentation

---

### 📦 **Cài đặt Node.js & React**

#### **Bước 1: Cài Node.js**
```powershell
# Windows - Download từ nodejs.org
# Hoặc dùng Chocolatey:
choco install nodejs-lts

# Verify
node --version
# Output: v18.x.x

npm --version
# Output: 9.x.x
```

#### **Bước 2: Tạo React App**
```powershell
# Navigate to project
cd D:\NCKH\demo

# Create React app with TypeScript
npx create-react-app library-frontend --template typescript

# Navigate to frontend
cd library-frontend

# Verify structure
dir
# Output: package.json, src/, public/, etc.
```

**❓ Create-React-App làm gì?**
- **Tạo project structure** hoàn chỉnh
- **Webpack config** tự động
- **Babel transpiler** tự động
- **Development server** built-in
- **Production build** optimization

#### **Bước 3: Cài Dependencies**
```powershell
# Still in library-frontend folder

# React Router
npm install react-router-dom

# State management
npm install @reduxjs/toolkit react-redux

# HTTP client
npm install axios

# UI Framework
npm install bootstrap react-bootstrap

# i18n
npm install react-i18next i18next

# React Query (optional, alternative to Redux)
npm install @tanstack/react-query

# Forms
npm install react-hook-form

# QR Scanner
npm install html5-qrcode

# Charts
npm install chart.js react-chartjs-2

# Date utilities
npm install date-fns

# Notifications
npm install react-toastify

# Icons
npm install @fortawesome/fontawesome-free

# Dev dependencies
npm install -D sass
npm install -D @types/react @types/node

# Verify
cat package.json
```

**💡 Giải thích từng package:**
- `react-router-dom`: Client-side routing
- `@reduxjs/toolkit`: State management (simplified Redux)
- `react-redux`: React bindings for Redux
- `axios`: HTTP requests
- `bootstrap`: CSS framework
- `react-bootstrap`: React components for Bootstrap
- `react-i18next`: Internationalization
- `react-hook-form`: Form validation
- `html5-qrcode`: QR code scanner
- `chart.js`: Charts library
- `date-fns`: Date manipulation
- `react-toastify`: Toast notifications
- `sass`: CSS preprocessor

---

### 🗄️ **Cài đặt MySQL**

#### **Bước 1: Cài MySQL Server**
```powershell
# Windows - Download từ mysql.com
# Hoặc dùng Chocolatey:
choco install mysql

# Hoặc dùng XAMPP (có MySQL + phpMyAdmin)
choco install xampp
```

#### **Bước 2: Tạo Database**
```sql
-- Login to MySQL
mysql -u root -p

-- Create database
CREATE DATABASE library_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- Create user
CREATE USER 'library_user'@'localhost' IDENTIFIED BY 'your_password';

-- Grant privileges
GRANT ALL PRIVILEGES ON library_db.* TO 'library_user'@'localhost';

-- Flush privileges
FLUSH PRIVILEGES;

-- Exit
EXIT;
```

**❓ Character set utf8mb4 là gì?**
- **UTF-8** với **4 bytes** (hỗ trợ emoji, tiếng Việt đầy đủ)
- **Best practice** cho international apps

---

### 🔴 **Cài đặt Redis**

#### **Windows:**
```powershell
# Option 1: Chocolatey
choco install redis-64

# Option 2: Download từ GitHub
# https://github.com/microsoftarchive/redis/releases

# Start Redis
redis-server

# Test Redis
redis-cli
127.0.0.1:6379> ping
# Output: PONG
```

**❓ Redis dùng để làm gì?**
- **Celery broker**: Lưu queue messages
- **Cache**: Lưu data thường xuyên truy cập
- **Session store**: Lưu user sessions (optional)

---

### 🎨 **Cài đặt VS Code Extensions**

```
Khuyến nghị cài các extensions:

Backend:
- Python (Microsoft)
- Pylance
- Django (Baptiste Darthenay)
- MySQL (cweijan)

Frontend:
- ES7+ React/Redux/React-Native snippets
- TypeScript Hero
- Auto Rename Tag
- Prettier - Code formatter
- ESLint

General:
- GitLens
- Error Lens
- TODO Highlight
- REST Client
```

---

## 3. THIẾT LẬP BACKEND (DJANGO)

### 📁 **Tạo Django Project**

#### **Bước 1: Khởi tạo project**
```powershell
# Activate venv
.\venv\Scripts\activate

# Navigate to backend folder
cd D:\NCKH\demo

# Create Django project
django-admin startproject library_backend

# Navigate to project
cd library_backend

# Directory structure:
library_backend/
├── manage.py
└── library_backend/
    ├── __init__.py
    ├── settings.py
    ├── urls.py
    ├── asgi.py
    └── wsgi.py
```

**💡 Giải thích files:**
- `manage.py`: Command-line utility (run server, migrations, etc.)
- `settings.py`: Project configuration
- `urls.py`: URL routing
- `wsgi.py`: Web Server Gateway Interface (for deployment)
- `asgi.py`: Async Server Gateway Interface (for async)

#### **Bước 2: Tạo Django Apps**
```powershell
# Still in library_backend folder

# Create apps
python manage.py startapp users
python manage.py startapp books
python manage.py startapp borrowing
python manage.py startapp notifications
python manage.py startapp system
python manage.py startapp iot

# Structure sau khi tạo:
library_backend/
├── manage.py
├── library_backend/
├── users/
│   ├── models.py
│   ├── views.py
│   ├── serializers.py
│   └── urls.py
├── books/
├── borrowing/
├── notifications/
├── system/
└── iot/
```

**❓ Django App là gì?**
- **Module** chức năng trong project
- **Reusable**: Có thể dùng lại trong projects khác
- **Separation of concerns**: Tách biệt logic theo chức năng

---

### ⚙️ **Cấu hình settings.py**

```python
# library_backend/settings.py

import os
from pathlib import Path
from decouple import config

BASE_DIR = Path(__file__).resolve().parent.parent

# Security
SECRET_KEY = config('SECRET_KEY', default='your-secret-key-here')
DEBUG = config('DEBUG', default=True, cast=bool)
ALLOWED_HOSTS = config('ALLOWED_HOSTS', default='localhost,127.0.0.1').split(',')

# Applications
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    
    # Third-party apps
    'rest_framework',
    'rest_framework_simplejwt',
    'corsheaders',
    'django_celery_beat',
    'drf_yasg',
    
    # Local apps
    'users',
    'books',
    'borrowing',
    'notifications',
    'system',
    'iot',
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'corsheaders.middleware.CorsMiddleware',  # Must be before CommonMiddleware
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

# Database
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': config('DB_NAME', default='library_db'),
        'USER': config('DB_USER', default='library_user'),
        'PASSWORD': config('DB_PASSWORD', default='password'),
        'HOST': config('DB_HOST', default='localhost'),
        'PORT': config('DB_PORT', default='3306'),
        'OPTIONS': {
            'charset': 'utf8mb4',
        }
    }
}

# Django REST Framework
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ],
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 20,
}

# JWT Settings
from datetime import timedelta

SIMPLE_JWT = {
    'ACCESS_TOKEN_LIFETIME': timedelta(hours=1),
    'REFRESH_TOKEN_LIFETIME': timedelta(days=7),
    'ROTATE_REFRESH_TOKENS': True,
    'BLACKLIST_AFTER_ROTATION': True,
}

# CORS Settings
CORS_ALLOWED_ORIGINS = [
    "http://localhost:3000",  # React development server
]
CORS_ALLOW_CREDENTIALS = True

# Celery Settings
CELERY_BROKER_URL = config('CELERY_BROKER_URL', default='redis://localhost:6379/0')
CELERY_RESULT_BACKEND = config('CELERY_RESULT_BACKEND', default='redis://localhost:6379/0')
CELERY_ACCEPT_CONTENT = ['json']
CELERY_TASK_SERIALIZER = 'json'
CELERY_RESULT_SERIALIZER = 'json'
CELERY_TIMEZONE = 'Asia/Ho_Chi_Minh'

# Email Settings
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = config('EMAIL_HOST', default='smtp.gmail.com')
EMAIL_PORT = config('EMAIL_PORT', default=587, cast=int)
EMAIL_USE_TLS = True
EMAIL_HOST_USER = config('EMAIL_HOST_USER', default='')
EMAIL_HOST_PASSWORD = config('EMAIL_HOST_PASSWORD', default='')

# Static & Media Files
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')

MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

# Internationalization
LANGUAGE_CODE = 'vi'
TIME_ZONE = 'Asia/Ho_Chi_Minh'
USE_I18N = True
USE_TZ = True

LANGUAGES = [
    ('vi', 'Vietnamese'),
    ('en', 'English'),
]

LOCALE_PATHS = [
    os.path.join(BASE_DIR, 'locale'),
]
```

**💡 Giải thích config:**

**INSTALLED_APPS**:
- Django default apps + Third-party apps + Local apps
- Thứ tự quan trọng (corsheaders phải trước CommonMiddleware)

**REST_FRAMEWORK**:
- Authentication: JWT
- Permission: Authenticated by default
- Pagination: 20 items per page

**SIMPLE_JWT**:
- Access token: 1 hour
- Refresh token: 7 days
- Rotate on refresh (security)

**CORS**:
- Allow React app (localhost:3000) to access API
- Allow credentials (cookies)

**CELERY**:
- Broker: Redis
- Timezone: Vietnam

---

### 📝 **Tạo .env file**

```bash
# library_backend/.env

# Django
SECRET_KEY=your-very-secret-key-here-change-in-production
DEBUG=True
ALLOWED_HOSTS=localhost,127.0.0.1

# Database
DB_ENGINE=django.db.backends.mysql
DB_NAME=library_db
DB_USER=library_user
DB_PASSWORD=your_database_password
DB_HOST=localhost
DB_PORT=3306

# Redis
REDIS_URL=redis://localhost:6379/0

# Celery
CELERY_BROKER_URL=redis://localhost:6379/0
CELERY_RESULT_BACKEND=redis://localhost:6379/0

# Email
EMAIL_BACKEND=django.core.mail.backends.smtp.EmailBackend
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USE_TLS=True
EMAIL_HOST_USER=your-email@gmail.com
EMAIL_HOST_PASSWORD=your-app-password

# AWS S3 (optional)
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_STORAGE_BUCKET_NAME=
```

**❓ Tại sao dùng .env?**
- **Không commit secrets** lên Git
- **Khác nhau** giữa dev/staging/production
- **Best practice** cho security

---

### 🗄️ **Tạo Models**

#### **Example: users/models.py**
```python
from django.db import models
from django.contrib.auth.models import User

class Role(models.Model):
    """User roles: Admin, Librarian, Reader"""
    ROLE_CHOICES = [
        ('ADMIN', 'Administrator'),
        ('LIBRARIAN', 'Librarian'),
        ('READER', 'Reader'),
    ]
    
    name = models.CharField(max_length=50)
    code = models.CharField(max_length=20, unique=True, choices=ROLE_CHOICES)
    description = models.TextField(null=True, blank=True)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    class Meta:
        db_table = 'users_role'
        verbose_name = 'Role'
        verbose_name_plural = 'Roles'
    
    def __str__(self):
        return self.name


class Profile(models.Model):
    """Extended user profile"""
    READER_TYPE_CHOICES = [
        ('STUDENT', 'Student'),
        ('STAFF', 'Staff'),
    ]
    
    user = models.OneToOneField(User, on_delete=models.CASCADE, related_name='profile')
    role = models.ForeignKey(Role, on_delete=models.SET_NULL, null=True)
    reader_type = models.CharField(max_length=20, choices=READER_TYPE_CHOICES, null=True, blank=True)
    phone = models.CharField(max_length=20, null=True, blank=True)
    avatar = models.ImageField(upload_to='avatars/', null=True, blank=True)
    address = models.TextField(null=True, blank=True)
    student_id = models.CharField(max_length=20, null=True, blank=True)
    faculty = models.CharField(max_length=100, null=True, blank=True)
    date_of_birth = models.DateField(null=True, blank=True)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    class Meta:
        db_table = 'users_profile'
        verbose_name = 'Profile'
        verbose_name_plural = 'Profiles'
    
    def __str__(self):
        return f"{self.user.username} - {self.role}"
```

**💡 Giải thích Model:**

**Fields**:
- `CharField`: Short text (có max_length)
- `TextField`: Long text
- `DateTimeField`: Date & time
- `ForeignKey`: Relationship many-to-one
- `OneToOneField`: Relationship one-to-one
- `ImageField`: Upload images

**Meta class**:
- `db_table`: Tên table trong DB
- `verbose_name`: Tên hiển thị (singular)
- `verbose_name_plural`: Tên hiển thị (plural)

**Methods**:
- `__str__()`: String representation (hiển thị trong admin)

---

### 🔄 **Chạy Migrations**

```powershell
# Create migrations
python manage.py makemigrations

# Output:
# Migrations for 'users':
#   users\migrations\0001_initial.py
#     - Create model Role
#     - Create model Profile

# Apply migrations
python manage.py migrate

# Output:
# Running migrations:
#   Applying users.0001_initial... OK
```

**❓ Migrations là gì?**
- **Database schema changes** dạng code
- **Version control** cho database
- **Reversible**: Có thể rollback

**Commands:**
- `makemigrations`: Tạo migration files từ models
- `migrate`: Apply migrations vào database
- `showmigrations`: Xem status migrations
- `sqlmigrate`: Xem SQL của migration

---

### 🔐 **Tạo Serializers**

```python
# users/serializers.py

from rest_framework import serializers
from django.contrib.auth.models import User
from .models import Profile, Role

class RoleSerializer(serializers.ModelSerializer):
    class Meta:
        model = Role
        fields = ['id', 'name', 'code', 'description']


class ProfileSerializer(serializers.ModelSerializer):
    role = RoleSerializer(read_only=True)
    role_id = serializers.IntegerField(write_only=True)
    
    class Meta:
        model = Profile
        fields = [
            'id', 'role', 'role_id', 'reader_type', 'phone',
            'avatar', 'address', 'student_id', 'faculty',
            'date_of_birth', 'created_at', 'updated_at'
        ]


class UserSerializer(serializers.ModelSerializer):
    profile = ProfileSerializer(read_only=True)
    password = serializers.CharField(write_only=True, required=False)
    
    class Meta:
        model = User
        fields = [
            'id', 'username', 'email', 'first_name', 'last_name',
            'password', 'is_active', 'date_joined', 'profile'
        ]
        extra_kwargs = {
            'password': {'write_only': True}
        }
    
    def create(self, validated_data):
        # Create user with hashed password
        password = validated_data.pop('password', None)
        user = User.objects.create(**validated_data)
        if password:
            user.set_password(password)
            user.save()
        return user
    
    def update(self, instance, validated_data):
        # Update user, hash password if changed
        password = validated_data.pop('password', None)
        for attr, value in validated_data.items():
            setattr(instance, attr, value)
        if password:
            instance.set_password(password)
        instance.save()
        return instance
```

**❓ Serializer làm gì?**
- **Python objects → JSON** (serialization)
- **JSON → Python objects** (deserialization)
- **Validation**: Validate input data
- **Nested serializers**: Include related objects

**Fields options:**
- `read_only=True`: Chỉ đọc (không cần khi create/update)
- `write_only=True`: Chỉ ghi (không trả về trong response)
- `required=False`: Optional field

---

### 🎯 **Tạo Views (ViewSets)**

```python
# users/views.py

from rest_framework import viewsets, status
from rest_framework.decorators import action
from rest_framework.response import Response
from rest_framework.permissions import IsAuthenticated, AllowAny
from django.contrib.auth.models import User
from .models import Profile, Role
from .serializers import UserSerializer, ProfileSerializer, RoleSerializer

class UserViewSet(viewsets.ModelViewSet):
    """
    ViewSet for User CRUD operations
    """
    queryset = User.objects.all()
    serializer_class = UserSerializer
    permission_classes = [IsAuthenticated]
    
    def get_permissions(self):
        # Allow anyone to create account (registration)
        if self.action == 'create':
            return [AllowAny()]
        return super().get_permissions()
    
    def get_queryset(self):
        # Filter based on user role
        queryset = super().get_queryset()
        user = self.request.user
        
        # Admin sees all users
        if user.is_superuser:
            return queryset
        
        # Librarian sees non-admin users
        if hasattr(user, 'profile') and user.profile.role.code == 'LIBRARIAN':
            return queryset.exclude(is_superuser=True)
        
        # Reader sees only themselves
        return queryset.filter(id=user.id)
    
    @action(detail=False, methods=['get'])
    def me(self, request):
        """Get current user profile"""
        serializer = self.get_serializer(request.user)
        return Response(serializer.data)
    
    @action(detail=True, methods=['post'])
    def change_password(self, request, pk=None):
        """Change user password"""
        user = self.get_object()
        old_password = request.data.get('old_password')
        new_password = request.data.get('new_password')
        
        # Verify old password
        if not user.check_password(old_password):
            return Response(
                {'error': 'Old password is incorrect'},
                status=status.HTTP_400_BAD_REQUEST
            )
        
        # Set new password
        user.set_password(new_password)
        user.save()
        
        return Response({'message': 'Password changed successfully'})


class RoleViewSet(viewsets.ReadOnlyModelViewSet):
    """
    ViewSet for Role (read-only)
    """
    queryset = Role.objects.all()
    serializer_class = RoleSerializer
    permission_classes = [IsAuthenticated]
```

**💡 Giải thích ViewSet:**

**ModelViewSet**:
- Auto CRUD operations:
  - `list()` → GET /users/
  - `create()` → POST /users/
  - `retrieve()` → GET /users/{id}/
  - `update()` → PUT /users/{id}/
  - `partial_update()` → PATCH /users/{id}/
  - `destroy()` → DELETE /users/{id}/

**Custom actions** (`@action`):
- `detail=False`: List endpoint (`/users/me/`)
- `detail=True`: Detail endpoint (`/users/{id}/change_password/`)
- `methods`: HTTP methods allowed

**Permissions**:
- `IsAuthenticated`: Phải login
- `AllowAny`: Không cần login
- Custom permissions có thể tạo

---

### 🛤️ **Cấu hình URLs**

```python
# users/urls.py

from rest_framework.routers import DefaultRouter
from .views import UserViewSet, RoleViewSet

router = DefaultRouter()
router.register('users', UserViewSet, basename='user')
router.register('roles', RoleViewSet, basename='role')

urlpatterns = router.urls
```

```python
# library_backend/urls.py

from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static
from rest_framework_simplejwt.views import TokenObtainPairView, TokenRefreshView
from drf_yasg.views import get_schema_view
from drf_yasg import openapi

# Swagger documentation
schema_view = get_schema_view(
    openapi.Info(
        title="Library Management API",
        default_version='v1',
        description="API documentation for Library Management System",
    ),
    public=True,
)

urlpatterns = [
    path('admin/', admin.site.urls),
    
    # JWT Authentication
    path('api/auth/login/', TokenObtainPairView.as_view(), name='token_obtain_pair'),
    path('api/auth/refresh/', TokenRefreshView.as_view(), name='token_refresh'),
    
    # API endpoints
    path('api/', include('users.urls')),
    path('api/', include('books.urls')),
    path('api/', include('borrowing.urls')),
    path('api/', include('notifications.urls')),
    path('api/', include('system.urls')),
    path('api/', include('iot.urls')),
    
    # API Documentation
    path('swagger/', schema_view.with_ui('swagger', cache_timeout=0), name='schema-swagger-ui'),
    path('redoc/', schema_view.with_ui('redoc', cache_timeout=0), name='schema-redoc'),
]

# Serve media files in development
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

**💡 Router tự động tạo URLs:**
```
GET    /api/users/           → list
POST   /api/users/           → create
GET    /api/users/{id}/      → retrieve
PUT    /api/users/{id}/      → update
PATCH  /api/users/{id}/      → partial_update
DELETE /api/users/{id}/      → destroy
GET    /api/users/me/        → custom action
POST   /api/users/{id}/change_password/ → custom action
```

---

### 🚀 **Chạy Development Server**

```powershell
# Activate venv
.\venv\Scripts\activate

# Navigate to project
cd library_backend

# Run server
python manage.py runserver

# Output:
# Starting development server at http://127.0.0.1:8000/
# Quit the server with CTRL-BREAK.
```

**Test API:**
- Admin: http://127.0.0.1:8000/admin/
- Swagger: http://127.0.0.1:8000/swagger/
- API: http://127.0.0.1:8000/api/users/

---

## 4. THIẾT LẬP FRONTEND (REACT)

### 📁 **Cấu trúc Project**

```
library-frontend/
├── public/
│   ├── index.html
│   └── favicon.ico
├── src/
│   ├── index.tsx                 # Entry point
│   ├── App.tsx                   # Main component
│   ├── api/                      # API services
│   │   ├── axios.ts              # Axios config
│   │   ├── authService.ts        # Auth APIs
│   │   ├── bookService.ts        # Book APIs
│   │   └── borrowService.ts      # Borrow APIs
│   ├── components/               # Reusable components
│   │   ├── common/
│   │   │   ├── Navbar.tsx
│   │   │   ├── Sidebar.tsx
│   │   │   ├── Loader.tsx
│   │   │   └── Modal.tsx
│   │   ├── books/
│   │   │   ├── BookCard.tsx
│   │   │   ├── BookList.tsx
│   │   │   └── BookForm.tsx
│   │   └── borrowing/
│   │       ├── BorrowList.tsx
│   │       └── ReturnForm.tsx
│   ├── pages/                    # Page components
│   │   ├── auth/
│   │   │   ├── LoginPage.tsx
│   │   │   └── ForgotPasswordPage.tsx
│   │   ├── admin/
│   │   │   ├── DashboardPage.tsx
│   │   │   ├── UserManagementPage.tsx
│   │   │   └── ReportsPage.tsx
│   │   ├── librarian/
│   │   │   ├── BorrowPage.tsx
│   │   │   └── ReturnPage.tsx
│   │   └── reader/
│   │       ├── CatalogPage.tsx
│   │       └── MyBorrowingsPage.tsx
│   ├── layouts/                  # Layout components
│   │   ├── PublicLayout.tsx
│   │   ├── AdminLayout.tsx
│   │   └── ReaderLayout.tsx
│   ├── hooks/                    # Custom hooks
│   │   ├── useAuth.ts
│   │   └── useDebounce.ts
│   ├── store/                    # Redux store
│   │   ├── store.ts
│   │   ├── authSlice.ts
│   │   ├── bookSlice.ts
│   │   └── appSlice.ts
│   ├── types/                    # TypeScript types
│   │   ├── user.ts
│   │   ├── book.ts
│   │   └── borrow.ts
│   ├── utils/                    # Utility functions
│   │   ├── formatters.ts
│   │   └── validators.ts
│   ├── constants/                # Constants
│   │   └── index.ts
│   ├── i18n/                     # Translations
│   │   ├── en/
│   │   │   └── common.json
│   │   └── vi/
│   │       └── common.json
│   └── styles/                   # Global styles
│       └── main.scss
├── package.json
├── tsconfig.json
└── .env
```

---

### ⚙️ **Cấu hình TypeScript (tsconfig.json)**

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "jsx": "react-jsx",
    "module": "ESNext",
    "moduleResolution": "Node",
    "resolveJsonModule": true,
    "allowJs": true,
    "noEmit": true,
    "isolatedModules": true,
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "skipLibCheck": true,
    "baseUrl": "src",
    "paths": {
      "@/*": ["./*"],
      "@components/*": ["components/*"],
      "@pages/*": ["pages/*"],
      "@api/*": ["api/*"],
      "@types/*": ["types/*"],
      "@utils/*": ["utils/*"]
    }
  },
  "include": ["src"],
  "exclude": ["node_modules"]
}
```

**💡 Path aliases:**
- `@/` → `src/`
- `@components/` → `src/components/`
- Import: `import Button from '@components/common/Button'`

---

### 🔌 **Setup Axios**

```typescript
// src/api/axios.ts

import axios, { AxiosError, InternalAxiosRequestConfig } from 'axios';

const API_URL = process.env.REACT_APP_API_URL || 'http://localhost:8000/api';

// Create axios instance
const axiosInstance = axios.create({
  baseURL: API_URL,
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Request interceptor - Add JWT token
axiosInstance.interceptors.request.use(
  (config: InternalAxiosRequestConfig) => {
    const token = localStorage.getItem('access_token');
    if (token && config.headers) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error: AxiosError) => {
    return Promise.reject(error);
  }
);

// Response interceptor - Handle errors & token refresh
axiosInstance.interceptors.response.use(
  (response) => response,
  async (error: AxiosError) => {
    const originalRequest = error.config as InternalAxiosRequestConfig & { _retry?: boolean };

    // If 401 and not already retried, try refresh token
    if (error.response?.status === 401 && !originalRequest._retry) {
      originalRequest._retry = true;

      try {
        const refreshToken = localStorage.getItem('refresh_token');
        if (refreshToken) {
          const response = await axios.post(`${API_URL}/auth/refresh/`, {
            refresh: refreshToken,
          });

          const { access } = response.data;
          localStorage.setItem('access_token', access);

          // Retry original request with new token
          if (originalRequest.headers) {
            originalRequest.headers.Authorization = `Bearer ${access}`;
          }
          return axiosInstance(originalRequest);
        }
      } catch (refreshError) {
        // Refresh failed, logout user
        localStorage.removeItem('access_token');
        localStorage.removeItem('refresh_token');
        window.location.href = '/login';
        return Promise.reject(refreshError);
      }
    }

    return Promise.reject(error);
  }
);

export default axiosInstance;
```

**💡 Giải thích Interceptors:**

**Request Interceptor**:
- Tự động thêm JWT token vào header mỗi request
- Không cần manually thêm token mỗi lần

**Response Interceptor**:
- Xử lý lỗi 401 (Unauthorized)
- Tự động refresh token
- Retry request với token mới
- Logout nếu refresh thất bại

---

### 🔐 **Authentication Service**

```typescript
// src/api/authService.ts

import axios from './axios';

export interface LoginCredentials {
  username: string;
  password: string;
}

export interface LoginResponse {
  access: string;
  refresh: string;
  user: {
    id: number;
    username: string;
    email: string;
    profile: {
      role: {
        code: string;
        name: string;
      };
    };
  };
}

export interface RegisterData {
  username: string;
  email: string;
  password: string;
  first_name: string;
  last_name: string;
}

class AuthService {
  async login(credentials: LoginCredentials): Promise<LoginResponse> {
    const response = await axios.post('/auth/login/', credentials);
    
    // Save tokens
    localStorage.setItem('access_token', response.data.access);
    localStorage.setItem('refresh_token', response.data.refresh);
    
    // Get user info
    const userResponse = await axios.get('/users/me/');
    
    return {
      ...response.data,
      user: userResponse.data,
    };
  }

  async logout(): Promise<void> {
    localStorage.removeItem('access_token');
    localStorage.removeItem('refresh_token');
  }

  async register(data: RegisterData): Promise<any> {
    const response = await axios.post('/users/', data);
    return response.data;
  }

  async getCurrentUser(): Promise<any> {
    const response = await axios.get('/users/me/');
    return response.data;
  }

  async changePassword(oldPassword: string, newPassword: string): Promise<void> {
    await axios.post('/users/me/change_password/', {
      old_password: oldPassword,
      new_password: newPassword,
    });
  }

  isAuthenticated(): boolean {
    return !!localStorage.getItem('access_token');
  }

  getAccessToken(): string | null {
    return localStorage.getItem('access_token');
  }
}

export default new AuthService();
```

**💡 Service pattern:**
- **Encapsulate API calls** trong service class
- **Reusable** trong các components
- **Easy to test** và maintain

---

### 🗂️ **Redux Store Setup**

```typescript
// src/store/store.ts

import { configureStore } from '@reduxjs/toolkit';
import authReducer from './authSlice';
import appReducer from './appSlice';

export const store = configureStore({
  reducer: {
    auth: authReducer,
    app: appReducer,
  },
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

```typescript
// src/store/authSlice.ts

import { createSlice, createAsyncThunk, PayloadAction } from '@reduxjs/toolkit';
import authService, { LoginCredentials } from '@/api/authService';

interface User {
  id: number;
  username: string;
  email: string;
  role: string;
}

interface AuthState {
  user: User | null;
  isAuthenticated: boolean;
  loading: boolean;
  error: string | null;
}

const initialState: AuthState = {
  user: null,
  isAuthenticated: authService.isAuthenticated(),
  loading: false,
  error: null,
};

// Async thunk for login
export const login = createAsyncThunk(
  'auth/login',
  async (credentials: LoginCredentials, { rejectWithValue }) => {
    try {
      const response = await authService.login(credentials);
      return response.user;
    } catch (error: any) {
      return rejectWithValue(error.response?.data?.message || 'Login failed');
    }
  }
);

// Async thunk for logout
export const logout = createAsyncThunk('auth/logout', async () => {
  await authService.logout();
});

// Async thunk for get current user
export const getCurrentUser = createAsyncThunk('auth/getCurrentUser', async () => {
  const user = await authService.getCurrentUser();
  return user;
});

const authSlice = createSlice({
  name: 'auth',
  initialState,
  reducers: {
    clearError: (state) => {
      state.error = null;
    },
  },
  extraReducers: (builder) => {
    builder
      // Login
      .addCase(login.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(login.fulfilled, (state, action: PayloadAction<User>) => {
        state.loading = false;
        state.isAuthenticated = true;
        state.user = action.payload;
      })
      .addCase(login.rejected, (state, action) => {
        state.loading = false;
        state.error = action.payload as string;
      })
      // Logout
      .addCase(logout.fulfilled, (state) => {
        state.user = null;
        state.isAuthenticated = false;
      })
      // Get current user
      .addCase(getCurrentUser.fulfilled, (state, action: PayloadAction<User>) => {
        state.user = action.payload;
        state.isAuthenticated = true;
      });
  },
});

export const { clearError } = authSlice.actions;
export default authSlice.reducer;
```

**💡 Redux Toolkit features:**
- **createSlice**: Tạo reducer + actions tự động
- **createAsyncThunk**: Handle async operations
- **No boilerplate**: Less code than classic Redux
- **Immer**: Mutate state directly (internally uses Immer)

---

### 🔌 **Connect Redux to React**

```typescript
// src/index.tsx

import React from 'react';
import ReactDOM from 'react-dom/client';
import { Provider } from 'react-redux';
import { store } from './store/store';
import App from './App';
import './styles/main.scss';
import 'bootstrap/dist/css/bootstrap.min.css';

const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);

root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);
```

**💡 Provider:**
- Wrap app với `<Provider store={store}>`
- Tất cả components có thể access Redux store

---

### 🛤️ **React Router Setup**

```typescript
// src/App.tsx

import React from 'react';
import { BrowserRouter, Routes, Route, Navigate } from 'react-router-dom';
import { useSelector } from 'react-redux';
import { RootState } from './store/store';

// Layouts
import PublicLayout from './layouts/PublicLayout';
import AdminLayout from './layouts/AdminLayout';
import ReaderLayout from './layouts/ReaderLayout';

// Pages
import LoginPage from './pages/auth/LoginPage';
import DashboardPage from './pages/admin/DashboardPage';
import CatalogPage from './pages/reader/CatalogPage';
import NotFoundPage from './pages/NotFoundPage';

// Protected Route Component
const ProtectedRoute: React.FC<{ children: React.ReactElement; allowedRoles?: string[] }> = ({
  children,
  allowedRoles,
}) => {
  const { isAuthenticated, user } = useSelector((state: RootState) => state.auth);

  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }

  if (allowedRoles && user && !allowedRoles.includes(user.role)) {
    return <Navigate to="/unauthorized" replace />;
  }

  return children;
};

const App: React.FC = () => {
  return (
    <BrowserRouter>
      <Routes>
        {/* Public Routes */}
        <Route element={<PublicLayout />}>
          <Route path="/login" element={<LoginPage />} />
          <Route path="/forgot-password" element={<div>Forgot Password</div>} />
        </Route>

        {/* Admin Routes */}
        <Route
          element={
            <ProtectedRoute allowedRoles={['ADMIN', 'LIBRARIAN']}>
              <AdminLayout />
            </ProtectedRoute>
          }
        >
          <Route path="/admin/dashboard" element={<DashboardPage />} />
          <Route path="/admin/users" element={<div>Users</div>} />
          <Route path="/admin/books" element={<div>Books</div>} />
        </Route>

        {/* Reader Routes */}
        <Route
          element={
            <ProtectedRoute allowedRoles={['READER']}>
              <ReaderLayout />
            </ProtectedRoute>
          }
        >
          <Route path="/reader/catalog" element={<CatalogPage />} />
          <Route path="/reader/my-borrowings" element={<div>My Borrowings</div>} />
        </Route>

        {/* Default & 404 */}
        <Route path="/" element={<Navigate to="/login" replace />} />
        <Route path="*" element={<NotFoundPage />} />
      </Routes>
    </BrowserRouter>
  );
};

export default App;
```

**💡 React Router concepts:**
- `<BrowserRouter>`: Wrap toàn bộ app
- `<Routes>`: Container cho routes
- `<Route>`: Define path và component
- `<Navigate>`: Redirect programmatically
- **Nested routes**: Routes trong routes (layouts)

---

(Tiếp tục trong phần 2...)

**File này đã quá dài. Tôi sẽ tạo thêm file part 2 để tiếp tục...**
