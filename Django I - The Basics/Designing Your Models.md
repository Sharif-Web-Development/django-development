## Designing Your Models

Designing your models is one of the most important steps when building a Django project. Models define the **structure of your data**, the relationships between different entities in your system, and they serve as the foundation for everything else, your views, serializers, forms, admin panel, and even your database schema. When you design your models well, your app becomes easy to scale, easy to query, and easy to maintain. When you design them poorly, you’ll feel the pain with every new feature you try to add.

## What Is a Model?

A model in Django is just a Python class that inherits from `django.db.models.Model`. Each attribute of that class represents a database column, and each instance of the class represents a row in the table.

Example:

```python
from django.db import models

class Book(models.Model):
    title = models.CharField(max_length=200)
    author = models.CharField(max_length=100)
    published = models.DateField()
```

This creates a `book` table with three columns: `title`, `author`, and `published`.

## Core Principles of Model Design

### 1. Think in Terms of Data, Not Code

Start by asking:

* What kind of data does this app need to store?
* What are the entities?
* How do they relate to each other?

If you're building a blog, you’ll probably need models like `Post`, `Comment`, and `Author`.
If you're building a ticketing system, you might have `Ticket`, `User`, and `Status`.

### 2. Use the Right Field Types

Django provides a wide variety of field types. Pick the one that best matches the nature of your data:

* `CharField`, `TextField`: Strings
* `IntegerField`, `FloatField`, `DecimalField`: Numbers
* `BooleanField`: True/False
* `DateField`, `DateTimeField`: Dates and times
* `FileField`, `ImageField`: File uploads
* `ForeignKey`, `ManyToManyField`, `OneToOneField`: Relationships

Every field can (and often should) have attributes like `blank`, `null`, `default`, `unique`, and `choices` to control behavior and validation.

Example:

```python
class Ticket(models.Model):
    STATUS_CHOICES = [
        ('open', 'Open'),
        ('closed', 'Closed'),
        ('pending', 'Pending'),
    ]
    
    title = models.CharField(max_length=200)
    status = models.CharField(max_length=10, choices=STATUS_CHOICES, default='open')
```

### 3. Use Relationships Wisely

Django makes it easy to define relationships between models:

* `ForeignKey`: Many-to-one
* `ManyToManyField`: Many-to-many
* `OneToOneField`: One-to-one

Example:

```python
class Author(models.Model):
    name = models.CharField(max_length=100)

class Book(models.Model):
    author = models.ForeignKey(Author, on_delete=models.CASCADE)
```

Choosing the correct relationship is critical. It determines how data can be queried and how deletions are handled.

### 4. Normalize, But Don't Overdo It

Database normalization helps avoid duplication and makes your data consistent. But over-normalizing can make your schema overly complex and slow down queries.

* Normalize common, reusable data into separate tables (e.g., Tags, Categories)
* Avoid putting everything into separate tables “just in case”
* It’s okay to denormalize sometimes for performance or simplicity

### 5. Use `Meta` Options for Extra Control

The `Meta` class inside your model lets you control things like table name, ordering, and constraints.

Example:

```python
class Book(models.Model):
    title = models.CharField(max_length=200)

    class Meta:
        ordering = ['title']
        verbose_name_plural = 'Books'
```

### 6. Add `__str__` Methods

Always add a `__str__` method to make your models easier to read in the admin panel and Django shell.

```python
def __str__(self):
    return self.title
```

### 7. Avoid Overusing `null=True` and `blank=True`

Know the difference:

* `null=True`: Database-level. Field can be NULL in the database.
* `blank=True`: Validation-level. Field can be empty in forms and serializers.

Don’t blindly add both to every field unless you know what you’re doing.

### 8. Use Custom Model Methods to Encapsulate Logic

If your model has logic like "calculate total price" or "check if overdue", put it inside a method.

```python
class Invoice(models.Model):
    due_date = models.DateField()

    def is_overdue(self):
        return self.due_date < timezone.now().date()
```

This keeps your logic close to your data and makes your code easier to test and maintain.

### 9. Don't Be Afraid of Abstract Base Models

If multiple models share the same fields (like `created_at`, `updated_at`), you can extract those into an abstract base class:

```python
class TimeStampedModel(models.Model):
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    class Meta:
        abstract = True

class Post(TimeStampedModel):
    title = models.CharField(max_length=200)
```

### 10. Think Ahead

It’s easy to just throw fields into a model as your app grows. But think about:

* How will this data be queried?
* How will it be indexed?
* What if it grows to millions of rows?

Use `db_index=True` on fields you filter on frequently. Consider splitting large models into smaller ones if they’re trying to do too much.
