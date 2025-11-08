# 👨‍💻 DEVELOPER 2 - FRONTEND LEAD

**Role:** Frontend Lead Developer  
**Focus:** React, TypeScript, UI/UX, Component Architecture  
**Duration:** 3 months (12 weeks)  
**Responsibility:** 40% Frontend features + UI/UX design

---

## 📋 TỔNG QUAN NHIỆM VỤ

### **Vai trò chính:**
- ✅ Thiết lập React + TypeScript project
- ✅ Thiết kế UI/UX cho toàn bộ system
- ✅ Xây dựng reusable components
- ✅ Implement responsive layouts
- ✅ Tích hợp với Backend APIs
- ✅ State management với Redux
- ✅ Review code của Dev 3 cho frontend tasks

### **Tech Stack:**
- React 18
- TypeScript
- Redux Toolkit
- React Router v6
- Bootstrap 5
- Axios
- react-i18next
- html5-qrcode
- Chart.js

---

## 📝 CHI TIẾT CÔNG VIỆC

### **A. PAGES & ROUTES (25+ Pages)**

#### **1. Public Pages (3 pages)**
```tsx
/login
  Component: LoginPage
  Purpose: User authentication
  Features: Login form, validation, error handling, "Remember me"
  
/register  
  Component: RegisterPage
  Purpose: Reader self-registration
  Features: Registration form, validation, terms acceptance
  
/forgot-password
  Component: ForgotPasswordPage
  Purpose: Password recovery
  Features: Email input, verification, password reset
```

#### **2. Admin Pages (10 pages)**
```tsx
/admin/dashboard
  Component: AdminDashboard
  Purpose: Overview statistics
  Features: Stats cards, charts, recent activities
  
/admin/users
  Component: UserListPage
  Purpose: Manage all users
  Features: User table, search, filter by role, CRUD operations
  
/admin/users/create
  Component: CreateUserPage
  Purpose: Add new user
  Features: User form, role selection, profile fields
  
/admin/books
  Component: BookListPage
  Purpose: Manage books
  Features: Book grid/table view, search, filter, pagination
  
/admin/books/create
  Component: CreateBookPage
  Purpose: Add new book
  Features: Book form, image upload, category/author/publisher selection
  
/admin/books/{id}
  Component: BookDetailPage
  Purpose: View book details
  Features: Full info, QR code, borrow history, edit/delete buttons
  
/admin/books/{id}/edit
  Component: EditBookPage
  Purpose: Edit book information
  Features: Pre-filled form, image replacement
  
/admin/categories
  Component: CategoryManagementPage
  Purpose: Manage categories
  Features: Tree view, drag-drop, CRUD operations
  
/admin/reports
  Component: ReportsPage
  Purpose: View statistics and reports
  Features: Date range filter, charts, export buttons
  
/admin/settings
  Component: SettingsPage
  Purpose: System configuration
  Features: Config editor, save changes
```

#### **3. Librarian Pages (6 pages)**
```tsx
/librarian/dashboard
  Component: LibrarianDashboard
  Purpose: Daily operations overview
  Features: Today's borrows/returns, pending tasks
  
/librarian/borrow
  Component: BorrowBooksPage
  Purpose: Process book borrowing
  Features: Reader search, book selection, QR scanner, borrow form
  
/librarian/return
  Component: ReturnBooksPage
  Purpose: Process book returns
  Features: Search borrow records, condition check, fine calculation
  
/librarian/borrow-history
  Component: BorrowHistoryPage
  Purpose: View all borrow records
  Features: Table view, search, filter by status/date
  
/librarian/readers
  Component: ReaderListPage
  Purpose: View reader information
  Features: Reader table, search, view borrow history
  
/librarian/fines
  Component: FineManagementPage
  Purpose: Manage fines and payments
  Features: Unpaid fines list, payment processing
```

#### **4. Reader Pages (5 pages)**
```tsx
/reader/dashboard
  Component: ReaderDashboard
  Purpose: Personal overview
  Features: Current borrows, due dates, notifications
  
/reader/catalog
  Component: BookCatalogPage
  Purpose: Browse available books
  Features: Book cards, search, filter, pagination
  
/reader/my-books
  Component: MyBooksPage
  Purpose: View current borrowed books
  Features: Book list, due dates, extend button
  
/reader/history
  Component: BorrowHistoryPage
  Purpose: View borrowing history
  Features: Past borrows, fines paid, export
  
/reader/profile
  Component: ProfilePage
  Purpose: Manage profile
  Features: View/edit profile, change password, preferences
```

