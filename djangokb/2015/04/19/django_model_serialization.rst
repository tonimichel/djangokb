
Django model serialization
==========================



.. author:: Christian Bergmiller
.. categories:: none
.. tags:: django, model, serialization, json
.. comments::

When i started to use Django for web development there was one obstacle that gave me a hard time to overcome: *serialization of model data*.

If you try to use ``json.dumps`` on a QuerySet you will get an error that looks something like this:

.. code-block:: python

	query = Foo.objects.all()
	data = json.dumps(query)
	
	TypeError: [<Foo: Bar>] is not JSON serializable

This is because the json encoder used for serialization only knows how to deal with pythons basic data types. Therefore serialization of django models has to be done in two steps:

- Conversion of the model-instances to basic python data types (e.g. dict, list, string, integer, float)
- Serialization to the desired format (e.g. JSON, XML)

Depending on the format you need, there are different ways to go:

Django.core.serializers
-----------------------

Django has an built-in model serialization framework that can do all the work in one line of code:

.. code-block:: python

	from Django.core import serializers
	query = models.Foo.objects.all()
	data = serializers.serialize( "json", query )

The resulting JSON data structure is a full representation of the model data:

.. code-block:: json

	[{
		"fields": {
			"<field1>": "<content_field1>",
			<...> 
		},
		"model": "myApp.foo",
		"pk": <id>
	}, <...> ]

See the Django documentation on ``django.core.serializers`` for more details:
https://docs.Djangoproject.com/en/1.8/topics/serialization/

ValuesQuerySet and json.dumps
-----------------------------

If you want a less verbose format, you can use the Methods ``values`` and ``values_list``. They return QuerySet subclasses that return dicts and lists when iterated, rather than model-instance objects. Dicts and lists of basic python data types can be serialized with json.dumps:

.. code-block:: python

	import json
	# serialize all Foo model objects as a list of dicts
	query = Foo.objects.values()
	data = json.dumps(list(query))
	# serialize all Foo model objects as a list of lists
	query = Foo.objects.values_list()
	data = json.dumps(list(query))
	

See the QuerySet API reference for details:
http://docs.Djangoproject.com/en/1.8/ref/models/querysets/#values
http://docs.Djangoproject.com/en/1.8/ref/models/querysets/#values-list

Custom Format
-------------

If you need to fully customize the way your model is turned into a serialized format, you can write your own QuerySet method that builds an custom python data structure and again serialize via json.dumps:

.. code-block:: python

	import json
	from django.db import models
	from django.core.serializers.json import DjangoJSONEncoder
	
	class FooQuerySet(models.QuerySet):
		"""Custom QuerySet extension"""
		def to_json(self):
			"""serialize QuerySet as JSON"""
			data = { foo.id: foo.to_json_dict() for foo in self }
			return json.dumps( data, cls=DjangoJSONEncoder )
			
	class Foo(models.Model):
		objects	= FooQuerySet.as_manager()
		field1 = models.IntegerField()
		field2 = models.CharField(max_length=10)
		def to_json_dict(self):
			return {
				"field1": self.field1,
				"field2": self.field2,
			}
			

To use this code you simply call ``to_json()`` on a QuerySet of the Foo model:

.. code-block:: python

	Foo.objects.all().to_json()
	
You can adapt the ``to_json_dict`` model method and the ``to_json`` QuerySet method to get the format you need.
