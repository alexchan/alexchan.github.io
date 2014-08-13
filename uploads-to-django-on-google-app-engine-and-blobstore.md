Title: Uploads To Django On Google App Engine And Blobstore
Date: 2013-03-11 20:25
Author: alex
Category: Technology
Slug: uploads-to-django-on-google-app-engine-and-blobstore

Problem
-------

I absolutely love Django. There are many reasons to love the framework
but sometimes it doesn't seem to work out. I recently tried to upload
files using Google's App Engine and Django to Google's blobstore. The
main reason why it doesn't work is because of an issue with Django and
supporting multi-part values.

Alternatives
------------

One alternative is to use the django-filetransfers library and upload
assets to another repository such as S3. If you need to stay on Google
App Engine, another possibility is to sadly use another web framework
such as webapp2.

