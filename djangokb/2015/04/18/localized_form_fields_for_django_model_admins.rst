Localized form fields for django model admins
=============================================



.. author:: Toni Michel
.. categories:: django admin
.. tags:: django, localization, model admin, float field, decimal field
.. comments::



Django provides great support for localization as described 
in https://docs.djangoproject.com/en/1.8/topics/i18n/.

However, when it comes to model admins localization unfortunately stops at the form layer.
As we make profound use of django model admins for client projects
and most of our clients are located in Germany, we definitely require our forms
to handle float and decimal input with german localization.

Django provides a localization attribute (``localize=True``) for form fields.
However, nobody wants to create custom forms for each model admin. 
Instead we got two helpers - one for patching a whole form class, the other for
making your admin forms localized per default:


.. code-block:: python

    from django.contrib import admin
    import django.forms


    class LocalizedAdmin(admin.ModelAdmin):
        '''
        Inherit this model admin to automatically provide localized form inputs
        on all your model admins.
        '''

        def get_form(self, *args, **kwargs):
            form = super(LocalizedAdmin, self).get_form(*args, **kwargs)
            localize_form(form)
            return form


    def localize_form(form):
        '''
        Patch <form> (class) to localize all decimal and float fields fields.
        '''
        def forminit(self, *args, **kwargs):
            super(form, self).__init__(*args, **kwargs)
            for key, field in self.fields.items():
                if isinstance(field, django.forms.DecimalField) or isinstance(field, django.forms.FloatField):
                    field.localize = True
                    field.widget.is_localized = True
                    # set textinput here, as otherwise HTML5 type=number field wont commit any data
                    # https://www.aeyoun.com/posts/html5-input-number-localization.html
                    field.widget = django.forms.TextInput()
        setattr(form, '__init__', forminit)
    

What is really tricky about this hook, is that the default widget for float and decimal fields
renders to ``<input type="number"/>`` which, in most modern browsers does not support localization
and commits an empty values for localized floats and decimals
(read this for detailed information: https://www.aeyoun.com/posts/html5-input-number-localization.html).

That's why we use a TextInput widget here.


