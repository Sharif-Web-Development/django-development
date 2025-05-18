## What are Django Migrations and Migration Files?

Django migrations are like version control for your database schema. Whenever you make changes to your models, like adding a new field or creating a new model, Django wants you to **generate a migration file** to keep track of what changed. This way, your code and your database stay in sync. A migration file is also just a Python file that describes changes to the database in a way Django understands. You don’t need to write it by hand ***(and you usually shouldn’t)***.

## How Do I Create a Migration?

Once you’ve made changes to your models, run this command:

```
python manage.py makemigrations
```

Django will then generate a migration file in your app’s `migrations/` directory.

## How Do I Apply a Migration?

To actually apply those changes to your database, use:

```
python manage.py migrate
```

This is where Django takes the migration file and turns it into real SQL commands that update your database tables.

## Quick Example

Let’s say you add a `pages` field to your `Book` model:

```python
class Book(models.Model):
    title = models.CharField(max_length=200)
    author = models.CharField(max_length=100)
    published = models.DateField()
    pages = models.IntegerField()  # New field
```

You would then run:

```
python manage.py makemigrations
python manage.py migrate
```

Boom. Django will add a `pages` column to your `book` table.

## A Few Things to Remember

- You **should** commit your migration files to version control (e.g., Git).
- Every migration has a **number** and **depends on the previous one**—don’t delete or rearrange them carelessly.
- If you change or delete a model field, Django won’t touch your data unless you tell it to. Be careful!
- Sometimes Django will ask for input when generating migrations (like a default value). Read what it says before hitting Enter mindlessly.