# Contributing to Inventory Management System

Thank you for considering contributing to this project! This guide will help you get started.

## Getting Started

### 1. Fork and Clone

```bash
# Clone your fork
git clone https://github.com/your-username/inventory.git
cd inventory

# Add upstream remote
git remote add upstream https://github.com/original-owner/inventory.git
```

### 2. Create Feature Branch

```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/bug-name
```

### 3. Set Up Development Environment

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
```

## Development Workflow

### Making Changes

1. **Follow Django conventions:**
   - Use meaningful variable names
   - Follow PEP 8 style guide
   - Add docstrings to functions/classes

2. **File Organization:**
   - Models in `models.py`
   - Views in `views.py`
   - Forms in `forms.py`
   - URLs in `urls.py`
   - Templates in `templates/`

3. **Code Quality:**
   ```bash
   flake8 .  # Check code style
   python manage.py test  # Run tests
   ```

### Making Commits

**Commit Message Format:**
```
[TYPE] Brief description

Longer description explaining the change if needed.

Fixes: #issue-number
```

**Types:**
- `feat:` New feature
- `fix:` Bug fix
- `docs:` Documentation
- `style:` Code style changes
- `refactor:` Refactoring
- `test:` Adding/updating tests
- `chore:` Build, deps, etc.

**Example:**
```
feat: Add bulk import from Excel

- Parse Excel files with product list
- Validate data before import
- Show import summary with errors

Fixes: #42
```

### Push and Create Pull Request

```bash
git push origin feature/your-feature-name
```

Then create a PR on GitHub with:
- Clear title
- Description of changes
- Related issues
- Test instructions

## Code Style Guidelines

### Python Code

```python
# Good
def create_inventory_record(product_id, quantity, supplier):
    """Create new inventory record for product."""
    record = Kirimlar.objects.create(
        mahsulot_id=product_id,
        soni=quantity,
        yetkazib_beruvchi=supplier
    )
    return record

# Bad
def f(p, q, s):
    r = Kirimlar.objects.create(mahsulot_id=p, soni=q, yetkazib_beruvchi=s)
    return r
```

### Models

```python
class Mahsulotlar(models.Model):
    """Product model for inventory system."""
    nomi = models.CharField(
        max_length=120,
        unique=True,
        help_text="Product name must be unique"
    )
    narxi = models.DecimalField(
        max_digits=10,
        decimal_places=2,
        help_text="Unit price in currency"
    )
    
    class Meta:
        verbose_name = "Product"
        verbose_name_plural = "Products"
        ordering = ['-created_date']
    
    def __str__(self):
        return self.nomi
```

### Views

```python
@login_required(login_url='login')
def create_product(request):
    """Create new product."""
    if request.method == 'POST':
        form = MahsulotlarForm(request.POST)
        if form.is_valid():
            product = form.save()
            return redirect('mahsulot-list')
    else:
        form = MahsulotlarForm()
    
    return render(request, 'store/create_mahsulot.html', {'form': form})
```

### Templates

```html
{% extends "base/base.html" %}

{% block title %}Products{% endblock %}

{% block content %}
<div class="container">
    <h1>Products</h1>
    
    {% for product in products %}
    <div class="card">
        <div class="card-body">
            <h5 class="card-title">{{ product.nomi }}</h5>
            <p class="card-text">Price: {{ product.narxi }}</p>
        </div>
    </div>
    {% endfor %}
</div>
{% endblock %}
```

## Testing

### Writing Tests

```python
# store/tests.py
from django.test import TestCase
from .models import Mahsulotlar, Kategoriyalar

class MahsulotlarTestCase(TestCase):
    def setUp(self):
        self.category = Kategoriyalar.objects.create(nomi="Electronics")
        self.product = Mahsulotlar.objects.create(
            nomi="Phone",
            narxi=100.00,
            kategoriya=self.category,
            ulchov_birligi="Штука"
        )
    
    def test_product_str(self):
        """Test product string representation."""
        self.assertEqual(str(self.product), "Phone - Штука")
    
    def test_product_price(self):
        """Test product price."""
        self.assertEqual(self.product.narxi, 100.00)
```

### Run Tests

```bash
python manage.py test                    # Run all tests
python manage.py test store              # Run store app tests
python manage.py test store.tests.MahsulotlarTestCase  # Run specific test
python manage.py test --verbosity=2     # Verbose output
```

## Documentation

### Update CLAUDE.md
- Update architecture if you change models or structure
- Add new features to overview section
- Update file structure if adding new directories

### Update README.md
- Add new features to features list
- Update routes if adding new endpoints
- Add troubleshooting tips if you find solutions

### Docstrings
```python
def process_inventory(product_id, quantity):
    """
    Process inventory transaction for product.
    
    Args:
        product_id (int): Product identifier
        quantity (int): Quantity to process
    
    Returns:
        Omborxona: Updated warehouse inventory object
    
    Raises:
        Mahsulotlar.DoesNotExist: If product not found
        ValueError: If quantity is negative
    """
    pass
```

## Pull Request Checklist

Before submitting a PR, ensure:

- [ ] Code follows PEP 8 style guide
- [ ] All tests pass: `python manage.py test`
- [ ] Code quality checked: `flake8 .`
- [ ] No console errors or warnings
- [ ] Database migrations created if models changed
- [ ] Documentation updated (CLAUDE.md, README.md)
- [ ] Docstrings added for new functions
- [ ] Commit messages are clear and descriptive
- [ ] No hardcoded values or credentials
- [ ] Feature works in browser (not just tests)

## Common Contributions

### Adding New Feature

1. Create new app or extend existing one
2. Add models in `models.py`
3. Register in `admin.py`
4. Create views in `views.py`
5. Create forms in `forms.py`
6. Add templates in `templates/`
7. Add URLs in `urls.py`
8. Add tests in `tests.py`
9. Update documentation

### Fixing Bug

1. Create test that reproduces bug
2. Fix the code
3. Verify test passes
4. Update CLAUDE.md if needed
5. Create PR with clear description

### Improving Documentation

1. Update relevant markdown files
2. Check for broken links
3. Verify code examples work
4. Test on different browsers/devices

## Ask For Help

- Open an issue for questions
- Check existing issues first
- Provide clear reproduction steps
- Include error messages and logs
- Mention your Python/Django versions

## Code Review Process

1. Maintainer reviews your PR
2. May request changes
3. You update code and push
4. Once approved, PR is merged
5. Your contribution is live!

## Reporting Issues

**Bug Report Template:**
```markdown
## Description
Brief description of the bug

## Steps to Reproduce
1. Step one
2. Step two
3. Bug occurs

## Expected Behavior
What should happen

## Actual Behavior
What actually happens

## Environment
- Python version: 3.13
- Django version: 6.0.6
- Database: SQLite3
- Browser: Chrome

## Screenshots/Logs
(if applicable)
```

**Feature Request Template:**
```markdown
## Description
What feature would you like?

## Use Case
Why do you need this?

## Proposed Solution
How should it work?

## Alternatives
Other solutions you considered
```

## Release Process

1. Increment version in `pyproject.toml`
2. Update `CHANGELOG.md`
3. Create git tag: `git tag v1.0.0`
4. Push: `git push origin v1.0.0`
5. Create GitHub release with notes

## Questions?

- Read CLAUDE.md for architecture
- Read README.md for usage
- Check Django documentation
- Search existing issues
- Ask in an issue

## License

By contributing, you agree that your contributions will be licensed under the same license as the project.

Thank you for contributing! 🙏
