# TECH STACK & ARCHITECTURE

## 🏗️ KIẾN TRÚC HỆ THỐNG

### System Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                         CLIENT LAYER                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐        │
│  │   Browser    │  │    Mobile    │  │   Tablet     │        │
│  │  (Desktop)   │  │   Browser    │  │   Browser    │        │
│  └──────────────┘  └──────────────┘  └──────────────┘        │
│                          │                                      │
│                          │ HTTPS                                │
│                          ▼                                      │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                      CDN LAYER (Cloudflare)                     │
│  - Static Assets Caching                                        │
│  - DDoS Protection                                              │
│  - SSL/TLS                                                      │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                        WEB SERVER (Nginx)                       │
│  - Reverse Proxy                                                │
│  - Load Balancing                                               │
│  - Static File Serving                                          │
│  - SSL Termination                                              │
└─────────────────────────────────────────────────────────────────┘
                              │
                ┌─────────────┴────────────┐
                ▼                          ▼
┌──────────────────────────┐   ┌──────────────────────────┐
│   REACT FRONTEND         │   │   DJANGO BACKEND         │
│   (Single Page App)      │   │   (REST API)             │
│                          │   │                          │
│  - React 18              │   │  - Django 5.0            │
│  - TypeScript            │   │  - DRF                   │
│  - Redux Toolkit         │   │  - JWT Auth              │
│  - React Router          │   │  - Celery                │
│  - Bootstrap 5           │   │  - MySQL                 │
│  - Axios                 │   │  - Redis                 │
│  - i18next               │   │                          │
│  - html5-qrcode          │   │                          │
└──────────────────────────┘   └──────────────────────────┘
                                           │
                        ┌──────────────────┼──────────────────┐
                        ▼                  ▼                  ▼
              ┌─────────────┐    ┌──────────────┐  ┌──────────────┐
              │   MySQL     │    │    Redis     │  │   AWS S3     │
              │  Database   │    │   (Cache +   │  │ (Media Files)│
              │             │    │    Queue)    │  │              │
              └─────────────┘    └──────────────┘  └──────────────┘
                                           │
                                           ▼
                                 ┌──────────────────┐
                                 │  Celery Workers  │
                                 │  - Email tasks   │
                                 │  - Scheduled     │
                                 │  - Background    │
                                 └──────────────────┘
```

---

## 💻 TECH STACK DETAILS

### Backend Stack

#### Core Framework
```yaml
Django: 5.0
  - Python web framework
  - ORM for database
  - Admin interface
  - Authentication system
  - Template engine (for emails)
  
Django REST Framework: 3.14+
  - RESTful API
  - Serialization
  - ViewSets and Routers
  - Permissions
  - Throttling
```

#### Authentication & Security
```yaml
djangorestframework-simplejwt: 5.3+
  - JWT token generation
  - Token refresh
  - Token blacklist
  
django-cors-headers: 4.3+
  - CORS handling
  - Cross-origin requests
  
Additional:
  - Django's built-in CSRF protection
  - Password hashing (PBKDF2)
  - XSS prevention
  - SQL injection prevention
```

#### Database
```yaml
MySQL: 8.0+
  - Production database
  - ACID compliance
  - Indexing
  - Full-text search
  
SQLite: 3.x
  - Development database
  - Testing database
  
mysqlclient: 2.2+
  - MySQL Python connector
```

#### Task Queue
```yaml
Celery: 5.3+
  - Asynchronous tasks
  - Scheduled tasks (Celery Beat)
  - Task queue management
  - Retry logic
  
Redis: 7.0+
  - Celery broker
  - Caching
  - Session storage
  
django-celery-beat: 2.5+
  - Database-backed periodic tasks
  - Django admin integration
```

#### File Handling
```yaml
Pillow: 10.1+
  - Image processing
  - Image validation
  - Thumbnail generation
  
python-qrcode: 7.4+
  - QR code generation
  - Customizable QR codes
  
openpyxl: 3.1+
  - Excel file reading
  - Excel file writing
  - Styling
```

#### Reports
```yaml
reportlab: 4.0+
  - PDF generation
  - Custom layouts
  - Charts in PDF
  
OR

WeasyPrint: 60+
  - HTML to PDF
  - CSS styling
  - Better formatting
```

#### Email
```yaml
Django Email Backend:
  - SMTP backend
  - Console backend (dev)
  - File backend (testing)
  
