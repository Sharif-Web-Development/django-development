# What is Django's Admin Panel?

Django has an admin panel to help you with the CRUD operations on your models. In that panel, you can:

- Create new instances of your models
- Edit them
- See them as a list
- Delete them

# How Can I Access It?

First of all, you have to create a **superuser**. The following command can achieve this:

```
python manage.py createsuperuser
```

It will ask you for some information about the superuser you want to create and then create it.

You must now go to the admin panel, which is typically located in your web application's /admin path. For example, if you're hosting your Django app on `localhost:8000`, the admin panel for that app would be located in `localhost:8000/admin`.

# Does Every Django App Have a Django Admin Panel?

No. These are the reasons you may not want an admin panel:

1. You're only using Django to develop a back-end application, and your front-end app provides you with an admin panel.
2. You want a custom admin panel (good luck making that :-) )
3. You see it as a security issue.

# User Privilege Hierarchy in Django

Typically, a Django application has this hierarchy of user privileges:

1. **Superuser:** This user has the highest level of privileges and can control all aspects of the application. They are often referred to as "system administrators."
2. **Staff:** These users possess higher privileges than regular users. While they do not have as much power as superusers, they can access and modify data more freely than typical users.
3. **User**: This category includes the regular users of your web application. For example, if you are not currently working at Spotify, you would be considered a typical Spotify user. You have no control over the data in the application and cannot modify any data that is not created by your own user account.