#### **5. Common Pages (2 pages)**
```tsx
/notifications
  Component: NotificationsPage
  Purpose: View all notifications
  Features: Notification list, filter, mark as read
  
/404
  Component: NotFoundPage
  Purpose: Handle invalid routes
  Features: Error message, back to home button
```

---

### **B. COMPONENTS (50+ Components)**

#### **1. Layout Components (5 components)**
```tsx
// components/layout/

<MainLayout>
  Purpose: Wrap all authenticated pages
  Props: children, role
  Features: Header, Sidebar, Content area, Footer
  
<Header>
  Purpose: Top navigation bar
  Features: Logo, search bar, notification icon, user menu
  
<Sidebar>
  Purpose: Side navigation menu
  Props: role (determines menu items)
  Features: Collapsible, role-based menu, active state
  
<Footer>
  Purpose: Bottom information
  Features: Copyright, links, contact
  
<Breadcrumb>
  Purpose: Show current page path
  Props: items: [{label, path}]
  Features: Clickable navigation
```

#### **2. Common/Reusable Components (15 components)**
```tsx
// components/common/

<Button>
  Props: variant, size, loading, disabled, onClick
  Variants: primary, secondary, danger, success
  
<Input>
  Props: type, label, value, onChange, error, required
  Types: text, email, password, number, date
  
<TextArea>
  Props: label, value, rows, onChange, error
  
<Select>
  Props: label, options, value, onChange, error
  Features: Single/multi select
  
<Checkbox>
  Props: label, checked, onChange
  
<Radio>
  Props: label, name, value, checked, onChange
  
<Modal>
  Props: isOpen, onClose, title, size
  Sizes: sm, md, lg, xl
  
<ConfirmDialog>
  Props: isOpen, title, message, onConfirm, onCancel
  Purpose: Confirmation prompts
  
<Toast>
  Props: message, type, duration
  Types: success, error, warning, info
  
<Spinner>
  Props: size, color
  Purpose: Loading indicator
  
<Pagination>
  Props: currentPage, totalPages, onPageChange
  
<SearchBar>
  Props: placeholder, value, onSearch, onClear
  
<DatePicker>
  Props: label, value, onChange, minDate, maxDate
  
<FileUpload>
  Props: label, accept, maxSize, onUpload
  Purpose: Image/file upload with preview
  
<Badge>
  Props: text, variant
  Variants: primary, success, warning, danger
```

#### **3. Book Components (8 components)**
```tsx
// components/features/books/

<BookCard>
  Props: book, showActions
  Purpose: Display book in card format
  Features: Cover image, title, author, availability badge
  
<BookTable>
  Props: books, onEdit, onDelete, onView
  Purpose: Display books in table format
  Features: Sortable columns, row actions
  
<BookForm>
  Props: initialData, onSubmit, onCancel
  Purpose: Add/Edit book form
  Fields: All book fields with validation
  
<BookDetail>
  Props: book
  Purpose: Display full book information
  Features: All fields, QR code, locate button
  
<BookSearch>
  Props: onSearch
  Purpose: Advanced book search
  Features: Multiple filters, autocomplete
  
<QRCodeDisplay>
  Props: bookId, qrCodeUrl
  Purpose: Show and download QR code
  Features: Display, download, print
  
<QRScanner>
  Props: onScan, onError
  Purpose: Scan QR codes with camera
  Features: Camera access, scan feedback
  
<CategoryTree>
  Props: categories, onSelect
  Purpose: Display category hierarchy
  Features: Expandable tree, selection
```

#### **4. User Components (5 components)**
```tsx
// components/features/users/

<UserCard>
  Props: user
  Purpose: Display user info card
  
<UserTable>
  Props: users, onEdit, onDelete
  Purpose: Display users in table
  
<UserForm>
  Props: initialData, onSubmit
  Purpose: Add/Edit user form
  Fields: User and profile fields
  
<RoleSelector>
  Props: value, onChange
  Purpose: Select user role
  Options: Admin, Librarian, Reader
  
<UserProfileModal>
  Props: userId, isOpen, onClose
  Purpose: View user profile in modal
```