Optional Email Services:
  - SendGrid (recommended)
  - Mailgun
  - Amazon SES
  - Gmail SMTP
```

#### Additional Libraries
```yaml
python-decouple: 3.8
  - Environment variables
  - Configuration management
  
django-filter: 23.5
  - Advanced filtering
  - Query parameter parsing
  
drf-yasg: 1.21+ (or drf-spectacular)
  - Swagger/OpenAPI documentation
  - API playground
```

---

### Frontend Stack

#### Core Framework
```yaml
React: 18.2+
  - Component-based UI
  - Hooks
  - Virtual DOM
  - JSX
  
TypeScript: 5.0+
  - Type safety
  - Better IDE support
  - Compile-time error checking
```

#### State Management
```yaml
Redux Toolkit: 2.0+
  - Global state management
  - Redux DevTools
  - RTK Query (optional)
  
OR

React Query (TanStack Query): 5.0+
  - Server state management
  - Caching
  - Automatic refetching
  - Simpler than Redux for API calls
```

#### Routing
```yaml
React Router: 6.20+
  - Client-side routing
  - Nested routes
  - Protected routes
  - Route parameters
```

#### UI Framework
```yaml
Bootstrap: 5.3+
  - CSS framework
  - Responsive grid
  - Components
  
React-Bootstrap: 2.9+
  - React components
  - Bootstrap integration
  - Customizable
```

#### HTTP Client
```yaml
Axios: 1.6+
  - HTTP requests
  - Interceptors
  - Request/Response transformation
  - Automatic JSON parsing
```

#### Internationalization
```yaml
react-i18next: 13.5+
  - Multi-language support
  - Language switching
  - Namespaces
  
i18next: 23.7+
  - Core i18n library
  - Translation management
```

#### QR Code
```yaml
html5-qrcode: 2.3+
  - QR code scanning
  - Camera access
  - Multiple formats
  
OR

react-qr-reader: 3.0+
  - React wrapper
  - Easier integration
```

#### Charts
```yaml
Chart.js: 4.4+
  - Various chart types
  - Responsive
  - Animations
  
react-chartjs-2: 5.2+
  - React wrapper
  - Component-based
```

#### Forms
```yaml
react-hook-form: 7.49+
  - Form validation
  - Performance optimization
  - Less re-renders
  - Easy integration
```

#### Date Handling
```yaml
date-fns: 3.0+
  - Date formatting
  - Date manipulation
  - Lightweight
  - Tree-shakeable
```

#### Notifications
```yaml
react-toastify: 9.1+
  - Toast notifications
  - Customizable
  - Easy to use
```

#### Additional Libraries
```yaml
@fortawesome/fontawesome-free: 6.5+
  - Icons
  - Various styles
  
sass: 1.69+
  - CSS preprocessor
  - Variables, mixins
  - Better organization
```

---

### DevOps & Infrastructure

#### Web Server
```yaml
Nginx: 1.24+
  - Reverse proxy
  - Static file serving
  - Load balancing
  - SSL termination
  - Gzip compression
```

#### Application Server
```yaml
Gunicorn: 21.2+
  - WSGI HTTP Server
  - Process management
  - Worker processes
  - Production-ready
```

#### Process Manager
```yaml
Supervisor: 4.2+
  - Process control
  - Celery worker management
  - Celery beat management
  - Auto-restart
```

#### Containerization (Optional)
```yaml
Docker: 24.0+
  - Container platform
  - Consistent environments
  
Docker Compose: 2.23+
  - Multi-container apps
  - Development environment
```

#### Version Control
```yaml
Git: 2.40+
  - Version control
  - Branching
  - Collaboration
  
GitHub/GitLab:
  - Repository hosting
  - CI/CD (optional)
  - Issue tracking
  - Project management
```

#### Monitoring & Logging
```yaml
Sentry: Latest
  - Error tracking
  - Performance monitoring
  - Real-time alerts
  
UptimeRobot: Free plan
  - Uptime monitoring
  - Email alerts
  - Status page
```

#### SSL/TLS
```yaml
Let's Encrypt:
  - Free SSL certificates
  - Auto-renewal
  
Certbot: Latest
  - Certificate management
  - Nginx integration
```

#### CDN & DNS
```yaml
Cloudflare: Free plan
  - CDN
  - DNS management
  - DDoS protection
  - SSL/TLS
  - Caching
