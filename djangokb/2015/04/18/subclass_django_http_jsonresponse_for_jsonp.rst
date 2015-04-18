
Subclass django.http.JsonResponse for JSONP
===========================================



.. author:: Christian Bergmiller
.. categories:: none
.. tags:: django, json, jsonp
.. comments::

Since Django 1.7 you can use ``django.http.JsonResponse`` if you need HTTP responses with serialized data structures in JSON format. Since ``JsonResponse`` is a subclass of ``django.http.HttpResponse`` it is easy to reuse this pattern for HTTP responses with JSONP (JSON with padding) data:

.. code:: python
	
	class JsonPaddingResponse(HttpResponse):
		"""
		An HTTP response class that consumes data to be serialized to JSONP.
		:param data: Data to be dumped into json. By default only ``dict`` objects
		  are allowed to be passed due to a security flaw before EcmaScript 5. See
		  the ``safe`` parameter for more information.
		:param callback: Callback ID
		:param encoder: Should be an json encoder class. Defaults to
		  ``django.core.serializers.json.DjangoJSONEncoder``.
		"""
		
		def __init__(self, data, callback, encoder=DjangoJSONEncoder, **kwargs):
			kwargs.setdefault('content_type', 'application/json')
			data = '{0}({1});'.format(callback, json.dumps(data, cls=encoder))
			super(JsonPaddingResponse, self).__init__(content=data, **kwargs)

To generate a JSONP response in a view you also have to read the callback string from the incoming GET request.

.. code:: python
	
	callback = request.GET['callback']
	