#### **5. Borrowing Components (7 components)**
```tsx
// components/features/borrowing/

<BorrowForm>
  Props: onSubmit
  Purpose: Process book borrowing
  Features: Reader search, book selection, notes
  
<ReturnForm>
  Props: borrowRecord, onSubmit
  Purpose: Process book returns
  Features: Condition check, fine display
  
<BorrowTable>
  Props: borrows, onView, onReturn
  Purpose: Display borrow records
  
<BorrowDetailModal>
  Props: borrowId, isOpen, onClose
  Purpose: View borrow details in modal
  
<FineCalculator>
  Props: dueDate, returnDate
  Purpose: Calculate and display fine
  
<ExtendBorrowModal>
  Props: borrowDetailId, isOpen, onExtend
  Purpose: Extend due date
  
<ReaderSelector>
  Props: onSelect
  Purpose: Search and select reader
  Features: Autocomplete search
```

#### **6. Notification Components (3 components)**
```tsx
// components/features/notifications/

<NotificationDropdown>
  Purpose: Show recent notifications in dropdown
  Features: Unread count badge, mark as read
  
<NotificationList>
  Props: notifications
  Purpose: Display list of notifications
  
<NotificationItem>
  Props: notification, onRead, onDelete
  Purpose: Single notification item
  Features: Icon, message, timestamp, actions
```

#### **7. Report Components (7 components)**
```tsx
// components/features/reports/

<StatCard>
  Props: title, value, icon, color
  Purpose: Display single statistic
  
<LineChart>
  Props: data, labels, title
  Purpose: Show trends over time
  Library: Chart.js
  
<BarChart>
  Props: data, labels, title
  Purpose: Compare categories
  
<PieChart>
  Props: data, labels, title
  Purpose: Show proportions
  
<DateRangePicker>
  Props: from, to, onChange
  Purpose: Select date range for reports
  
<ExportButton>
  Props: reportType, format, data
  Purpose: Export reports
  Formats: CSV, PDF
  
<ReportFilters>
  Props: filters, onChange
  Purpose: Filter options for reports
```

---

### **C. REDUX STORE STRUCTURE**

#### **Store Slices (6 slices)**
```typescript
// store/

1. authSlice.ts
   State: {
     user: User | null
     token: string | null
     isAuthenticated: boolean
     loading: boolean
     error: string | null
   }
   Actions: login, logout, refreshToken, updateProfile
   
2. bookSlice.ts
   State: {
     books: Book[]
     selectedBook: Book | null
     loading: boolean
     error: string | null
     pagination: {page, totalPages, count}
   }
   Actions: fetchBooks, getBook, createBook, updateBook, deleteBook
   
3. borrowSlice.ts
   State: {
     borrows: BorrowRecord[]
     myBorrows: BorrowRecord[]
     loading: boolean
     error: string | null
   }
   Actions: borrowBooks, returnBooks, extendBorrow, fetchBorrows
   
4. notificationSlice.ts
   State: {
     notifications: Notification[]
     unreadCount: number
     loading: boolean
   }
   Actions: fetchNotifications, markAsRead, markAllAsRead
   
5. userSlice.ts (Admin only)
   State: {
     users: User[]
     loading: boolean
     error: string | null
   }
   Actions: fetchUsers, createUser, updateUser, deleteUser
   
6. uiSlice.ts
   State: {
     sidebarCollapsed: boolean
     language: 'vi' | 'en'
     theme: 'light' | 'dark'
   }
   Actions: toggleSidebar, setLanguage, setTheme
```

---

### **D. TYPE DEFINITIONS**

#### **TypeScript Interfaces (15+ interfaces)**
```typescript
// types/

interface User {
  id: number
  username: string
  email: string
  first_name: string
  last_name: string
  profile: Profile
  date_joined: string
}

interface Profile {
  id: number
  role: Role
  phone: string
  address: string
  student_id?: string
  faculty?: string
  grade?: string
  profile_picture?: string
}

interface Role {
  id: number
  name: string
  code: 'ADMIN' | 'LIBRARIAN' | 'READER'
  description: string
}

interface Book {
  id: number
  title: string
  isbn?: string
  barcode?: string
  author: number
  author_detail?: Author
  publisher: number
  publisher_detail?: Publisher
  category: number
  category_detail?: Category
  publication_year?: number
  language: string
  pages?: number
  description?: string
  cover_image?: string
  cover_image_url?: string
  quantity: number
  available: number
  location_shelf?: string
  location_row?: string
  qr_code?: string
  qr_code_url?: string
  price?: number
  status: 'ACTIVE' | 'INACTIVE'
  created_at: string
  updated_at: string
}

interface Category {
  id: number
  name: string
  code: string
  parent?: number
  description?: string
  children?: Category[]
}

interface Author {
  id: number
  name: string
  bio?: string
  nationality?: string
}

interface Publisher {
  id: number
  name: string
  address?: string
  phone?: string
  email?: string
  website?: string
}

interface BorrowRecord {
  id: number
  reader: number
  reader_detail?: User
  librarian: number
  librarian_detail?: User
  borrow_date: string
  due_date: string
  return_date?: string
  status: 'BORROWED' | 'RETURNED' | 'OVERDUE'
  total_fine: number
  notes?: string
  details: BorrowDetail[]
}

interface BorrowDetail {
  id: number
  borrow_record: number
  book: number
  book_detail?: Book
  due_date: string
  return_date?: string
  extension_count: number
  fine_amount: number
  status: 'BORROWED' | 'RETURNED' | 'OVERDUE' | 'LOST'
  condition_borrow?: string
  condition_return?: string
}

interface Notification {
  id: number
  user: number
  title: string
  message: string
  notification_type: 'DUE_SOON' | 'OVERDUE' | 'AVAILABLE' | 'ANNOUNCEMENT'
  link?: string
  is_read: boolean
  read_at?: string
  created_at: string
}

interface DashboardStats {
  total_books: number
  total_users: number
  total_borrows: number
  active_borrows: number
  overdue_borrows: number
  books_by_category: CategoryStat[]
  borrowing_trend: TrendData[]
}
```