```

---

## 🗄️ DATABASE DESIGN

### Database Schema Summary

**Total Tables**: 15

#### User Management (3 tables)
1. `auth_user` - Django default user table
2. `users_role` - User roles
3. `users_profile` - Extended user profile

#### Books Management (4 tables)
4. `books_category` - Book categories (tree structure)
5. `books_author` - Authors
6. `books_publisher` - Publishers
7. `books_book` - Books

#### Borrowing System (3 tables)
8. `borrowing_record` - Borrow records
9. `borrowing_detail` - Borrow details (books in record)
10. `borrowing_violation` - Violations

#### Notifications (1 table)
11. `notifications_notification` - System notifications

#### System (2 tables)
12. `system_config` - System configuration
13. `activity_log` - Activity logging

#### IoT (2 tables)
14. `iot_device` - IoT devices
15. `iot_command` - IoT commands

### Key Relationships

```
User 1:1 Profile
User 1:N BorrowRecord (as reader)
User 1:N BorrowRecord (as librarian)
User 1:N Violation
User 1:N Notification
User 1:N ActivityLog

Book N:1 Category
Book N:1 Author
Book N:1 Publisher
Book 1:N BorrowDetail

BorrowRecord 1:N BorrowDetail
BorrowDetail 1:N Violation

Category 1:N Category (self-referencing)

IoTDevice 1:N IoTCommand
```

---

## 🔐 SECURITY FEATURES

### Authentication
- JWT-based authentication
- Token expiration
- Token refresh mechanism
- Token blacklist on logout
- Password hashing (PBKDF2)
- Password strength validation

### Authorization
- Role-based access control (RBAC)
- Permission-based actions
- Object-level permissions
- API endpoint protection

### Data Protection
- SQL injection prevention (ORM)
- XSS prevention (React auto-escaping)
- CSRF protection (Django middleware)
- Input validation
- Output encoding
- Secure file upload

### Communication
- HTTPS enforcement
- SSL/TLS certificates
- Secure cookies
- CORS configuration
- HTTP security headers

### Application Security
- Rate limiting
- Request throttling
- Activity logging
- Error handling (no sensitive data leak)
- Secure session management

---

## 📊 PERFORMANCE OPTIMIZATION

### Backend
- Database indexing
- Query optimization (select_related, prefetch_related)
- Redis caching
- Connection pooling
- Lazy loading
- Pagination

### Frontend
- Code splitting
- Lazy loading components
- Image optimization
- Bundle size optimization
- Tree shaking
- Memoization (useMemo, useCallback)

### Infrastructure
- CDN for static assets
- Gzip compression
- Browser caching
- HTTP/2
- Nginx caching

---

## 📱 RESPONSIVE DESIGN

### Breakpoints
```scss
// Bootstrap 5 breakpoints
$xs: 0;      // Extra small devices (portrait phones)
$sm: 576px;  // Small devices (landscape phones)
$md: 768px;  // Medium devices (tablets)
$lg: 992px;  // Large devices (desktops)
$xl: 1200px; // Extra large devices (large desktops)
$xxl: 1400px; // Extra extra large devices
```

### Mobile-First Approach
- Design for mobile first
- Progressive enhancement
- Touch-friendly UI
- Responsive images
- Flexible grids

---

## 🌐 INTERNATIONALIZATION

### Supported Languages
- Vietnamese (vi)
- English (en)

### Implementation
- Backend: Django i18n
- Frontend: react-i18next
- Translation files (JSON)
- Language switcher
- Date/Time formatting
- Number formatting
- Currency formatting (if needed)

---

## 🔄 API DESIGN

### RESTful Principles
- Resource-based URLs
- HTTP methods (GET, POST, PUT, DELETE)
- Status codes (200, 201, 400, 401, 404, 500)
- JSON format
- Versioning (optional: `/api/v1/`)

### Pagination
```json
{
  "count": 100,
  "next": "http://api.example.com/books/?page=2",
  "previous": null,
  "results": [...]
}
```

### Error Response
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": {
      "title": ["This field is required."]
    }
  }
}
```

### API Documentation
- Swagger UI
- OpenAPI 3.0 specification
- Request/Response examples
- Authentication guide
- Error codes reference

---

## 📦 PROJECT STRUCTURE

