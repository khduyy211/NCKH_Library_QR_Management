# HƯỚNG DẪN TỪNG BƯỚC - PHẦN 2

## 5. KẾT NỐI BACKEND VÀ FRONTEND

### 🔗 **Test kết nối**

#### **Bước 1: Chạy Backend**
```powershell
# Terminal 1 - Backend
cd D:\NCKH\demo\library_backend
.\venv\Scripts\activate
python manage.py runserver

# Output: http://127.0.0.1:8000/
```

#### **Bước 2: Chạy Frontend**
```powershell
# Terminal 2 - Frontend
cd D:\NCKH\demo\library-frontend
npm start

# Output: http://localhost:3000/
```

#### **Bước 3: Test Login**

**Tạo test user trong Django:**
```powershell
# Terminal 1
python manage.py createsuperuser
# Username: admin
# Email: admin@example.com
# Password: admin123
```

**Test login từ React:**
```typescript
// src/pages/auth/LoginPage.tsx

import React, { useState } from 'react';
import { useDispatch } from 'react-redux';
import { useNavigate } from 'react-router-dom';
import { login } from '@/store/authSlice';
import { Form, Button, Alert } from 'react-bootstrap';

const LoginPage: React.FC = () => {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');
  const [error, setError] = useState('');
  const [loading, setLoading] = useState(false);
  
  const dispatch = useDispatch();
  const navigate = useNavigate();

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    setLoading(true);
    setError('');

    try {
      await dispatch(login({ username, password })).unwrap();
      navigate('/admin/dashboard');
    } catch (err: any) {
      setError(err || 'Login failed');
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="container mt-5">
      <div className="row justify-content-center">
        <div className="col-md-6">
          <div className="card">
            <div className="card-body">
              <h3 className="card-title text-center mb-4">Login</h3>
              
              {error && <Alert variant="danger">{error}</Alert>}
              
              <Form onSubmit={handleSubmit}>
                <Form.Group className="mb-3">
                  <Form.Label>Username</Form.Label>
                  <Form.Control
                    type="text"
                    value={username}
                    onChange={(e) => setUsername(e.target.value)}
                    required
                  />
                </Form.Group>

                <Form.Group className="mb-3">
                  <Form.Label>Password</Form.Label>
                  <Form.Control
                    type="password"
                    value={password}
                    onChange={(e) => setPassword(e.target.value)}
                    required
                  />
                </Form.Group>

                <Button
                  type="submit"
                  variant="primary"
                  className="w-100"
                  disabled={loading}
                >
                  {loading ? 'Logging in...' : 'Login'}
                </Button>
              </Form>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
};

export default LoginPage;
```

---

### 🐛 **CORS Issues & Solutions**

#### **Issue: CORS Error**
```
Access to XMLHttpRequest at 'http://localhost:8000/api/...' 
from origin 'http://localhost:3000' has been blocked by CORS policy
```

#### **Solution:**

**Backend (Django):**
```python
# library_backend/settings.py

INSTALLED_APPS = [
    ...
    'corsheaders',
]

MIDDLEWARE = [
    'corsheaders.middleware.CorsMiddleware',  # Must be first
    'django.middleware.common.CommonMiddleware',
    ...
]

# Development
CORS_ALLOWED_ORIGINS = [
    "http://localhost:3000",
    "http://127.0.0.1:3000",
]

# Production
# CORS_ALLOWED_ORIGINS = [
#     "https://yourdomain.com",
# ]

CORS_ALLOW_CREDENTIALS = True
CORS_ALLOW_HEADERS = [
    'accept',
    'accept-encoding',
    'authorization',
    'content-type',
    'dnt',
    'origin',
    'user-agent',
    'x-csrftoken',
    'x-requested-with',
]
```

---

## 6. TRIỂN KHAI TÍNH NĂNG CỤ THỂ

### 📚 **Feature 1: Book Management**

