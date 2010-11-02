===============
django-stdimage
===============

This is a forked version of Marc Garcia's django-stdimage found at http://code.google.com/p/django-stdimage/.  It includes a setup 
script and some patches (see CHANGELOG) and custom fixes.

django-stdimage extends django's ImageField class:

* Automatic resizing
* Automatic thumbnail creation
* Renaming after creation 
    * location is definable with upload_to just like in ImageField, but file names are standardized using the field name and instance id
* Image/thumbnail deletion after creation

Installation
============

Installing from the forked source
---------------------------------

Clone the repository and run the following command inside the 
django-stdimage directory:::

    python setup.py install

Or you can place the included ``stdimage`` directory somewhere on 
your Python path, or symlink to it from somewhere on your Python path.


Usage
=====

It's not necessary to include anything in INSTALLED_APPLICATIONS.

Import StdImageField and use in your models. Example:

In your model
-------------
::

    from stdimage.fields import StdImageField

    class MyClass(models.Model):
        # works just like ImageField
        image1 = StdImageField(upload_to='path/to/img') 

        # can be deleted throwgh admin
        image2 = StdImageField(upload_to='path/to/img', blank=True) 

        # resizes image to maximum size to fit a 640x480 area
        image3 = StdImageField(upload_to='path/to/img', size=(640, 480)) 
        
        # resizes image to 640x480 croping if necessary
        image4 = StdImageField(upload_to='path/to/img', size=(640, 480, True)) 

        # creates a thumbnail resized to maximum size to fit a 100x75 area
        image5 = StdImageField(upload_to='path/to/img', 
            thumbnail_size=(100, 75))

        # creates a thumbnail resized to 100x100 croping if necessary
        image6 = StdImageField(upload_to='path/to/img', 
            thumbnail_size=(100, 100, True)) 

        # all previous features in one declaration
        image_all = StdImageField(upload_to='path/to/img', blank=True, 
            size=(640, 480), thumbnail_size=(100, 100, True)) 


For using generated thumbnail in templates use "myimagefield.thumbnail". Example:::

    <a href="{{ object.myimage.url }}"><img alt="" src="{{ object.myimage.thumbnail.url }}"/></a>


About image names
=================

StdImageField stores images in the filesystem by modifying the name of the uploaded file. Renames are performed using the field name and object primary key. "jpg" extesions are replaced with the standard "jpeg".

For example, if the stdimage field is named 'image_upload' with parameters seet to generate a thumbnail, and an image is uploaded with the filename 'myimage.jpg', the resulting images on filesystem would be (assume this image belongs to an instance with pk=14):

* image_upload_14.jpeg
* image_upload_14.thumbnail.jpeg