---

### **E. API SERVICE METHODS**

#### **API Services (7 service files)**
```typescript
// api/

1. authService.ts
   - login(username, password): Promise<AuthResponse>
   - register(data): Promise<User>
   - logout(refreshToken): Promise<void>
   - refreshToken(refresh): Promise<TokenResponse>
   - getCurrentUser(): Promise<User>
   - updateProfile(data): Promise<Profile>
   - changePassword(oldPassword, newPassword): Promise<void>

2. bookService.ts
   - getBooks(params): Promise<PaginatedResponse<Book>>
   - getBook(id): Promise<Book>
   - createBook(data: FormData): Promise<Book>
   - updateBook(id, data: FormData): Promise<Book>
   - deleteBook(id): Promise<void>
   - getQRCode(id): Promise<QRCodeResponse>
   - scanQR(qrData): Promise<Book>
   - locateBook(id): Promise<void>
   - getCategories(): Promise<Category[]>
   - getAuthors(search): Promise<Author[]>
   - getPublishers(search): Promise<Publisher[]>

3. borrowService.ts
   - borrowBooks(data): Promise<BorrowRecord>
   - returnBooks(data): Promise<ReturnResponse>
   - extendBorrow(detailId): Promise<BorrowDetail>
   - getMyBorrows(params): Promise<PaginatedResponse<BorrowRecord>>
   - getAllBorrows(params): Promise<PaginatedResponse<BorrowRecord>>
   - getBorrowDetail(id): Promise<BorrowRecord>
   - getOverdue(): Promise<BorrowRecord[]>
   - payFine(violationId, amount): Promise<PaymentResponse>

4. notificationService.ts
   - getNotifications(params): Promise<PaginatedResponse<Notification>>
   - getNotification(id): Promise<Notification>
   - markAsRead(id): Promise<void>
   - markAllAsRead(): Promise<void>

5. userService.ts
   - getUsers(params): Promise<PaginatedResponse<User>>
   - getUser(id): Promise<User>
   - createUser(data): Promise<User>
   - updateUser(id, data): Promise<User>
   - deleteUser(id): Promise<void>

6. reportService.ts
   - getDashboardStats(params): Promise<DashboardStats>
   - getBooksByCategory(): Promise<CategoryStat[]>
   - getMostBorrowed(limit): Promise<BookStat[]>
   - getOverdueStats(): Promise<OverdueStats>
   - getUserActivity(userId, params): Promise<Activity[]>
   - exportReport(type, format, params): Promise<Blob>

7. systemService.ts
   - getConfig(): Promise<SystemConfig>
   - updateConfig(data): Promise<SystemConfig>
   - getActivityLog(params): Promise<PaginatedResponse<ActivityLog>>
```

---

### **F. STYLING & THEMING**

#### **1. Bootstrap Customization**
```scss
// styles/custom-bootstrap.scss

$primary: #007bff;
$secondary: #6c757d;
$success: #28a745;
$danger: #dc3545;
$warning: #ffc107;
$info: #17a2b8;

$font-family-base: 'Inter', sans-serif;
$font-size-base: 1rem;
$line-height-base: 1.5;

$border-radius: 0.375rem;
$box-shadow: 0 0.125rem 0.25rem rgba(0, 0, 0, 0.075);

// Import Bootstrap
@import '~bootstrap/scss/bootstrap';
```

