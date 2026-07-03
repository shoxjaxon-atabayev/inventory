# Inventory Management System

A comprehensive Django-based inventory management system for warehouse and retail operations. Designed to track products, suppliers, incoming goods (purchases), and outgoing goods (sales).

## Features

✅ **Product Management** - Create and manage products with categories and pricing  
✅ **Supplier Management** - Track suppliers and their contact information  
✅ **Inventory Tracking** - Real-time warehouse stock levels and total values  
✅ **Purchase Orders** - Record incoming goods from suppliers  
✅ **Sales Records** - Track outgoing goods and customer information  
✅ **User Authentication** - Secure login system with custom user roles  
✅ **Excel Export** - Generate reports in XLSX format for all modules  
✅ **Dashboard** - Overview of key metrics and statistics  

## Technology Stack

- **Backend:** Django 6.0.6 (Python 3.13+)
- **Database:** SQLite3 (easily configurable to PostgreSQL/MySQL)
- **Package Manager:** uv
- **Excel Generation:** openpyxl
- **Authentication:** Django's built-in auth with custom User model

## Project Structure

```
inventory/
├── inventory/                 # Django project config
│   ├── settings.py           # Configuration
│   ├── urls.py               # Main routing
│   ├── views.py              # Dashboard logic
│   ├── wsgi.py               # WSGI config
│   └── asgi.py               # ASGI config
├── store/                     # Main inventory app
│   ├── models.py             # Database models
│   ├── views.py              # CRUD operations
│   ├── forms.py              # Form definitions
│   ├── urls.py               # Store routes
│   └── admin.py              # Admin interface
├── users/                     # Authentication app
│   ├── models.py             # Custom User model
│   ├── views.py              # Login/logout
│   ├── urls.py               # Auth routes
│   └── forms.py              # Auth forms
├── templates/                # HTML templates
│   ├── base/                 # Layout templates
│   ├── store/                # Store module
│   ├── edit/                 # Edit forms
│   ├── users/                # Login pages
│   └── dashboard.html        # Main dashboard
├── manage.py                 # Django CLI
├── pyproject.toml            # Dependencies
└── CLAUDE.md                 # Developer documentation
```

## Database Models

### Core Models

**Kategoriyalar** (Categories)
- Product categories for organization

**Mahsulotlar** (Products)
- Product details: name, price, category, unit of measurement

**Yetkazib_beruvchilar** (Suppliers)
- Supplier information: name, tax ID, address

**Kirimlar** (Incoming Goods)
- Purchase records from suppliers
- Links: product, supplier, quantity, date, price

**Chiqimlar** (Outgoing Goods)
- Sales records to customers
- Links: product, customer name, phone, quantity, date

**Omborxona** (Warehouse Inventory)
- Current stock levels and total values
- Automatically updated when goods arrive or leave

**User** (Custom User Model)
- Extends Django's AbstractUser
- Includes job position (lavozimi)

## Installation

### Prerequisites
- Python 3.13+
- pip or uv package manager

### Setup

1. **Clone and navigate to project:**
```bash
cd inventory
```

2. **Install dependencies:**
```bash
uv pip install -r requirements.txt
# OR
pip install -r requirements.txt
```

3. **Create virtual environment (if needed):**
```bash
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
```

4. **Apply migrations:**
```bash
python manage.py migrate
```

5. **Create superuser (admin account):**
```bash
python manage.py createsuperuser
```

6. **Run development server:**
```bash
python manage.py runserver
```

7. **Access the application:**
- Main app: http://localhost:8000
- Admin panel: http://localhost:8000/admin

## Usage Guide

### Dashboard
- Main landing page with inventory overview
- Shows total quantity in stock, incoming/outgoing records count, supplier count
- Requires login

### Product Management (`/store/mahsulotlar/`)
- Create new products with name, price, category, and unit
- View all products in list format
- Edit existing product details
- Delete products
- Export products to Excel

### Supplier Management (`/store/yetkazib-beruvchilar/`)
- Add new suppliers with name, tax ID, and address
- View supplier list
- Edit supplier information
- Delete suppliers
- Export suppliers to Excel

### Incoming Goods (`/store/tushumlar/`)
- Record purchases from suppliers
- Select supplier, product, quantity, price, and delivery date
- Automatically updates warehouse inventory
- View history of incoming goods
- Export to Excel

