# Django Settings

The **Django settings file** (`settings.py`) is the brainstem of your Django project. It holds all the configuration for your app, everything from which database to use to whether your app is in debug mode, to how Django handles authentication, internationalization, static files, middleware, and much more. You _can’t_ build a serious Django project without understanding this file inside-out.

## `DEBUG`

```python
DEBUG = True
```

This tells Django whether to show full error tracebacks or to hide them behind a generic “500 error” page.

- **`True`**: Show detailed errors. Only use this in development.
- **`False`**: Hide details. Use this in production. Please. For everyone’s safety.


## `ALLOWED_HOSTS`

```python
ALLOWED_HOSTS = ['localhost', '127.0.0.1', 'mycoolsite.com']
```

Django won’t serve your app unless the host making the request is on this list. When `DEBUG = False`, it becomes mandatory.


## `INSTALLED_APPS`

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    ...
    'your_custom_app',
]
```

This is a list of all Django apps (built-in and custom) that are active in your project. If it’s not listed here, Django ignores it. This includes built-in stuff like:

- `admin`: Admin panel
- `auth`: User authentication
- `contenttypes`, `sessions`, `messages`: Core Django features

## `MIDDLEWARE`

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    ...
]
```

Think of middleware as Django’s airport security. Every HTTP request and response passes through this list in order.

Some examples:
- `SessionMiddleware`: Enables sessions
- `AuthenticationMiddleware`: Associates users with requests
- `CommonMiddleware`: Adds trailing slashes, forbids some bad requests, etc.


## `ROOT_URLCONF`

```python
ROOT_URLCONF = 'myproject.urls'
```

Tells Django which file to look in for URL patterns. Usually it's `urls.py` in your root project folder. This is where routing starts.

## `TEMPLATES`

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],
        ...
    },
]
```

Controls how Django renders HTML templates. You define:

- Template engine backend (usually DjangoTemplates)
- `DIRS`: Custom template locations
- `APP_DIRS`: Whether to look in each app’s `templates/` folder
- Context processors: Auto-add common variables (like `user`) to every template

## `DATABASES`

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',  # or 'postgresql', 'mysql', etc.
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

Tells Django which database engine to use and how to connect to it. You can swap SQLite for Postgres/MySQL easily, just update this dict.

## `STATIC_URL` and `MEDIA_URL`

```python
STATIC_URL = '/static/'
MEDIA_URL = '/media/'
```

- `STATIC_URL`: For CSS, JS, images that **don’t** change. 
- `MEDIA_URL`: For user-uploaded files (like profile pics).

You’ll also see:

```python
STATICFILES_DIRS = [BASE_DIR / "static"]
MEDIA_ROOT = BASE_DIR / "media"
```

Which tell Django _where to find or store_ those files.


## `LANGUAGE_CODE` and `TIME_ZONE`

```python
LANGUAGE_CODE = 'en-us'
TIME_ZONE = 'Asia/Tehran'
USE_I18N = True
USE_TZ = True
```

These control internationalization and timezone behavior. Django apps are built to support multi-language and timezone-aware applications out of the box.

## `SECRET_KEY`

```python
SECRET_KEY = 'django-insecure-4s!$k3y!@#!%etc'
```

This is **your app’s cryptographic identity**. It signs sessions, tokens, and more.

- Never hardcode this in production. 
- Never commit it to Git.
- Use environment variables or a `.env` file to store it securely.

## `EMAIL_*` Settings

Used when your app sends emails (for password resets, notifications, etc.):

```python
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_PORT = 587
EMAIL_USE_TLS = True
EMAIL_HOST_USER = 'your_email@example.com'
EMAIL_HOST_PASSWORD = 'your_password'
```

For dev, use:

```python
EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'
```

So Django just prints the email to your terminal instead of actually sending it.


## `AUTH_USER_MODEL`

```python
AUTH_USER_MODEL = 'users.CustomUser'
```

If you’re using a **custom user model**, this is mandatory. You must define this **before** creating your first migration. Changing it afterward is painful.


## `TEST_RUNNER`, `LOGGING`, etc.

You can configure custom test runners, logging outputs, security settings like:

```python
SECURE_HSTS_SECONDS = 31536000
CSRF_COOKIE_SECURE = True
SESSION_COOKIE_SECURE = True
```

Which are all crucial in production deployments.

## ENVIRONMENT-SPECIFIC SETTINGS

Your `settings.py` should **not** have secrets or production-only stuff hardcoded. Use:

- `python-decouple`
- `django-environ`
- or just plain old `os.environ`

Example:

```python
import os

DEBUG = os.environ.get('DEBUG', 'False') == 'True'
SECRET_KEY = os.environ.get('SECRET_KEY')
```


## TL;DR — What You Should Definitely Know

|Setting|Why It Matters|
|---|---|
|`DEBUG`|Dev vs. Production behavior|
|`ALLOWED_HOSTS`|Prevents host header attacks|
|`INSTALLED_APPS`|Enables Django and third-party features|
|`MIDDLEWARE`|Controls how requests/responses are processed|
|`DATABASES`|Database engine + connection|
|`TEMPLATES`|Controls rendering logic|
|`STATIC_URL`|Static files (CSS, JS)|
|`MEDIA_URL`|User-uploaded content|
|`AUTH_USER_MODEL`|Custom user model setup|
|`SECRET_KEY`|Don’t leak this. Ever.|