#### **2. Global Styles**
```css
/* styles/global.css */

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Inter', sans-serif;
  background-color: #f8f9fa;
}

.container-fluid {
  padding: 1.5rem;
}

.card {
  border: none;
  box-shadow: 0 0.125rem 0.25rem rgba(0, 0, 0, 0.075);
  margin-bottom: 1.5rem;
}

.btn {
  font-weight: 500;
  padding: 0.5rem 1rem;
}

.table {
  background-color: white;
}

/* Loading states */
.skeleton {
  animation: pulse 1.5s ease-in-out infinite;
  background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
  background-size: 200% 100%;
}

@keyframes pulse {
  0% { background-position: 200% 0; }
  100% { background-position: -200% 0; }
}
```

#### **3. Responsive Breakpoints**
```scss
// Mobile first approach
// XS: < 576px (default)
// SM: >= 576px
// MD: >= 768px
// LG: >= 992px
// XL: >= 1200px
// XXL: >= 1400px

@media (max-width: 767px) {
  .sidebar {
    transform: translateX(-100%);
  }
  
  .card {
    margin-bottom: 1rem;
  }
}
```

---

## 📊 TỔNG KẾT CÔNG VIỆC FRONTEND

| Category | Count | Details |
|----------|-------|---------|
| **Pages** | 25+ | Public (3), Admin (10), Librarian (6), Reader (5), Common (2) |
| **Components** | 50+ | Layout (5), Common (15), Books (8), Users (5), Borrowing (7), Notifications (3), Reports (7) |
| **Redux Slices** | 6 | auth, book, borrow, notification, user, ui |
| **TypeScript Interfaces** | 15+ | User, Book, Category, Author, Publisher, BorrowRecord, etc. |
| **API Services** | 7 | auth, book, borrow, notification, user, report, system |
| **Forms** | 10+ | Login, Register, BookForm, UserForm, BorrowForm, etc. |
| **Charts** | 3 | Line chart, Bar chart, Pie chart |
| **Modals** | 5+ | Confirm, BookDetail, UserProfile, BorrowDetail, Extend |

---

## 📅 TIMELINE CHI TIẾT

### **THÁNG 1: FOUNDATION & AUTHENTICATION UI**

#### **Week 1: Project Setup (5 days)**
**Branch:** `feature/frontend-setup`

**Monday - Tuesday (2 days):**
- [ ] Setup Node.js + npm
- [ ] Create React app với TypeScript template
- [ ] Install dependencies (React Router, Redux, Bootstrap, etc.)
- [ ] Configure TypeScript (tsconfig.json)
- [ ] Setup folder structure
- [ ] Configure path aliases

```bash
# Commands
npx create-react-app library-frontend --template typescript
cd library-frontend
npm install react-router-dom @reduxjs/toolkit react-redux
npm install bootstrap react-bootstrap
npm install axios
npm install react-i18next i18next
npm install html5-qrcode
npm install chart.js react-chartjs-2
npm install @types/node --save-dev
```

**Folder Structure:**
```
src/
├── api/                 # API services
├── assets/              # Images, icons
├── components/          # Reusable components
│   ├── common/         # Button, Input, Modal, etc.
│   ├── layout/         # Header, Footer, Sidebar
│   └── features/       # Feature-specific components
├── pages/              # Page components
│   ├── auth/
│   ├── admin/
│   ├── librarian/
│   └── reader/
├── store/              # Redux store
├── types/              # TypeScript types
├── utils/              # Helper functions
├── hooks/              # Custom hooks
├── styles/             # Global styles
└── App.tsx
```

**Wednesday - Thursday (2 days):**
- [ ] Configure Axios instance
- [ ] Setup Redux store structure
- [ ] Create base layout components (Header, Footer, Sidebar)
- [ ] Configure React Router
- [ ] Setup Bootstrap theme customization
- [ ] Create global styles

**Friday:**
- [ ] Test dev server runs
- [ ] Test routing works
- [ ] Test Redux DevTools
- [ ] Document setup
- [ ] Commit: `feat(setup): initialize React TypeScript frontend`

**Deliverables:**
- ✅ React app chạy được
- ✅ Routing configured
- ✅ Redux store setup

---

#### **Week 2-3: Login & Authentication UI (10 days)**
**Branch:** `feature/login-ui`

