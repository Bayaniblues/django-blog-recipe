Building a blog app, recipe
---
prereq

    python 3.7
    virtualenv
    pip install django
    
start django

    django-admin startproject mysite
    
Tree structure
    
    mysite/
        manage.py
        mysite/
            __init__.py
            settings.py
            urls.py
            wsgi.py

manage.py - used to interact with project, no need to edit this file.

__init__.py - an empty file that tells python to treat mysite directory as a python module

settings.py - settings and configuration for project

urls.py - URLS pattens live here, for routing

wsgi.py - run project as WEB SERVER GATEWAY INTERFACE application

create Database
---

    cd mysite
    python manage.py migrate
    
Run Development server
---
    
    python manage.py runserver

then open http://127.0.0.1:8000/

Inside Settings.py
---

    DEBUG
    
Debug is a boolean that turns on/off debug mode, set to TRUE to show error pages, set to FALSE when deploying 
    
    ALLOWED_HOSTS
    
Add the host/domain here while deploying
    
    INSTALLED APPS
    
Which applications are active for this site
        
        django.contrib.admin
        
this is admin site
        
        django.contrib.auth
        
Authentication framework
        
        django.contrib.contenttypes
        
Framework for content types
        
        django.contrib.sessions
        
framework for sessions

        django.contrib.messages
        
framework for messages
        
        django.contrib.staticfiles
        
framework for managing static files
        
    MIDDLEWARE
    
List that contains middleware
    
    ROOT_URLCONF
    
where root URL patterns of apps are defined
    
    DATABASES
    
dictionary that contains settings for all databases, uses SQLite default
    
    LANGUAGE_CODE
    
default language for django
    
    USE_TZ
    
activate/deactivate timezones

Creating an application
---
From root, run the following command
    
    python manage.py startapp blog
    
Tree structure should be

    blog/
        __init__.py
        admin.py
        apps.py
        migrations/
            __init__.py
        models.py
        tests.py
        views.py

admin.py - register models in django admin site

apps.py - include main config of the blog app

migrations - contain database migrations, tracks models changes synchronize database

models.py - data models of app, django apps need to have models.py

tests.py - add tests to app

views.py - logic of applications goes here

Design Blog data schema
---
Define data models for the blog, 

Define Post model in the models.py file of the blog application.

        from django.db import models
        from django.utils import timezones
        from django.contrib.auth.models import User
        
        class Post(models.Model):
            STATUS_CHOICES = (
                ('draft', 'Draft')
                ('published', 'Published'),
            )
            title = models.CharField(max_length=250)
            slug = models.SlugField(max_length=250,
                                    unique_for_delete='publish')
            author = models.ForeignKey(User, 
                                       on_delete=models.CASCADE,
                                       related_name='blog_posts')
           body = models.TextField()
           publish = models.DateTimeField(default=timezone.now)
           created = models.DateTimeField(auto_now_add=True)
           updated = modeles.DateTimeField(auto_now=True)
           status = models.CharField(max_length=10,
                                     choices=STATUS_CHOICES,
                                     default='draft')
           class Meta:
                ordering = ('-publish',)
                
            def __str__(self):
                return self.title
                
Data model fields

    Title
    
Field for the post title, translate to VARCHAR column in SQL database

    slug
    
Used in URLS, its a short label, use slug to create SEO friendly URL for blog posts

The unique_for_date parameter builds URLs for posts by using publish date and slug.

Django will prevent multiple posts from having the same slug
    
    author
    
Foreign key in database, tells django that the post is written by user, the
parameters allows us to name user and delete blog posts. CASCADE deletes everything.
    
    body
    
Thi field is a text field, translates to TEXT column in the sql database.
    
    publish
    

    
    created
    
    
    
    updated
    
    
    
    status
                                     
                                     
                                     



Activate application
---

Creating/applying migrations
---

creating admin site for models
---

Create SuperUser
---

Adding models to admin site
---

Customizing models are displayed
---

Working with QuerySet
---

Creating Objects
---

Updating objects
---

Retrieving objects
---

Filter method
---

Using exclude()
---

Using order_by()
---

Deleting objects
---

Querysets
---

Creating model mangers
---

Building list and detail views
---

Creating list and detail views
---

Adding URL patterns
---

Canonical URL for models
---

Creating Templates for views
---

Adding pagination
---

Using class-based views
----









    