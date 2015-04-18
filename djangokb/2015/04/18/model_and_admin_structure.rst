
Model and admin structure for larger projects
==================================================



.. author:: Toni Michel
.. categories:: none
.. tags:: django, models, model admins, project structure
.. comments::


Smaller projects work fine with all models in a models.py and
all admins in an admin.py. However, if you have a project with
a larger amount of models it makes sense to devide your models and your model admins
into several files.

The straight-forward way to do so is to create a package named ``models`` or respectively
``admins`` and a module for each of your model class and admin class.

As django expects to import your models from ``<appname>.models``, just need to import
your specific models in the ``__init__.py`` of your ``models`` package:

.. code-block:: python

    from myapp.models.news import News
    from myapp.models.gallery import Gallery
    from myapp.models.category import Category
    
Although you get the boilerplate of importing all your models, this is an elegant
way of breaking up your very large model and admin files into smaller, better maintainable pieces of code.

This also works fine for test cases. Instead of having only a single module ``test``,
you just create a package ``test`` and import all your test cases in the ``__init__.py`` file
of your package.