**Week 2 - Monday-Tuesday (2 days):**
- [ ] Design Login page UI
- [ ] Create Login form component
- [ ] Create Input components (TextInput, PasswordInput)
- [ ] Create Button component
- [ ] Add form validation

**Week 2 - Wednesday-Thursday (2 days):**
- [ ] Integrate Login API
- [ ] Create auth slice (Redux)
- [ ] Implement login logic
- [ ] Store JWT tokens in localStorage
- [ ] Add loading states
- [ ] Add error handling

**Week 2 - Friday (1 day):**
- [ ] Create Register page (for readers)
- [ ] Create registration form
- [ ] Integrate Register API
- [ ] Add form validation

**Week 3 - Monday-Tuesday (2 days):**
- [ ] Create ProtectedRoute component
- [ ] Implement role-based routing
- [ ] Create different dashboards:
  - Admin Dashboard
  - Librarian Dashboard
  - Reader Dashboard
- [ ] Add navigation based on role

**Week 3 - Wednesday-Thursday (2 days):**
- [ ] Create Header với user menu
- [ ] Add logout functionality
- [ ] Create Profile page
- [ ] Implement profile update
- [ ] Add password change form

**Week 3 - Friday (1 day):**
- [ ] Test authentication flow
- [ ] Test role-based access
- [ ] Fix UI bugs
- [ ] Responsive testing
- [ ] Commit: `feat(auth): implement authentication UI and protected routes`

**Pages Created:**
```
/login
/register
/admin/dashboard
/librarian/dashboard
/reader/dashboard
/profile
/change-password
```

**Deliverables:**
- ✅ Authentication UI complete
- ✅ Role-based routing works
- ✅ Responsive design

---

#### **Week 4: Book Management UI (5 days)**
**Branch:** `feature/book-ui`

**Monday-Tuesday (2 days):**
- [ ] Design Book List page
- [ ] Create BookCard component
- [ ] Create BookTable component
- [ ] Add search and filter UI
- [ ] Add pagination component
- [ ] Integrate Book List API

**Wednesday-Thursday (2 days):**
- [ ] Create Add/Edit Book form
- [ ] Create BookForm component với validation
- [ ] Add image upload preview
- [ ] Integrate Create/Update Book API
- [ ] Add delete confirmation modal

**Friday:**
- [ ] Create Book Detail page
- [ ] Display book information
- [ ] Show QR code
- [ ] Add "Locate Book" button (for IoT)
- [ ] Test CRUD operations
- [ ] Commit: `feat(books): implement book management UI`

**Pages Created:**
```
/admin/books
/admin/books/create
/admin/books/{id}
/admin/books/{id}/edit
```

**Deliverables:**
- ✅ Book CRUD UI complete
- ✅ Search and filter working
- ✅ Image upload working

---

### **THÁNG 2: CORE FEATURES UI**

#### **Week 5: Borrowing System UI (5 days)**
**Branch:** `feature/borrow-ui`

**Monday-Tuesday (2 days):**
- [ ] Design Borrowing page for librarian
- [ ] Create BorrowForm component
- [ ] Add book search/selection
- [ ] Add reader search
- [ ] Show available quantity
- [ ] Add validation

**Wednesday-Thursday (2 days):**
- [ ] Create Return Books page
- [ ] Create ReturnForm component
- [ ] Show borrowed books list
- [ ] Calculate fines
- [ ] Integrate Borrow/Return APIs

**Friday:**
- [ ] Create Borrow History page
- [ ] Add filters (date range, status)
- [ ] Create BorrowDetailModal
- [ ] Test borrow/return flow
- [ ] Commit: `feat(borrowing): implement borrowing UI`

**Pages Created:**
```
/librarian/borrow
/librarian/return
/librarian/borrow-history
```

**Deliverables:**
- ✅ Borrow/Return UI working
- ✅ Fine calculation displayed

---

#### **Week 6: QR Code Scanner UI (5 days)**
**Branch:** `feature/qr-scanner-ui`

**Monday-Wednesday (3 days):**
- [ ] Integrate html5-qrcode library
- [ ] Create QRScanner component
- [ ] Add camera permissions handling
- [ ] Implement scan book flow
- [ ] Show scanned book info
- [ ] Add manual ISBN input fallback

**Thursday-Friday (2 days):**
- [ ] Create QR code display component
- [ ] Add "Download QR" button
- [ ] Add "Print QR" functionality
- [ ] Test scanning on mobile
- [ ] Test on different browsers
- [ ] Commit: `feat(qr): implement QR code scanner UI`