### Backend (Django)
```
library_backend/
├── manage.py
├── requirements.txt
├── .env
├── .gitignore
├── library/                  # Project settings
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── wsgi.py
│   └── celery.py
├── users/                    # User app
│   ├── models.py
│   ├── serializers.py
│   ├── views.py
│   ├── urls.py
│   └── permissions.py
├── books/                    # Books app
│   ├── models.py
│   ├── serializers.py
│   ├── views.py
│   └── urls.py
├── borrowing/                # Borrowing app
│   ├── models.py
│   ├── serializers.py
│   ├── views.py
│   └── urls.py
├── notifications/            # Notifications app
├── system/                   # System app
├── iot/                      # IoT app
├── media/                    # Uploaded files
├── static/                   # Static files
└── templates/                # Email templates
```

### Frontend (React)
```
library-frontend/
├── package.json
├── tsconfig.json
├── .env
├── .gitignore
├── public/
│   ├── index.html
│   └── favicon.ico
└── src/
    ├── index.tsx
    ├── App.tsx
    ├── api/                  # API services
    │   ├── axios.ts
    │   ├── authService.ts
    │   ├── bookService.ts
    │   └── borrowService.ts
    ├── components/           # Reusable components
    │   ├── common/
    │   ├── books/
    │   └── borrowing/
    ├── pages/                # Page components
    │   ├── auth/
    │   ├── admin/
    │   ├── librarian/
    │   └── reader/
    ├── layouts/              # Layout components
    │   ├── PublicLayout.tsx
    │   ├── AdminLayout.tsx
    │   └── ReaderLayout.tsx
    ├── store/                # Redux store
    │   ├── store.ts
    │   ├── authSlice.ts
    │   └── appSlice.ts
    ├── hooks/                # Custom hooks
    ├── types/                # TypeScript types
    ├── utils/                # Utilities
    ├── constants/            # Constants
    ├── i18n/                 # Translations
    │   ├── en/
    │   └── vi/
    └── styles/               # Global styles
```

---

## 🚀 DEPLOYMENT ARCHITECTURE

### Production Setup

```
Internet
    │
    ▼
[Cloudflare CDN]
    │
    ▼
[Domain] → [DNS]
    │
    ▼
[VPS Server - Ubuntu 22.04]
    │
    ├─ [Nginx :80, :443]
    │   ├─ Reverse proxy to Gunicorn
    │   └─ Serve React build files
    │
    ├─ [Gunicorn :8000]
    │   └─ Django application
    │
    ├─ [MySQL :3306]
    │   └─ Database
    │
    ├─ [Redis :6379]
    │   ├─ Celery broker
    │   └─ Cache
    │
    ├─ [Celery Workers]
    │   └─ Background tasks
    │
    ├─ [Celery Beat]
    │   └─ Scheduled tasks
    │
    └─ [Supervisor]
        ├─ Manage Gunicorn
        ├─ Manage Celery workers
        └─ Manage Celery beat
```

### Server Requirements

**Minimum**:
- 2GB RAM
- 1 CPU Core
- 25GB SSD
- Ubuntu 22.04 LTS

**Recommended**:
- 4GB RAM
- 2 CPU Cores
- 50GB SSD
- Ubuntu 22.04 LTS

---

## 📋 ENVIRONMENT VARIABLES

### Backend (.env)
```bash
# Django
SECRET_KEY=your-secret-key
DEBUG=False
ALLOWED_HOSTS=yourdomain.com,www.yourdomain.com

# Database
DB_ENGINE=django.db.backends.mysql
DB_NAME=library_db
DB_USER=library_user
DB_PASSWORD=strong-password
DB_HOST=localhost
DB_PORT=3306

# Redis
REDIS_URL=redis://localhost:6379/0

# Email
EMAIL_BACKEND=django.core.mail.backends.smtp.EmailBackend
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USE_TLS=True
EMAIL_HOST_USER=your-email@gmail.com
EMAIL_HOST_PASSWORD=your-app-password

# Celery
CELERY_BROKER_URL=redis://localhost:6379/0
CELERY_RESULT_BACKEND=redis://localhost:6379/0

# AWS S3 (optional)
AWS_ACCESS_KEY_ID=your-key
AWS_SECRET_ACCESS_KEY=your-secret
AWS_STORAGE_BUCKET_NAME=your-bucket

# Sentry (optional)
SENTRY_DSN=your-sentry-dsn
```

### Frontend (.env)
```bash
REACT_APP_API_URL=https://api.yourdomain.com
REACT_APP_SENTRY_DSN=your-sentry-dsn
REACT_APP_VERSION=1.0.0
```

---

**Version**: 1.0  
**Last Updated**: November 2, 2025  
**Status**: Ready for Implementation
