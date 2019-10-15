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
    
used DateTimeField to timestamp when the post was published
    
    created
    
indicate when the post was created
    
    updated
    
shows the last time the post was created.
    
    status
                                     
status shows the status of the post with the choices parameter.                                     
                                     
Look up the different types of fields at 

https://dics.djangoproject.com/en/2.0/ref/models/fields/


Activate application
---

edit the Settings.py file, add 
    
    'blog.apps.BlogConfig' 
    
to the Installed_apps setting

Creating/applying migrations
---

migrate command makes changes to the database

    python manage.py makemigrations blog
    
    python manage.py migrate

creating admin site for models
---

Django comes with builtin admin

Create SuperUser
---

    python manage.py createsuperuser

Adding models to admin site
---

start dev server

    python manage.py
    
Add models to the admin site. edit blog/admin.py

    from django.contrib import admin
    from .models import Post
    
    admin.site.register(post)
    
Then create a blog post

Customizing models are displayed
---
editing the admin.py

    from django.contrib import admin
    from .models import Post
    
    @admin.register(Post)
    class PostAdmin(admin.ModelAdmin):
        list_display = ('title', 'slug', 'author', 'publish', 'status')
        
add more options

    list_filter = ('status', 'created', 'publish', 'author')
    search_fields = ('title', 'body')
    prepopulated_fields = {'slug': ('title',)}
    raw_id_fields = ('author',)
    date_hierarchy = 'publish'
    ordering = ('status', 'publish')

Working with QuerySet
---

Django can work with multiple databases, find out more at https://docs.djangoproject.com/en/2.0/ref/models/

Creating Objects
---

    python manage.py shell
    
    >> from django.contrib.auth.models.import User
    >> from blog.models import Post
    >> user = User.objects.get(username='admin')
    >> post = Post(title='Another post',
                    slug='another-post'
                    body='Post body',
                    author=user)
                    
    >> post.save()
    
We create a post instance with a custom title, slug, body. set the user we previously retrieved as the author of the post.

    post = Post(title='Another post', slug='another-post', body='Post-body', author=user)
    
    post.save()
    
the above create an INSERT sql statement. we can also use the create() method.

    Post.objects.create(title='one more post', slug='one-more-post', body-'Post body', author=user) 

Updating objects
---

saving the object

    >> post.title = 'New Title'
    >> post.save()
    
the save method performs an UPDATE SQL statement

Retrieving objects
---

use the method Post.object.get() to fetch a single object from the database.

use the Post.objects.all() to fetch all objects in a database

    >> all_posts = Post.objects.all()
    
Django QuerySets are Lazy, and are not evaluated unless forced to. 

Filter method
---

use the filter() method to select a specific post

    Post.object.filter(publish__year=2017)
    
to filter out multiple parameters
    
    Post.objects.filter(publish__year-2017, author__username='admin')
    
Building querySet chaining multiple filters

    Post.objects.filter(publish__year=2017) \ 
                .filter(author__username='admin')

Using exclude()
---

exclude method fetches all posts excluding a specific pattern.

    Post.objects.filter(publish__year=2017) \
                .exclude(title__startswith="Why")

Using order_by()
---

order the results by different fields. By using the order_by() method.

    Post.objects.order_by('title') 
    
    Post.objects.order_by('-title')

Deleting objects
---

deletes a specific post, will delete everything if on_delete == CASCADE

    post = Post.objects.get(id=1)
    post.delete()

Querysets
---

Querysets are evaluated in the following cases.

    - first time interated
    - sliceing querysets, Post.objects.all()[:3]
    - when pickled
    - when called repr() or len()
    - when explicitly called list()
    - when testing in a statement, like bool() or and or if

Creating model mangers
---

Create model managers 

    Post.object.my_manager()
    Post.my_manager.all()
    Post.published.all()

edit blog/models.py and add custom manager:

    class PublishedManager(models.Manager):
        def get_queryset(self):
            return super(PublishedManager,
                        self).self_queryset()\
                             .filter(status="published")
                             
    class Post(models.Model):
        objects = model.Manager()
        published = PublishedManager()
        
start dev server to get all published posts

    python manage.py shell
    
    Post.published.filter(title__startwith="who")

Building list and detail views
---

A Django view is a python function that receives a web request. 

Creating list and detail views
---

Editing the views.py file.

    from django.shortcuts import render, get_object_or_404
    from .models import Post
    
    def post_list(request):
        posts = Post.published.all()
        return render(request,
                        'blog/post/list.html',
                        {'posts': posts})

Adding URL patterns
---

the URL patterns allows us to map URL to views.

create blog/urls.py

    from django.urls import path
    from . import views
    
    app_name = 'blog'
    
    urlpatterns = [
        path('', views.post_list, name='post_list'),
        path('<int:year>/<int:month>/<int:day>/<slug:post>',
            views.post_detail,
            name='post_detail'
        ),
    ]
    
app_name - define app namespace, and organizes URL's
path() - define path patterns
post_list - our views name
post_detail - takes 4 arguments
    year - integer
    month - integer
    day - integer
    post - words and hyphens
    
angle brackets to capture values in URL, as <parameter>, we use path converters like 

    <int:year> matche and return integer.
    
    <slug:post> to specifically match a slug.
    