**Components:**
```tsx
<QRScanner onScan={handleScan} />
<QRCodeDisplay bookId={id} />
```

**Deliverables:**
- ✅ QR Scanner works on camera
- ✅ QR display and download

---

#### **Week 7: Reader Portal UI (5 days)**
**Branch:** `feature/reader-dashboard`

**Monday-Tuesday (2 days):**
- [ ] Design Reader Dashboard
- [ ] Create DashboardCard component
- [ ] Show current borrowed books
- [ ] Show upcoming due dates
- [ ] Show notifications
- [ ] Show statistics

**Wednesday-Thursday (2 days):**
- [ ] Create Book Catalog page (for readers)
- [ ] Show available books
- [ ] Add search and filters
- [ ] Create BookDetailModal
- [ ] Add "Request to Borrow" (optional)

**Friday:**
- [ ] Create Reader History page
- [ ] Show past borrows
- [ ] Show fines paid
- [ ] Add export functionality
- [ ] Test reader flow
- [ ] Commit: `feat(reader): implement reader portal UI`

**Pages Created:**
```
/reader/dashboard
/reader/catalog
/reader/my-books
/reader/history
```

**Deliverables:**
- ✅ Reader portal complete
- ✅ Book catalog functional

---

#### **Week 8: Notifications UI (5 days)**
**Branch:** `feature/notification-ui`

**Monday-Wednesday (3 days):**
- [ ] Create Notification dropdown in Header
- [ ] Create NotificationItem component
- [ ] Show unread count badge
- [ ] Implement mark as read
- [ ] Add notification sound (optional)
- [ ] Integrate Notification API

**Thursday-Friday (2 days):**
- [ ] Create Notifications page
- [ ] Add filter (read/unread, type)
- [ ] Add "Mark all as read"
- [ ] Add "Delete" notifications
- [ ] Test real-time updates (polling)
- [ ] Commit: `feat(notifications): implement notification UI`

**Components:**
```tsx
<NotificationDropdown />
<NotificationList />
<NotificationItem />
```

**Deliverables:**
- ✅ Notification system works
- ✅ Real-time updates

---

### **THÁNG 3: POLISH & DEPLOYMENT**

#### **Week 9: Reports & Charts UI (5 days)**
**Branch:** `feature/charts-ui`

**Monday-Tuesday (2 days):**
- [ ] Design Reports Dashboard
- [ ] Create StatCard component
- [ ] Show key metrics (cards)
- [ ] Integrate Statistics API

**Wednesday-Thursday (2 days):**
- [ ] Create charts với Chart.js:
  - Books by Category (Bar chart)
  - Borrowing Trends (Line chart)
  - Most Borrowed Books (Pie chart)
  - User Activity (Area chart)
- [ ] Add date range picker
- [ ] Add filter options

**Friday:**
- [ ] Create Export Reports page
- [ ] Add export to CSV/PDF buttons
- [ ] Create print-friendly views
- [ ] Test charts responsive
- [ ] Commit: `feat(reports): implement reports and charts UI`

**Pages Created:**
```
/admin/reports
/admin/reports/borrowing
/admin/reports/books
/admin/reports/users
```

**Deliverables:**
- ✅ Dashboard với charts
- ✅ Export functionality

---

#### **Week 10: Internationalization & UI Polish (5 days)**
**Branch:** `feature/i18n-frontend`

**Monday-Tuesday (2 days):**
- [ ] Setup i18next
- [ ] Create translation files (en.json, vi.json)
- [ ] Translate all strings
- [ ] Add language switcher in Header
- [ ] Test language switching

**Wednesday-Friday (3 days):**
- [ ] UI polish:
  - Improve spacing, colors
  - Add loading skeletons
  - Add empty states
  - Add error states
  - Improve animations
- [ ] Add tooltips
- [ ] Improve form validation messages
- [ ] Test accessibility (a11y)
- [ ] Commit: `feat(i18n): add internationalization and UI polish`

**Deliverables:**
- ✅ Multi-language support
- ✅ Polished UI/UX

---

#### **Week 11: Testing & Bug Fixes (5 days)**
**Branch:** `test/e2e-tests`

**Monday-Wednesday (3 days):**
- [ ] Manual testing tất cả features
- [ ] Test trên nhiều browsers (Chrome, Firefox, Edge)
- [ ] Test responsive (mobile, tablet, desktop)
- [ ] Fix UI bugs
- [ ] Fix responsiveness issues