#### **Backend: Book Model**
```python
# books/models.py

from django.db import models
from django.utils.text import slugify
import qrcode
from io import BytesIO
from django.core.files import File

class Category(models.Model):
    name = models.CharField(max_length=100)
    code = models.CharField(max_length=50, unique=True)
    parent = models.ForeignKey('self', on_delete=models.CASCADE, null=True, blank=True)
    description = models.TextField(null=True, blank=True)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    class Meta:
        db_table = 'books_category'
        verbose_name_plural = 'Categories'
    
    def __str__(self):
        return self.name


class Author(models.Model):
    name = models.CharField(max_length=200)
    bio = models.TextField(null=True, blank=True)
    nationality = models.CharField(max_length=100, null=True, blank=True)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    class Meta:
        db_table = 'books_author'
    
    def __str__(self):
        return self.name


class Publisher(models.Model):
    name = models.CharField(max_length=200)
    address = models.TextField(null=True, blank=True)
    phone = models.CharField(max_length=20, null=True, blank=True)
    email = models.EmailField(null=True, blank=True)
    website = models.URLField(null=True, blank=True)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    class Meta:
        db_table = 'books_publisher'
    
    def __str__(self):
        return self.name


class Book(models.Model):
    STATUS_CHOICES = [
        ('ACTIVE', 'Active'),
        ('INACTIVE', 'Inactive'),
    ]
    
    title = models.CharField(max_length=255)
    isbn = models.CharField(max_length=20, unique=True, null=True, blank=True)
    author = models.ForeignKey(Author, on_delete=models.SET_NULL, null=True)
    publisher = models.ForeignKey(Publisher, on_delete=models.SET_NULL, null=True)
    category = models.ForeignKey(Category, on_delete=models.SET_NULL, null=True)
    publication_year = models.IntegerField(null=True, blank=True)
    language = models.CharField(max_length=50, default='Vietnamese')
    pages = models.IntegerField(null=True, blank=True)
    description = models.TextField(null=True, blank=True)
    cover_image = models.ImageField(upload_to='books/covers/', null=True, blank=True)
    quantity = models.IntegerField(default=0)
    available = models.IntegerField(default=0)
    location_shelf = models.CharField(max_length=50, null=True, blank=True)
    location_row = models.CharField(max_length=50, null=True, blank=True)
    qr_code = models.ImageField(upload_to='books/qr_codes/', null=True, blank=True)
    barcode = models.CharField(max_length=50, null=True, blank=True)
    price = models.DecimalField(max_digits=10, decimal_places=2, null=True, blank=True)
    status = models.CharField(max_length=20, choices=STATUS_CHOICES, default='ACTIVE')
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    class Meta:
        db_table = 'books_book'
        ordering = ['-created_at']
    
    def __str__(self):
        return self.title
    
    def save(self, *args, **kwargs):
        # Set available = quantity if new book
        if not self.pk:
            self.available = self.quantity
        
        # Generate QR code if not exists
        if not self.qr_code:
            self.generate_qr_code()
        
        super().save(*args, **kwargs)
    
    def generate_qr_code(self):
        """Generate QR code for book"""
        qr_data = f"BOOK:{self.id}"  # Format: BOOK:123
        qr = qrcode.QRCode(version=1, box_size=10, border=5)
        qr.add_data(qr_data)
        qr.make(fit=True)
        
        img = qr.make_image(fill_color="black", back_color="white")
        
        # Save to BytesIO
        buffer = BytesIO()
        img.save(buffer, format='PNG')
        buffer.seek(0)
        
        # Save to model field
        filename = f'book_{self.id}_qr.png'
        self.qr_code.save(filename, File(buffer), save=False)
```

**💡 Giải thích:**
- **ForeignKey**: Relationship many-to-one
- **choices**: Dropdown options
- **upload_to**: Folder để lưu files
- **save() override**: Custom logic khi save
- **generate_qr_code()**: Tạo QR code tự động

---

#### **Backend: Book Serializer**
```python
# books/serializers.py

from rest_framework import serializers
from .models import Book, Category, Author, Publisher

class CategorySerializer(serializers.ModelSerializer):
    class Meta:
        model = Category
        fields = '__all__'


class AuthorSerializer(serializers.ModelSerializer):
    class Meta:
        model = Author
        fields = '__all__'


class PublisherSerializer(serializers.ModelSerializer):
    class Meta:
        model = Publisher
        fields = '__all__'


class BookSerializer(serializers.ModelSerializer):
    # Nested serializers (read-only)
    author_detail = AuthorSerializer(source='author', read_only=True)
    publisher_detail = PublisherSerializer(source='publisher', read_only=True)
    category_detail = CategorySerializer(source='category', read_only=True)
    
    # Foreign keys (write-only)
    author_id = serializers.IntegerField(write_only=True, required=False)
    publisher_id = serializers.IntegerField(write_only=True, required=False)
    category_id = serializers.IntegerField(write_only=True, required=False)
    
    # Custom fields
    qr_code_url = serializers.SerializerMethodField()
    cover_image_url = serializers.SerializerMethodField()
    
    class Meta:
        model = Book
        fields = [
            'id', 'title', 'isbn', 'barcode',
            'author', 'author_id', 'author_detail',
            'publisher', 'publisher_id', 'publisher_detail',
            'category', 'category_id', 'category_detail',
            'publication_year', 'language', 'pages',
            'description', 'cover_image', 'cover_image_url',
            'quantity', 'available',
            'location_shelf', 'location_row',
            'qr_code', 'qr_code_url',
            'price', 'status',
            'created_at', 'updated_at'
        ]
        read_only_fields = ['id', 'qr_code', 'created_at', 'updated_at', 'available']
    
    def get_qr_code_url(self, obj):
        if obj.qr_code:
            request = self.context.get('request')
            if request:
                return request.build_absolute_uri(obj.qr_code.url)
        return None
    
    def get_cover_image_url(self, obj):
        if obj.cover_image:
            request = self.context.get('request')
            if request:
                return request.build_absolute_uri(obj.cover_image.url)
        return None


class BookListSerializer(serializers.ModelSerializer):
    """Lightweight serializer for list view"""
    author_name = serializers.CharField(source='author.name', read_only=True)
    category_name = serializers.CharField(source='category.name', read_only=True)
    cover_image_url = serializers.SerializerMethodField()
    
    class Meta:
        model = Book
        fields = [
            'id', 'title', 'isbn',
            'author_name', 'category_name',
            'cover_image_url', 'quantity', 'available',
            'status', 'created_at'
        ]
    
    def get_cover_image_url(self, obj):
        if obj.cover_image:
            request = self.context.get('request')
            if request:
                return request.build_absolute_uri(obj.cover_image.url)
        return None
```

