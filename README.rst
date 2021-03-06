======================
django-shop-categories
======================

A extendable category app using django-mptt for django-shop.

Installation
------------

For the current stable version:

:: 
 
    pip install django-shop-categories
    
For the development version:

::

    pip install -e git+git://github.com/fivethreeo/django-shop-categories.git#egg=django-shop-categories

Running tests:

::

    git clone git://github.com/fivethreeo/django-shop-categories.git
    cd django-shop-categories
    virtualenv test_env
    source ./test_env/bin/activate
    pip install -r requirements.txt
    python runtests.py  
    
Configuration
-------------

Add ``shop_categories`` to ``settings.INSTALLED_APPS``.

Set ``SHOP_PRODUCT_MODEL`` to ``shop_categories.models.defaults.product.default.CategoryProduct``.

In your urls.py add this **before** your shop patterns:

::
    
    urlpatterns += patterns('',
        url(r'^catalog/', include('shop_categories.urls')),
    )   

Then run:

::

    manage.py syncdb

Extending
-------------

In your own app make a models dir with __init__.py and a category.py dir, like so:

::

    app/models/__init__.py
    app/models/category.py


In category.py:

:: 

    from django.db import models
    from shop_categories.models.defaults.category.base import ProductCategoryBase
            
    class Category(ProductCategoryBase):
        
        image = models.ImageField(upload_to='categoryimages/', null=True, blank=True)
    
        class Meta:
            abstract = False
            app_label = 'app'

Set ``SHOP_CATEGORIES_CATEGORY_MODEL`` to ``app.models.category.Category``

Register your custom category model in admin.py:

::
    from shop_categories.admin import ProductCategoryAdmin

    admin.site.register(Category, ProductCategoryAdmin)


Then, assuming your Product model is not already synced, run:

::

    manage.py syncdb
    
When extending Product models in your shop make sure they subclass from ``shop_categories.models.defaults.product.base.CategoryProductBase``