### Outgoing Goods (`/store/chiqimlar/`)
- Record sales/distributions
- Select product, customer name, phone, quantity, and sale date
- System validates available stock
- Automatically updates warehouse inventory
- View history of sales
- Export to Excel

### Authentication
- Login: `/users/login/`
- Logout: `/users/logout/`
- User positions (lavozimi) stored in database

## API Routes

### Authentication
```
POST   /users/login/          Login
GET    /users/logout/         Logout
```

### Products
```
GET    /store/mahsulotlar/           List all products
POST   /store/mahsulotlar/create/    Create new product
GET    /store/mahsulotlar/edit/<id>/ Edit product
POST   /store/mahsulotlar/edit/<id>/ Save changes
GET    /store/mahsulotlar/delete/<id>/ Delete product
GET    /store/download/mahsulotlar/  Export to Excel
```

### Suppliers
```
GET    /store/yetkazib-beruvchilar/           List suppliers
POST   /store/yetkazib-beruvchi/create/       Create supplier
GET    /store/yetkazib-beruvchi/edit/<id>/    Edit supplier
POST   /store/yetkazib-beruvchi/edit/<id>/    Save changes
GET    /store/yetkazib-beruvchi/delete/<id>/  Delete supplier
GET    /store/download/yetkazib_beruvchilar/  Export to Excel
```

### Incoming Goods (Purchases)
```
GET    /store/tushumlar/          List purchases
POST   /store/tushumlar/create/   Create purchase
GET    /store/kirimlar/download/  Export to Excel
```

### Outgoing Goods (Sales)
```
GET    /store/chiqimlar/          List sales
POST   /store/chiqimlar/create/   Create sale
GET    /store/download/chiqimlar/ Export to Excel
```

### Dashboard
```
GET    /                 Dashboard (login required)
```

## Configuration

### settings.py Key Settings

```python
# Authentication
AUTH_USER_MODEL = 'users.User'
LOGIN_URL = 'login'

# Database (default: SQLite)
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

# Timezone (Tashkent)
TIME_ZONE = 'Asia/Tashkent'

# Installed apps
INSTALLED_APPS = [
    ...
    'users',
    'store',
]
```

## Troubleshooting

### Database errors
```bash
python manage.py migrate
python manage.py migrate --run-syncdb
```

### Missing tables
```bash
python manage.py makemigrations
python manage.py migrate
```

### Static files not loading
```bash
python manage.py collectstatic
```

### User login issues
```bash
python manage.py createsuperuser
```

## Performance Notes

- Inventory updates are calculated on-the-fly from Kirimlar and Chiqimlar records
- For large datasets, consider adding database indexes
- Excel exports work well with <10,000 records

## Security Notes

⚠️ **Development Only**: This setup is configured for development mode with DEBUG=True

For production deployment:
1. Set `DEBUG = False` in settings.py
2. Change `SECRET_KEY` to a secure random value
3. Set `ALLOWED_HOSTS` to your domain
4. Use PostgreSQL or MySQL instead of SQLite
5. Set up HTTPS/SSL
6. Use environment variables for sensitive data
7. Run `python manage.py collectstatic`

## Known Issues & Future Improvements

### Current Limitations
- Product lookup uses string parsing (fragile approach)
- No transaction handling for concurrent inventory updates
- No backup/restore functionality
- Limited reporting capabilities

### Planned Features
- Advanced analytics and reporting
- Multi-warehouse support
- Role-based access control (RBAC)
- API for mobile app
- Email notifications
- Barcode/QR code support
- Batch operations

## Development

### Running Tests
```bash
python manage.py test
```

### Code Quality
```bash
flake8 .
```

### Creating Superuser
```bash
python manage.py createsuperuser
```

## Contributing

Contributions welcome! Please follow Django best practices:
- Use meaningful commit messages
- Write docstrings for functions
- Follow PEP 8 style guide
- Test changes locally before committing

## License

This project is open source. Please check LICENSE file for details.

## Support

For issues and questions:
1. Check CLAUDE.md for developer documentation
2. Review Django documentation: https://docs.djangoproject.com/
3. Check database models in `store/models.py` for field details

## Contact

For more information, please contact the development team.