**💡 Serializer patterns:**
- **Nested serializers**: Show related objects
- **SerializerMethodField**: Custom computed fields
- **write_only**: Không trả về trong response
- **List serializer**: Lightweight cho list views

---

#### **Backend: Book ViewSet**
```python
# books/views.py

from rest_framework import viewsets, status, filters
from rest_framework.decorators import action
from rest_framework.response import Response
from rest_framework.permissions import IsAuthenticated
from django_filters.rest_framework import DjangoFilterBackend
from django.db.models import Q
from .models import Book, Category, Author, Publisher
from .serializers import (
    BookSerializer, BookListSerializer,
    CategorySerializer, AuthorSerializer, PublisherSerializer
)

class BookViewSet(viewsets.ModelViewSet):
    queryset = Book.objects.all()
    permission_classes = [IsAuthenticated]
    filter_backends = [DjangoFilterBackend, filters.SearchFilter, filters.OrderingFilter]
    filterset_fields = ['category', 'author', 'publisher', 'status', 'language']
    search_fields = ['title', 'isbn', 'barcode', 'author__name', 'publisher__name']
    ordering_fields = ['title', 'created_at', 'publication_year']
    ordering = ['-created_at']
    
    def get_serializer_class(self):
        if self.action == 'list':
            return BookListSerializer
        return BookSerializer
    
    def get_queryset(self):
        queryset = super().get_queryset()
        
        # Filter by available only
        available_only = self.request.query_params.get('available_only')
        if available_only == 'true':
            queryset = queryset.filter(available__gt=0)
        
        return queryset
    
    @action(detail=True, methods=['get'])
    def qr_code(self, request, pk=None):
        """Get QR code for book"""
        book = self.get_object()
        if not book.qr_code:
            book.generate_qr_code()
            book.save()
        
        return Response({
            'qr_code_url': request.build_absolute_uri(book.qr_code.url),
            'qr_data': f'BOOK:{book.id}'
        })
    
    @action(detail=False, methods=['post'])
    def scan_qr(self, request):
        """Scan QR code to find book"""
        qr_data = request.data.get('qr_data')
        
        if not qr_data or not qr_data.startswith('BOOK:'):
            return Response(
                {'error': 'Invalid QR code'},
                status=status.HTTP_400_BAD_REQUEST
            )
        
        try:
            book_id = int(qr_data.split(':')[1])
            book = Book.objects.get(id=book_id)
            serializer = self.get_serializer(book)
            return Response(serializer.data)
        except (ValueError, Book.DoesNotExist):
            return Response(
                {'error': 'Book not found'},
                status=status.HTTP_404_NOT_FOUND
            )
    
    @action(detail=True, methods=['post'])
    def locate(self, request, pk=None):
        """Trigger LED to locate book (IoT)"""
        book = self.get_object()
        
        # TODO: Send command to IoT device
        # This will be implemented in IoT module
        
        return Response({
            'message': 'LED activated',
            'location': {
                'shelf': book.location_shelf,
                'row': book.location_row
            }
        })


class CategoryViewSet(viewsets.ModelViewSet):
    queryset = Category.objects.all()
    serializer_class = CategorySerializer
    permission_classes = [IsAuthenticated]
    
    @action(detail=False, methods=['get'])
    def tree(self, request):
        """Get category tree structure"""
        root_categories = Category.objects.filter(parent=None)
        serializer = self.get_serializer(root_categories, many=True)
        return Response(serializer.data)


class AuthorViewSet(viewsets.ModelViewSet):
    queryset = Author.objects.all()
    serializer_class = AuthorSerializer
    permission_classes = [IsAuthenticated]
    search_fields = ['name']


class PublisherViewSet(viewsets.ModelViewSet):
    queryset = Publisher.objects.all()
    serializer_class = PublisherSerializer
    permission_classes = [IsAuthenticated]
    search_fields = ['name']
```

**💡 ViewSet features:**
- **Filters**: DjangoFilter, Search, Ordering
- **Custom actions**: @action decorator
- **Query params**: request.query_params.get()
- **Dynamic serializer**: get_serializer_class()

