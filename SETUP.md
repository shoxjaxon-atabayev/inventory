# Setup Guide - Inventory Management System

Complete step-by-step guide to set up and run the Inventory Management System.

## Prerequisites

- Python 3.13 or higher
- pip or uv package manager
- Git
- A text editor or IDE (VS Code, PyCharm, etc.)

## Initial Setup

### 1. Clone the Repository

```bash
git clone <repository-url>
cd inventory
```

### 2. Create Virtual Environment

**Using Python venv:**
```bash
python -m venv .venv
source .venv/bin/activate    # On Windows: .venv\Scripts\activate
```

**Using uv (optional):**
```bash
uv venv
source .venv/bin/activate
```

### 3. Install Dependencies

**Using requirements.txt:**
```bash
pip install -r requirements.txt
```

**Using pyproject.toml:**
```bash
pip install -e .
# OR with uv
uv pip install -r requirements.txt
```

### 4. Database Setup

**Apply migrations:**
```bash
python manage.py migrate
```

**Output should show:**
```
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions, users, store
...
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  ...
```

### 5. Create Admin User

```bash
python manage.py createsuperuser
```

**Fill in the prompts:**
```
Username: admin
Email: admin@example.com
Password: (enter password)
Password (again): (confirm)
Position (lavozimi): Administrator
Superuser created successfully.
```

### 6. Run Development Server

```bash
python manage.py runserver
```

**Expected output:**
```
Django version 6.0.6, using settings 'inventory.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

### 7. Access the Application

Open your browser and navigate to:
- **Main App:** http://localhost:8000
- **Admin Panel:** http://localhost:8000/admin

Login with the credentials created in step 5.

## First Steps

### 1. Add Categories

1. Go to Admin panel: http://localhost:8000/admin
2. Click **Categories** (Kategoriyalar)
3. Click **Add Category**
4. Enter category name (e.g., "Electronics", "Food", "Clothing")
5. Save

### 2. Add Suppliers

1. Go to Dashboard: http://localhost:8000
2. Go to **Suppliers** → **Create New**
3. Fill in:
   - Full Name (F.I.O)
   - Tax ID (INN)
   - Address
4. Save

### 3. Add Products

1. Go to Dashboard: http://localhost:8000
2. Go to **Products** → **Create New**
3. Fill in:
   - Product Name
   - Unit Price
   - Category (select from dropdown)
   - Unit of Measurement (Gram, Kilogram, Piece, Box, Liter)
4. Save

### 4. Record Incoming Goods

1. Go to Dashboard: http://localhost:8000
2. Go to **Purchases** → **Create New**
3. Fill in:
   - Supplier
   - Product
   - Quantity
   - Purchase Price
   - Delivery Date
4. Save (inventory automatically updates)

### 5. Record Sales

1. Go to Dashboard: http://localhost:8000
2. Go to **Sales** → **Create New**
3. Fill in:
   - Product
   - Quantity (must be ≤ available stock)
   - Customer Name
   - Customer Phone
   - Sale Date
4. Save (inventory automatically updates)

## Common Tasks

### Export Data to Excel

1. Go to any list page (Products, Suppliers, Sales, Purchases)
2. Click **Download Excel** button
3. File downloads automatically

### View Inventory

Dashboard shows:
- Total items in stock
- Number of purchase records
- Number of sales records
- Number of suppliers

### Edit/Delete Records

1. Go to the list page (e.g., Products)
2. Click **Edit** or **Delete** next to the item
3. Confirm changes

### Logout

Click **Logout** button in the top-right corner (on dashboard)

## Troubleshooting

### Issue: "No module named 'django'"

**Solution:**
```bash
pip install django==6.0.6
# OR install all dependencies
pip install -r requirements.txt
```

### Issue: "No such table" error

**Solution:**
```bash
python manage.py migrate
```

### Issue: Can't access dashboard after login

**Make sure:**
1. You're logged in (check browser cookies)
2. Database migrations are applied: `python manage.py migrate`
3. No errors in console

### Issue: Static files not loading (CSS/styling missing)

**For development:**
Django handles static files automatically. Try:
```bash
python manage.py collectstatic --noinput
```

### Issue: Excel export not working

**Check:**
1. openpyxl is installed: `pip install openpyxl`
2. Download folder has write permissions
3. Try exporting from a different list page

### Issue: Forgot admin password

**Reset password:**
```bash
python manage.py changepassword admin
```

### Issue: Port 8000 already in use

**Use different port:**
```bash
python manage.py runserver 8080
```

Then access at: http://localhost:8080

## Advanced Setup

### Change Database to PostgreSQL

1. Install PostgreSQL driver:
```bash
pip install psycopg2-binary
```

2. Update settings.py:
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'inventory_db',
        'USER': 'postgres',
        'PASSWORD': 'your_password',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
```

3. Apply migrations:
```bash
python manage.py migrate
```

### Change Timezone

Edit `inventory/settings.py`:
```python
TIME_ZONE = 'Asia/Tashkent'  # Change to your timezone
```

### Production Deployment

For production environment:

1. Set `DEBUG = False` in settings.py
2. Generate new SECRET_KEY (use Django utility)
3. Set ALLOWED_HOSTS to your domain
4. Use PostgreSQL/MySQL instead of SQLite
5. Set up HTTPS/SSL certificates
6. Use environment variables for sensitive data
7. Collect static files: `python manage.py collectstatic`
8. Use production server (Gunicorn, uWSGI, etc.)

Example production settings:
```bash
DEBUG = False
ALLOWED_HOSTS = ['yourdomain.com', 'www.yourdomain.com']
SECRET_KEY = 'your-secret-key-here'
SECURE_SSL_REDIRECT = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
```

## Development Commands

### Create app migrations
```bash
python manage.py makemigrations
```

### Apply migrations
```bash
python manage.py migrate
```

### Run tests
```bash
python manage.py test
```

### Check code with flake8
```bash
flake8 .
```

### Access Django shell
```bash
python manage.py shell
```

### Create superuser
```bash
python manage.py createsuperuser
```

### Change user password
```bash
python manage.py changepassword username
```

### Check project settings
```bash
python manage.py check
```

## File Structure After Setup

```
inventory/
├── .venv/                  # Virtual environment
├── db.sqlite3              # Database file
├── manage.py
├── CLAUDE.md              # Developer docs
├── README.md              # Project readme
├── SETUP.md               # This file
├── requirements.txt       # Dependencies
├── .gitignore             # Git ignore rules
├── pyproject.toml
├── inventory/             # Project settings
├── store/                 # Main app
├── users/                 # Auth app
└── templates/             # HTML templates
```

## Next Steps

1. **Familiarize yourself with the UI** - Click around the dashboard
2. **Read CLAUDE.md** - Understand the project architecture
3. **Add test data** - Create some sample products and suppliers
4. **Review models** - Look at `store/models.py` to understand data structure
5. **Explore admin panel** - Manage users and permissions

## Support & Documentation

- **Django Documentation:** https://docs.djangoproject.com/
- **Developer Notes:** See `CLAUDE.md`
- **README:** See `README.md`

## Quick Reference

| Task | Command |
|------|---------|
| Start server | `python manage.py runserver` |
| Create user | `python manage.py createsuperuser` |
| Change password | `python manage.py changepassword username` |
| Database migrations | `python manage.py migrate` |
| Create migrations | `python manage.py makemigrations` |
| Access shell | `python manage.py shell` |
| Run tests | `python manage.py test` |
| Check code | `flake8 .` |
| Collect static files | `python manage.py collectstatic` |

Happy coding! 🚀