all path converters be found at https://docs.djangoproject.com/en/2.0/topics/http/urls/#path-converters

regular expressions https://docs.djangoproject.com/en/2.0/ref/urls/#django.urls.re_path

https://docs.python.org/3/howto/regex.html

creating a urls.py for each app. 

    from django.urls import path, include
    from django.contrib import admin
    
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('blog/', include('blog.urls', namespace='blog')),
    ]
    
namespaces should be unique, more inof at https://docs.djangoproject.com/en/2.0/topics/http/urls/#url-namespaces


Canonical URL for models
---

get_absolute_url() - returns templates of a specific post
reverse() method  - to build URLs by name

In models.py add this code

    class Post(models.Model):
        def get_absolute_url(self)
            return reverse('blog:post_detail',
                            args=[self.publish.year,
                                  self.publish.month,
                                  self.publish.day, 
                                  self.slug])

Creating Templates for views
---

create the following directories inside the blog application

    templates/
        blog/
            base.html
            post/
                list.html
                detail.html
    
Django contains a powerful template language, containing template tags, template variables, and template filters

    {% tag %} control the rendering of the templates 
    
    {{ variable }} variables get replaced with values when the template is rendered
    
    {{variable|filter}} filters modifies variabls for display
    
edit the base.html to add the following code.

    {% load static %}
    <!DOCTYPE html>
    <html>
        <head>
            <title>{% block title %}{% endblock %}</title>
            <link href="{% static "css/blog.css" %}" rel="stylesheet">
        </head>
        <body>
            <div id="content">
                {% block content %}
                {% endblock %}
            </div>
            <div id="sidebar">
                <h2>myblog</h2>
                <p>this is my blog<p>
            </div>
        
        </body>
        
    </html>
    
{% load static %} tells django to load static template file provided by the django.contrib.staticfiles app and uses
the {% static %} template.

{% block %} defines where to define the block in an area.

edit the post/list.html

    {% extends "blog/base.html" %}
    
    {% block title %}My blog{% endblock %}
    
    {% block content %}
        <h1>My Blog</h1>
        {% for post in posts %}
            <h2>
                <a href="{{ post.get_absolute_url }}">
                    {{ post.title }}
                </a>
            </h2>
            <p class='date'>
                Published {{ post.pubish }} by {{ post.author }}
            </p>
            {{ post.body|truncatewords:30|linebreaks }}
        {% endfor %}
    {% endblock %}
    
{% extends %} inherits from the blog/base.html

truncatewords truncates the value to the number of words specified.

linebreaks converts the output into HTML linebreaks

edit the post/detail.html file for blog details

    {% extends "blog/base.html" %}
    {% block title %}{{ post.title }}{% endblock %}
    
    {% block content %}
    
        <h1>{{ post.title }}</h1>
        <p class="date">
            Published {{ post.publish }} by {{ post.author }}
        </p>
        {{ post.body|linebreaks}}
    {% endblock %}
        
    
    

Adding pagination
---

Pagination allows us to split multiple post across many pages.

    1. instantiate Paginator class with number of objects to display on each page
    2. get page GET parameter to indicate the page number
    3. obtain the objects for the desired page calling the page() method
    4. if page != integer, then  get the first page of results. will retrieve the last page.
    5. We pass the page number and retrieved objects to the template.

Edit the views.py

    from django.core.paginator import Paginator, EmptyPage, \
                                      PageNotAnInteger
                                      
    def post_list(request):
        object_list = Post.published.all()
        paginator = Paginator(object_list, 3) # 3 posts in each page
        page = request.GET.get('page')
        try:
            post = paginator.page(page)
        except PageNotAnInteger:
            # if not integer, deliver first page
            posts = paginator.page(1)
        except EmptyPage:
            # IF page is out of range deliver last page of results
            posts = paginator.page(paginator.num_pages)
        return render(request,
                       'blog/post/list.html',
                        {'page': page,
                         'posts': posts})
                         
                         
Go to templates/blog/pagination.html and add...

    <div class="pagination">
        <span class='step-links'>
        
            {% if page.has_previous %}
                <a href="?page={{ page.previous_page_number }}">Previous</a>
            {% endif %}
            
            <span class="current">
                Page {{ page.number }} of {{ page.paginator.num_pages }}.
            </span>
            
                {% if page.has_next %}
                    <a href="?page={{ page.next_page_number }}"> Next </a>
                {% endif %}            
        </span>
    </div>
    

blog/post/list.html

    {% block content%}
        ...
        {% include "pagination.html" with page=posts %}
    {% endblock %}


Using class-based views
----

An object oriented way to implement views instead of functions

Organize HTTP methods to GET POST PUT 

more class based views at https://docs.djangoproject.com/en/2.0/topics/class-based-views/intro/

change views.py

    from django.views.generic import Listview
    
    class PostListView(ListView):
        queryset = Post.published.all()
        context_object_name = 'posts' 
        paginate_by = 3
        template_name = 'blog/post/list.html'
        
open urls.py add this to URL patterns

    path('', views.PostListView.as_view(), name="post_list")
    
open post/list.html and edit

    {% include "pagination.html" with page=page_obj%}