---

#### **Frontend: Book Types**
```typescript
// src/types/book.ts

export interface Category {
  id: number;
  name: string;
  code: string;
  parent?: number;
  description?: string;
}

export interface Author {
  id: number;
  name: string;
  bio?: string;
  nationality?: string;
}

export interface Publisher {
  id: number;
  name: string;
  address?: string;
  phone?: string;
  email?: string;
  website?: string;
}

export interface Book {
  id: number;
  title: string;
  isbn?: string;
  barcode?: string;
  author: number;
  author_detail?: Author;
  publisher: number;
  publisher_detail?: Publisher;
  category: number;
  category_detail?: Category;
  publication_year?: number;
  language: string;
  pages?: number;
  description?: string;
  cover_image?: string;
  cover_image_url?: string;
  quantity: number;
  available: number;
  location_shelf?: string;
  location_row?: string;
  qr_code?: string;
  qr_code_url?: string;
  price?: number;
  status: 'ACTIVE' | 'INACTIVE';
  created_at: string;
  updated_at: string;
}

export interface BookFormData {
  title: string;
  isbn?: string;
  barcode?: string;
  author_id?: number;
  publisher_id?: number;
  category_id?: number;
  publication_year?: number;
  language: string;
  pages?: number;
  description?: string;
  cover_image?: File;
  quantity: number;
  location_shelf?: string;
  location_row?: string;
  price?: number;
  status: 'ACTIVE' | 'INACTIVE';
}
```

---

#### **Frontend: Book Service**
```typescript
// src/api/bookService.ts

import axios from './axios';
import { Book, BookFormData, Category, Author, Publisher } from '@/types/book';

interface PaginatedResponse<T> {
  count: number;
  next: string | null;
  previous: string | null;
  results: T[];
}

class BookService {
  // Books
  async getBooks(params?: any): Promise<PaginatedResponse<Book>> {
    const response = await axios.get('/books/', { params });
    return response.data;
  }

  async getBook(id: number): Promise<Book> {
    const response = await axios.get(`/books/${id}/`);
    return response.data;
  }

  async createBook(data: FormData): Promise<Book> {
    const response = await axios.post('/books/', data, {
      headers: { 'Content-Type': 'multipart/form-data' },
    });
    return response.data;
  }

  async updateBook(id: number, data: FormData): Promise<Book> {
    const response = await axios.put(`/books/${id}/`, data, {
      headers: { 'Content-Type': 'multipart/form-data' },
    });
    return response.data;
  }

  async deleteBook(id: number): Promise<void> {
    await axios.delete(`/books/${id}/`);
  }

  async getQRCode(id: number): Promise<{ qr_code_url: string; qr_data: string }> {
    const response = await axios.get(`/books/${id}/qr_code/`);
    return response.data;
  }

  async scanQR(qrData: string): Promise<Book> {
    const response = await axios.post('/books/scan_qr/', { qr_data: qrData });
    return response.data;
  }

  async locateBook(id: number): Promise<any> {
    const response = await axios.post(`/books/${id}/locate/`);
    return response.data;
  }

  // Categories
  async getCategories(): Promise<Category[]> {
    const response = await axios.get('/categories/');
    return response.data.results || response.data;
  }

  async createCategory(data: Partial<Category>): Promise<Category> {
    const response = await axios.post('/categories/', data);
    return response.data;
  }

  // Authors
  async getAuthors(search?: string): Promise<Author[]> {
    const response = await axios.get('/authors/', {
      params: { search },
    });
    return response.data.results || response.data;
  }

  async createAuthor(data: Partial<Author>): Promise<Author> {
    const response = await axios.post('/authors/', data);
    return response.data;
  }

  // Publishers
  async getPublishers(search?: string): Promise<Publisher[]> {
    const response = await axios.get('/publishers/', {
      params: { search },
    });
    return response.data.results || response.data;
  }

  async createPublisher(data: Partial<Publisher>): Promise<Publisher> {
    const response = await axios.post('/publishers/', data);
    return response.data;
  }
}

export default new BookService();
```

---