**Thursday-Friday (2 days):**
- [ ] Write component tests (optional)
- [ ] Performance optimization
- [ ] Code splitting
- [ ] Lazy loading routes
- [ ] Optimize images
- [ ] Commit: `test: e2e testing and bug fixes`

**Testing Checklist:**
- [ ] Login/Logout
- [ ] Book CRUD
- [ ] Borrow/Return
- [ ] QR Scanning
- [ ] Notifications
- [ ] Reports
- [ ] Language switching
- [ ] Mobile responsive

---

#### **Week 12: Build & Deployment Documentation (5 days)**
**Branch:** `docs/deployment-guide`

**Monday-Tuesday (2 days):**
- [ ] Create production build
- [ ] Optimize build size
- [ ] Test production build locally
- [ ] Configure environment variables
- [ ] Create `.env.example`

**Wednesday-Thursday (2 days):**
- [ ] Create deployment guide
- [ ] Document build process
- [ ] Document environment setup
- [ ] Create troubleshooting guide
- [ ] Take screenshots for docs

**Friday:**
- [ ] Final testing on production
- [ ] Monitor console errors
- [ ] Fix production issues
- [ ] Update README
- [ ] Commit: `docs: add deployment guide and finalize frontend`

**Deliverables:**
- ✅ Production build ready
- ✅ Deployment documented
- ✅ Frontend complete

---

## 🎨 DESIGN GUIDELINES

### **Color Scheme:**
```css
Primary: #007bff (Blue)
Success: #28a745 (Green)
Warning: #ffc107 (Yellow)
Danger: #dc3545 (Red)
Background: #f8f9fa
Text: #212529
```

### **Typography:**
```css
Font Family: 'Inter', sans-serif
Heading: 600 weight
Body: 400 weight
Small: 14px
Normal: 16px
Large: 18px
```

### **Spacing:**
```css
XS: 4px
SM: 8px
MD: 16px
LG: 24px
XL: 32px
```

### **Responsive Breakpoints:**
```css
Mobile: < 576px
Tablet: 576px - 768px
Desktop: > 768px
Large: > 1200px
```

---

## 🔧 CÔNG CỤ & SETUP

### **Development Tools:**
```
- VS Code with React extensions
- Chrome DevTools
- React Developer Tools
- Redux DevTools
- Figma/Adobe XD (for design)
- Postman (for API testing)
```

### **VS Code Extensions:**
```
- ES7+ React/Redux/React-Native snippets
- ESLint
- Prettier
- Auto Rename Tag
- Bracket Pair Colorizer
- GitLens
- Path Intellisense
```

### **Daily Workflow:**
```bash
# Morning
git checkout develop
git pull origin develop
git checkout -b feature/your-feature

# During work
npm start  # Run dev server

# Before push
npm run build  # Test build
git add .
git commit -m "feat(module): description"
git push origin feature/your-feature

# Create PR for review
```

---

## 📊 KPI & METRICS

### **Week 1-4:**
- ✅ Frontend setup complete
- ✅ Authentication UI working
- ✅ Book management UI functional
- ✅ 10+ pages created

### **Week 5-8:**
- ✅ Borrowing UI complete
- ✅ QR scanner working
- ✅ Reader portal functional
- ✅ Notifications working
- ✅ 20+ pages total

### **Week 9-12:**
- ✅ Reports with charts
- ✅ Multi-language support
- ✅ 25+ pages total
- ✅ 100% responsive
- ✅ Production ready

---

## 📝 CODE REVIEW RESPONSIBILITIES

Bạn sẽ review code của:
- **Dev 3:** UI components, styling, responsive design

**Review checklist:**
- Component reusability?
- Props properly typed?
- Responsive design?
- Accessibility?
- Code style consistent?
- Performance issues?

---

## 🆘 SUPPORT & COMMUNICATION

### **Daily Standup (10 phút mỗi sáng):**
- Hôm qua làm gì?
- Hôm nay làm gì?
- Có blockers không?

### **Weekly Design Review:**
- Demo UI progress
- Get feedback
- Discuss UX improvements

---

## 📚 HỌC LIỆU THAM KHẢO

- [React Documentation](https://react.dev/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Redux Toolkit](https://redux-toolkit.js.org/)
- [React Router](https://reactrouter.com/)
- [Bootstrap 5](https://getbootstrap.com/)
- [Chart.js](https://www.chartjs.org/)
- [html5-qrcode](https://github.com/mebjas/html5-qrcode)

---

**📝 Last Updated:** November 2, 2025  
**👤 Developer:** Frontend Lead  
**📧 Contact:** Team Lead for questions
