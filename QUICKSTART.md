# Quick Start Guide

Get the Inventory Management System running in 5 minutes.

## Installation (5 minutes)

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Create database
python manage.py migrate

# 3. Create admin user
python manage.py createsuperuser
# Follow prompts...

# 4. Start server
python manage.py runserver

# 5. Open browser
# http://localhost:8000
```

## First Use (5 minutes)

### 1. Login
- Go to http://localhost:8000
- Login with credentials created in step 3

### 2. Add Category
- Admin: http://localhost:8000/admin
- Click **Categories** → **Add Category**
- Enter: "Electronics"

### 3. Add Supplier
- Dashboard → **Suppliers** → **Create New**
- Fill: Name, Tax ID, Address

### 4. Add Product
- Dashboard → **Products** → **Create New**
- Fill: Name, Price, Category, Unit

### 5. Record Purchase
- Dashboard → **Purchases** → **Create New**
- Fill: Supplier, Product, Quantity, Price, Date

### 6. Record Sale
- Dashboard → **Sales** → **Create New**
- Fill: Product, Quantity, Customer, Phone, Date

### 7. Export Report
- Go to any list page
- Click **Download Excel**

## Common Commands

```bash
# Start development server
python manage.py runserver

# Create superuser
python manage.py createsuperuser

# Apply migrations
python manage.py migrate

# Make migrations
python manage.py makemigrations

# Access Django shell
python manage.py shell

# Run tests
python manage.py test

# Check code quality
flake8 .
```

## URLs

| What | URL |
|------|-----|
| Dashboard | http://localhost:8000 |
| Admin | http://localhost:8000/admin |
| Products | http://localhost:8000/store/mahsulotlar/ |
| Suppliers | http://localhost:8000/store/yetkazib-beruvchilar/ |
| Purchases | http://localhost:8000/store/tushumlar/ |
| Sales | http://localhost:8000/store/chiqimlar/ |

## Troubleshooting

### "Port already in use"
```bash
python manage.py runserver 8080
```

### "No such table"
```bash
python manage.py migrate
```

### "Module not found"
```bash
pip install -r requirements.txt
```

### "Can't login"
```bash
python manage.py createsuperuser
```

### "Forgot password"
```bash
python manage.py changepassword username
```

## Project Structure

```
inventory/
├── README.md              ← Full documentation
├── SETUP.md              ← Detailed setup
├── CONTRIBUTING.md       ← Development guide
├── CLAUDE.md             ← Architecture
├── settings.py           ← Django config
├── store/                ← Main app
├── users/                ← Auth
└── templates/            ← HTML files
```

## Key Features

✅ Inventory tracking  
✅ Product management  
✅ Supplier management  
✅ Purchase records  
✅ Sales records  
✅ Excel export  
✅ User authentication  
✅ Dashboard  

## Next Steps

1. **Read full docs:** [README.md](README.md)
2. **Setup guide:** [SETUP.md](SETUP.md)
3. **Development:** [CONTRIBUTING.md](CONTRIBUTING.md)
4. **Architecture:** [CLAUDE.md](CLAUDE.md)

## Quick Help

- **Installation stuck?** → See [SETUP.md](SETUP.md)
- **Want to develop?** → See [CONTRIBUTING.md](CONTRIBUTING.md)
- **Need details?** → See [README.md](README.md)
- **Want architecture?** → See [CLAUDE.md](CLAUDE.md)

**You're all set!** 🚀

Access the app at http://localhost:8000