#### **Frontend: Book List Component**
```typescript
// src/pages/admin/BookListPage.tsx

import React, { useState, useEffect } from 'react';
import { Container, Row, Col, Table, Button, Form, InputGroup } from 'react-bootstrap';
import { Link } from 'react-router-dom';
import bookService from '@/api/bookService';
import { Book } from '@/types/book';

const BookListPage: React.FC = () => {
  const [books, setBooks] = useState<Book[]>([]);
  const [loading, setLoading] = useState(true);
  const [search, setSearch] = useState('');
  const [page, setPage] = useState(1);
  const [totalPages, setTotalPages] = useState(1);

  useEffect(() => {
    fetchBooks();
  }, [page, search]);

  const fetchBooks = async () => {
    setLoading(true);
    try {
      const response = await bookService.getBooks({
        page,
        search,
      });
      setBooks(response.results);
      setTotalPages(Math.ceil(response.count / 20));
    } catch (error) {
      console.error('Error fetching books:', error);
    } finally {
      setLoading(false);
    }
  };

  const handleSearch = (e: React.FormEvent) => {
    e.preventDefault();
    setPage(1);
    fetchBooks();
  };

  const handleDelete = async (id: number) => {
    if (window.confirm('Are you sure you want to delete this book?')) {
      try {
        await bookService.deleteBook(id);
        fetchBooks();
      } catch (error) {
        console.error('Error deleting book:', error);
      }
    }
  };

  return (
    <Container fluid className="py-4">
      <Row className="mb-4">
        <Col>
          <h2>Book Management</h2>
        </Col>
        <Col className="text-end">
          <Link to="/admin/books/create">
            <Button variant="primary">Add New Book</Button>
          </Link>
        </Col>
      </Row>

      <Row className="mb-3">
        <Col md={6}>
          <Form onSubmit={handleSearch}>
            <InputGroup>
              <Form.Control
                type="text"
                placeholder="Search by title, ISBN, author..."
                value={search}
                onChange={(e) => setSearch(e.target.value)}
              />
              <Button type="submit" variant="outline-secondary">
                Search
              </Button>
            </InputGroup>
          </Form>
        </Col>
      </Row>

      {loading ? (
        <div>Loading...</div>
      ) : (
        <>
          <Table striped bordered hover responsive>
            <thead>
              <tr>
                <th>ID</th>
                <th>Cover</th>
                <th>Title</th>
                <th>Author</th>
                <th>Category</th>
                <th>Available</th>
                <th>Status</th>
                <th>Actions</th>
              </tr>
            </thead>
            <tbody>
              {books.map((book) => (
                <tr key={book.id}>
                  <td>{book.id}</td>
                  <td>
                    {book.cover_image_url ? (
                      <img
                        src={book.cover_image_url}
                        alt={book.title}
                        style={{ width: '50px', height: '70px', objectFit: 'cover' }}
                      />
                    ) : (
                      <div
                        style={{
                          width: '50px',
                          height: '70px',
                          backgroundColor: '#ddd',
                          display: 'flex',
                          alignItems: 'center',
                          justifyContent: 'center',
                        }}
                      >
                        No image
                      </div>
                    )}
                  </td>
                  <td>{book.title}</td>
                  <td>{book.author_detail?.name || 'N/A'}</td>
                  <td>{book.category_detail?.name || 'N/A'}</td>
                  <td>
                    {book.available} / {book.quantity}
                  </td>
                  <td>
                    <span
                      className={`badge bg-${
                        book.status === 'ACTIVE' ? 'success' : 'secondary'
                      }`}
                    >
                      {book.status}
                    </span>
                  </td>
                  <td>
                    <Link to={`/admin/books/${book.id}`}>
                      <Button size="sm" variant="info" className="me-1">
                        View
                      </Button>
                    </Link>
                    <Link to={`/admin/books/${book.id}/edit`}>
                      <Button size="sm" variant="warning" className="me-1">
                        Edit
                      </Button>
                    </Link>
                    <Button
                      size="sm"
                      variant="danger"
                      onClick={() => handleDelete(book.id)}
                    >
                      Delete
                    </Button>
                  </td>
                </tr>
              ))}
            </tbody>
          </Table>

          {/* Pagination */}
          <div className="d-flex justify-content-center mt-3">
            <Button
              variant="outline-primary"
              disabled={page === 1}
              onClick={() => setPage(page - 1)}
            >
              Previous
            </Button>
            <span className="mx-3 align-self-center">
              Page {page} of {totalPages}
            </span>
            <Button
              variant="outline-primary"
              disabled={page === totalPages}
              onClick={() => setPage(page + 1)}
            >
              Next
            </Button>
          </div>
        </>
      )}
    </Container>
  );
};

export default BookListPage;
```

**💡 React patterns:**
- **useState**: Component state
- **useEffect**: Side effects (API calls)
- **Conditional rendering**: {loading ? ... : ...}
- **List rendering**: map()
- **Event handlers**: onClick, onChange, onSubmit

---

### 🔄 **Feature 2: Borrowing System**

