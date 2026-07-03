# Inventory Management System

A Django-based inventory management system for tracking products, suppliers, incoming goods (kirimlar), and outgoing goods (chiqimlar) in a warehouse (omborxona).

## Project Overview

**Type:** Django Web Application  
**Language:** Python 3.13+  
**Framework:** Django 6.0.6  
**Package Manager:** uv  
**Database:** SQLite3 (default)

### Key Features

- **Product Management** (Mahsulotlar): Create, view, edit, and delete products with categories, prices, and units
- **Supplier Management** (Yetkazib beruvchilar): Manage suppliers/vendors with contact information
- **Incoming Goods** (Kirimlar): Track product receipts from suppliers
- **Outgoing Goods** (Chiqimlar): Track product sales/distribution
- **Warehouse Inventory** (Omborxona): Real-time inventory tracking with quantity and total value
- **User Authentication**: Login/logout with custom User model
- **Excel Exports**: Generate XLSX reports for all modules
- **Dashboard**: Overview of inventory statistics and metrics

## Project Structure

```
inventory/
├── inventory/              # Main Django project config
│   ├── settings.py        # Django settings
│   ├── urls.py            # Main URL routing
│   ├── views.py           # Dashboard view
│   ├── wsgi.py            # WSGI application
│   └── asgi.py            # ASGI application
├── store/                  # Main app for inventory management
│   ├── models.py          # Data models
│   ├── views.py           # CRUD views for all modules
│   ├── forms.py           # Django forms
│   ├── urls.py            # Store app URL routing
│   ├── admin.py           # Django admin configuration
│   └── migrations/        # Database migrations
├── users/                  # Authentication app
│   ├── models.py          # Custom User model
│   ├── views.py           # Login/logout views
│   ├── urls.py            # Auth URL routing
│   └── forms.py           # Auth forms
├── templates/             # HTML templates
│   ├── base/              # Base templates (navbar, sidebar, footer)
│   ├── store/             # Store module templates
│   ├── edit/              # Edit templates
│   ├── users/             # Login templates
│   └── dashboard.html     # Main dashboard
├── manage.py              # Django management script
├── pyproject.toml         # Project dependencies
└── README.md              # Project documentation
```

## Data Models

### Kategoriyalar (Categories)
- `nomi` (str, unique): Category name

### Yetkazib_beruvchilar (Suppliers)
- `FISH` (str): Supplier name
- `INN` (str): Tax ID
- `address` (str): Address
- `created_date` (date): Creation date

### Mahsulotlar (Products)
- `nomi` (str, unique): Product name
- `narxi` (decimal): Unit price
- `kategoriya` (FK): Foreign key to Categories
- `ulchov_birligi` (str): Unit of measurement (Грамм, Килограмм, Штука, Коробка, Литр)
- `created_date` (date): Creation date

### Kirimlar (Incoming Goods)
- `yetkazib_beruvchi` (FK): Supplier
- `mahsulot` (FK): Product
- `soni` (int): Quantity received
- `keltirilgan_sana` (date): Delivery date
- `narx` (decimal): Unit purchase price

### Chiqimlar (Outgoing Goods)
- `mahsulot` (FK): Product
- `miqdori` (int): Quantity sold
- `ism` (str): Customer first name
- `familiya` (str): Customer last name
- `telefon` (str): Customer phone
- `sotilgan_sana` (date): Sale date
- `created_date` (date): Creation date

### Omborxona (Warehouse Inventory)
- `mahsulot` (FK, unique): Product
- `umumiy_soni` (int): Total quantity in stock
- `umumiy_narx` (decimal): Total inventory value

### User (Custom User Model)
- Extends Django's AbstractUser
- `lavozimi` (str): Job position/title

## URL Routes

### Main Routes
- `/` - Dashboard (login required)

### Store Routes (`/store/`)
- **Products:**
  - `mahsulotlar/create/` - Create product
  - `mahsulotlar/` - List products
  - `mahsulotlar/edit/<id>/` - Edit product
  - `mahsulotlar/delete/<id>` - Delete product
  - `download/mahsulotlar/` - Export to Excel

- **Suppliers:**
  - `yetkazib-beruvchi/create/` - Create supplier
  - `yetkazib-beruvchilar/` - List suppliers
  - `yetkazib-beruvchi/edit/<id>/` - Edit supplier
  - `yetkazib-beruvchi/delete/<id>` - Delete supplier
  - `download/yetkazib_beruvchilar/` - Export to Excel

- **Incoming Goods:**
  - `tushumlar/create/` - Create incoming record
  - `tushumlar/` - List incoming goods
  - `kirimlar/download/` - Export to Excel

- **Outgoing Goods:**
  - `chiqimlar/create/` - Create outgoing record
  - `chiqimlar/` - List outgoing goods
  - `download/chiqimlar/` - Export to Excel

### Auth Routes (`/users/`)
- `login/` - User login
- `logout/` - User logout

## Key Views

### Dashboard (inventory/views.py:6)
- Displays total inventory quantity
- Shows count of incoming goods records
- Shows count of outgoing goods records
- Shows count of suppliers
- Lists all warehouse items

### Inventory Operations
- **Kirim (Incoming):** Updates Omborxona when product received
- **Chiqim (Outgoing):** Validates stock availability, updates Omborxona when product sold

## Dependencies

```
django            # Web framework
asgiref          # ASGI support
openpyxl         # Excel file generation
sqlparse         # SQL parsing
flake8           # Code linting
```

## Setup & Running

```bash
# Install dependencies
uv pip install -r requirements.txt

# Apply migrations
python manage.py migrate

# Create superuser
python manage.py createsuperuser

# Run development server
python manage.py runserver

# Access at http://localhost:8000
```

## Notes

- Language mix: UI uses Uzbek/Russian terms, code comments minimal
- Authentication required for all views except login
- Excel exports use openpyxl with bold headers
- Inventory calculated dynamically from Kirimlar (incoming) and Chiqimlar (outgoing)
- Product lookup by name string parsing (fragile - consider refactoring to direct ID usage)
- No transaction handling for concurrent inventory updates
