Title: Tastypie and App Engine's NDB models
Date: 2013-07-27 20:29
Author: alex
Category: Technology
Slug: tastypie-and-app-engines-ndb-models

Tastypie is a great library to plug in and get a REST API fairly easily.
 It's a little more difficult when working with NDB models.  I've
implemented a version of Tastypie's
[resources.py](https://github.com/alexchan/django-tastypie/blob/master/tastypie/ndb_resources.py), to
work with App Engine's datastore models.  Simply import this version and
you can work with Tastypie in the familiar manner except inherit from
NDBResource instead of Resource.

    class MyModelResource(NDBResource):

        class Meta:
            resource_name = 'my_model'
            object_class = MyModel
            authentication = Authentication()
            authorization = Authorization()
            filtering = {
                'active': ('exact',),
                'archived': ('exact',),
            }