#### **Backend: Borrowing Models**
```python
# borrowing/models.py

from django.db import models
from django.contrib.auth.models import User
from books.models import Book
from system.models import SystemConfig

class BorrowRecord(models.Model):
    STATUS_CHOICES = [
        ('BORROWED', 'Borrowed'),
        ('RETURNED', 'Returned'),
        ('OVERDUE', 'Overdue'),
    ]
    
    reader = models.ForeignKey(User, on_delete=models.CASCADE, related_name='borrows')
    librarian = models.ForeignKey(User, on_delete=models.SET_NULL, null=True, related_name='processed_borrows')
    borrow_date = models.DateTimeField(auto_now_add=True)
    due_date = models.DateField()
    return_date = models.DateTimeField(null=True, blank=True)
    status = models.CharField(max_length=20, choices=STATUS_CHOICES, default='BORROWED')
    total_fine = models.DecimalField(max_digits=10, decimal_places=2, default=0)
    notes = models.TextField(null=True, blank=True)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    class Meta:
        db_table = 'borrowing_record'
        ordering = ['-created_at']
    
    def __str__(self):
        return f"Borrow #{self.id} - {self.reader.username}"
    
    def calculate_fine(self):
        """Calculate fine for overdue books"""
        from datetime import datetime, date
        
        if self.return_date:
            days_late = (self.return_date.date() - self.due_date).days
        else:
            days_late = (date.today() - self.due_date).days
        
        if days_late > 0:
            fine_per_day = SystemConfig.get_value('FINE_PER_DAY', 5000)
            return days_late * fine_per_day
        return 0


class BorrowDetail(models.Model):
    STATUS_CHOICES = [
        ('BORROWED', 'Borrowed'),
        ('RETURNED', 'Returned'),
        ('OVERDUE', 'Overdue'),
        ('LOST', 'Lost'),
    ]
    
    borrow_record = models.ForeignKey(BorrowRecord, on_delete=models.CASCADE, related_name='details')
    book = models.ForeignKey(Book, on_delete=models.CASCADE)
    due_date = models.DateField()
    return_date = models.DateTimeField(null=True, blank=True)
    extension_count = models.IntegerField(default=0)
    fine_amount = models.DecimalField(max_digits=10, decimal_places=2, default=0)
    status = models.CharField(max_length=20, choices=STATUS_CHOICES, default='BORROWED')
    condition_borrow = models.CharField(max_length=50, null=True, blank=True)
    condition_return = models.CharField(max_length=50, null=True, blank=True)
    notes = models.TextField(null=True, blank=True)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    class Meta:
        db_table = 'borrowing_detail'
    
    def __str__(self):
        return f"{self.borrow_record.id} - {self.book.title}"
    
    def extend(self):
        """Extend due date"""
        from datetime import timedelta
        
        max_extensions = SystemConfig.get_value('MAX_EXTENSION_COUNT', 2)
        
        if self.extension_count >= max_extensions:
            raise ValueError(f"Maximum {max_extensions} extensions allowed")
        
        if self.status != 'BORROWED':
            raise ValueError("Can only extend borrowed books")
        
        # Add extension days
        extension_days = SystemConfig.get_value('BORROW_DURATION_DAYS', 14)
        self.due_date += timedelta(days=extension_days)
        self.extension_count += 1
        self.save()
        
        # Update borrow record due date (latest due date)
        latest_due = self.borrow_record.details.aggregate(
            models.Max('due_date')
        )['due_date__max']
        self.borrow_record.due_date = latest_due
        self.borrow_record.save()
```

**💡 Business logic trong models:**
- `calculate_fine()`: Tính phí phạt
- `extend()`: Gia hạn sách
- Validation logic trong methods

---

#### **Celery Tasks cho Auto-check**
```python
# borrowing/tasks.py

from celery import shared_task
from django.utils import timezone
from datetime import timedelta, date
from .models import BorrowRecord, BorrowDetail, Violation
from notifications.models import Notification
from system.models import SystemConfig
from django.core.mail import send_mail
from django.template.loader import render_to_string

@shared_task
def check_overdue_books():
    """Check and update overdue books status"""
    today = date.today()
    
    # Find overdue details
    overdue_details = BorrowDetail.objects.filter(
        due_date__lt=today,
        status='BORROWED'
    )
    
    for detail in overdue_details:
        # Update status
        detail.status = 'OVERDUE'
        detail.fine_amount = detail.calculate_fine()
        detail.save()
        
        # Update borrow record status
        if detail.borrow_record.status == 'BORROWED':
            detail.borrow_record.status = 'OVERDUE'
            detail.borrow_record.save()
        
        # Create notification
        Notification.objects.create(
            user=detail.borrow_record.reader,
            title='Sách quá hạn',
            message=f'Sách "{detail.book.title}" đã quá hạn. Vui lòng trả sách.',
            notification_type='OVERDUE'
        )
        
        # Send email
        send_overdue_email.delay(detail.id)
    
    return f"Checked {overdue_details.count()} overdue books"


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


@shared_task
def send_overdue_email(detail_id):
    """Send overdue email"""
    try:
        detail = BorrowDetail.objects.get(id=detail_id)
        reader = detail.borrow_record.reader
        
        # Render email template
        html_message = render_to_string('emails/overdue_notice.html', {
            'reader': reader,
            'book': detail.book,
            'due_date': detail.due_date,
            'fine_amount': detail.fine_amount,
        })
        
        send_mail(
            subject='Thông báo sách quá hạn',
            message='',
            from_email='library@example.com',
            recipient_list=[reader.email],
            html_message=html_message,
            fail_silently=False,
        )
        
        return f"Sent email to {reader.email}"
    except Exception as e:
        return f"Error: {str(e)}"
```

---

#### **Cấu hình Celery Beat Schedule**
```python
# library_backend/celery.py

from celery import Celery
from celery.schedules import crontab
import os

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'library_backend.settings')

app = Celery('library_backend')
app.config_from_object('django.conf:settings', namespace='CELERY')
app.autodiscover_tasks()

# Celery Beat Schedule
app.conf.beat_schedule = {
    'check-overdue-books-daily': {
        'task': 'borrowing.tasks.check_overdue_books',
        'schedule': crontab(hour=8, minute=0),  # 8:00 AM daily
    },
    'send-due-soon-reminders-daily': {
        'task': 'borrowing.tasks.send_due_soon_reminders',
        'schedule': crontab(hour=9, minute=0),  # 9:00 AM daily
    },
}
```

**💡 Celery Beat:**
- **Scheduled tasks**: Chạy tự động theo lịch
- **crontab**: Cron-like schedule
- **Celery worker**: Process tasks
- **Celery beat**: Schedule tasks

---

## 7. TESTING VÀ DEBUGGING

### 🧪 **Backend Testing**

#### **Unit Tests**
```python
# books/tests.py

from django.test import TestCase
from django.contrib.auth.models import User
from .models import Book, Category, Author, Publisher

class BookModelTest(TestCase):
    def setUp(self):
        self.author = Author.objects.create(name='Test Author')
        self.publisher = Publisher.objects.create(name='Test Publisher')
        self.category = Category.objects.create(name='Test Category', code='TEST')
    
    def test_book_creation(self):
        """Test creating a book"""
        book = Book.objects.create(
            title='Test Book',
            isbn='123456789',
            author=self.author,
            publisher=self.publisher,
            category=self.category,
            quantity=10
        )
        
        self.assertEqual(book.title, 'Test Book')
        self.assertEqual(book.available, 10)
        self.assertIsNotNone(book.qr_code)
    
    def test_book_availability(self):
        """Test book availability calculation"""
        book = Book.objects.create(
            title='Test Book',
            author=self.author,
            quantity=5
        )
        
        self.assertEqual(book.available, 5)
```

**Run tests:**
```powershell
python manage.py test
```

---

#### **API Testing với Postman**

**Collection structure:**
```
Library API
├── Auth
│   ├── Login
│   ├── Refresh Token
│   └── Get Current User
├── Books
│   ├── List Books
│   ├── Create Book
│   ├── Get Book
│   ├── Update Book
│   ├── Delete Book
│   └── Get QR Code
└── Borrowing
    ├── Create Borrow
    ├── Return Book
    └── Extend Book
```

**Example request:**
```http
POST http://localhost:8000/api/auth/login/
Content-Type: application/json

{
  "username": "admin",
  "password": "admin123"
}

Response:
{
  "access": "eyJ0eXAiOiJKV1QiLCJhbGc...",
  "refresh": "eyJ0eXAiOiJKV1QiLCJhbGc..."
}
```

---

### 🐛 **Common Debugging Techniques**

#### **1. Django Debug Toolbar**
```python
# Install
pip install django-debug-toolbar

# settings.py
INSTALLED_APPS = [
    ...
    'debug_toolbar',
]

MIDDLEWARE = [
    'debug_toolbar.middleware.DebugToolbarMiddleware',
    ...
]

INTERNAL_IPS = ['127.0.0.1']

# urls.py
if settings.DEBUG:
    import debug_toolbar
    urlpatterns += [
        path('__debug__/', include(debug_toolbar.urls)),
    ]
```

**Features:**
- SQL queries analysis
- Request/Response timing
- Templates used
- Cache statistics

---

#### **2. Logging**
```python
# settings.py
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'console': {
            'class': 'logging.StreamHandler',
        },
        'file': {
            'class': 'logging.FileHandler',
            'filename': 'debug.log',
        },
    },
    'loggers': {
        'django': {
            'handlers': ['console', 'file'],
            'level': 'INFO',
        },
        'books': {
            'handlers': ['console', 'file'],
            'level': 'DEBUG',
        },
    },
}

# Usage in views
import logging
logger = logging.getLogger(__name__)

def my_view(request):
    logger.debug('Debug message')
    logger.info('Info message')
    logger.error('Error message')
```

---

#### **3. React DevTools**
- Install browser extension: React Developer Tools
- Inspect component props, state
- View component tree
- Profile performance

#### **4. Redux DevTools**
- Install browser extension: Redux DevTools
- View actions dispatched
- Time-travel debugging
- State diff

---

## 8. DEPLOYMENT

### 🚀 **Production Deployment Steps**

#### **1. Prepare Server (Ubuntu 22.04)**
```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Python
sudo apt install python3.11 python3.11-venv python3-pip -y

# Install MySQL
sudo apt install mysql-server -y
sudo mysql_secure_installation

# Install Redis
sudo apt install redis-server -y

# Install Nginx
sudo apt install nginx -y

# Install Supervisor
sudo apt install supervisor -y

# Install certbot (SSL)
sudo apt install certbot python3-certbot-nginx -y
```

---

#### **2. Deploy Backend**
```bash
# Clone repository
cd /var/www
sudo git clone <your-repo-url> library-backend
cd library-backend

# Create virtual environment
python3 -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Configure .env
nano .env
# Set production values

# Run migrations
python manage.py migrate

# Collect static files
python manage.py collectstatic --noinput

# Create superuser
python manage.py createsuperuser
```

---

#### **3. Configure Gunicorn**
```bash
# Create systemd service
sudo nano /etc/systemd/system/gunicorn.service
```

```ini
[Unit]
Description=Gunicorn for Library Backend
After=network.target

[Service]
User=www-data
Group=www-data
WorkingDirectory=/var/www/library-backend
Environment="PATH=/var/www/library-backend/venv/bin"
ExecStart=/var/www/library-backend/venv/bin/gunicorn \
    --workers 3 \
    --bind 127.0.0.1:8000 \
    library_backend.wsgi:application

[Install]
WantedBy=multi-user.target
```

```bash
# Start Gunicorn
sudo systemctl start gunicorn
sudo systemctl enable gunicorn
```

---

#### **4. Configure Nginx**
```bash
sudo nano /etc/nginx/sites-available/library
```

```nginx
server {
    listen 80;
    server_name yourdomain.com;

    client_max_body_size 20M;

    # Frontend
    location / {
        root /var/www/library-frontend/build;
        try_files $uri $uri/ /index.html;
    }

    # Backend API
    location /api {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # Django Admin
    location /admin {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
    }

    # Static files
    location /static {
        alias /var/www/library-backend/staticfiles;
    }

    # Media files
    location /media {
        alias /var/www/library-backend/media;
    }
}
```

```bash
# Enable site
sudo ln -s /etc/nginx/sites-available/library /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

---

#### **5. Install SSL Certificate**
```bash
sudo certbot --nginx -d yourdomain.com
```

---

#### **6. Deploy Frontend**
```bash
# Build React app
cd /var/www/library-frontend
npm install
npm run build

# Build artifacts in build/ folder
# Nginx will serve these files
```

---

#### **7. Configure Celery**
```bash
# Create Celery worker service
sudo nano /etc/supervisor/conf.d/celery-worker.conf
```

```ini
[program:celery-worker]
command=/var/www/library-backend/venv/bin/celery -A library_backend worker -l info
directory=/var/www/library-backend
user=www-data
autostart=true
autorestart=true
stdout_logfile=/var/log/celery/worker.log
stderr_logfile=/var/log/celery/worker_err.log
```

```bash
# Create Celery beat service
sudo nano /etc/supervisor/conf.d/celery-beat.conf
```

```ini
[program:celery-beat]
command=/var/www/library-backend/venv/bin/celery -A library_backend beat -l info
directory=/var/www/library-backend
user=www-data
autostart=true
autorestart=true
stdout_logfile=/var/log/celery/beat.log
stderr_logfile=/var/log/celery/beat_err.log
```

```bash
# Create log directory
sudo mkdir /var/log/celery

# Start services
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start celery-worker
sudo supervisorctl start celery-beat
```

---

### ✅ **Post-Deployment Checklist**

- [ ] All services running (Gunicorn, Nginx, Celery)
- [ ] SSL certificate installed
- [ ] Database migrated
- [ ] Static files collected
- [ ] Media files accessible
- [ ] Email sending works
- [ ] Celery tasks running
- [ ] Backup scheduled
- [ ] Monitoring setup (Sentry)
- [ ] Domain points to server
- [ ] All features tested on production

---

## 🎓 TỔNG KẾT

Bạn đã học được:

### Backend (Django):
✅ Models, Serializers, ViewSets  
✅ Django REST Framework  
✅ JWT Authentication  
✅ Celery for background tasks  
✅ Database migrations  
✅ File uploads & QR codes  

### Frontend (React):
✅ Components, Props, State  
✅ React Router  
✅ Redux state management  
✅ Axios HTTP client  
✅ TypeScript  
✅ Form handling  

### DevOps:
✅ Nginx configuration  
✅ Gunicorn WSGI server  
✅ SSL certificates  
✅ Supervisor for process management  
✅ Production deployment  

### Best Practices:
✅ Separation of concerns  
✅ RESTful API design  
✅ Error handling  
✅ Security practices  
✅ Testing  
✅ Documentation  

---

**🚀 Chúc bạn thành công với dự án!**
