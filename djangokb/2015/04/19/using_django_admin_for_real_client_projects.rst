Using django admin for real client projects
===========================================



.. author:: Toni Michel
.. categories:: django admin
.. tags:: none
.. comments::


Django Admin is a powerful tool to automatically build user interfaces
from django models. However, the default design is a time travel back
to 2001, which in fact cannot be offered for real clients, except
they are familiar with SAP. Then, they will enjoy the upgrade.

This post gives an overview and on how to use
django admin as a powerful tool for building feature-blown web applications
without restrictions plus a short css snippet for a contemporary django admin design.


Scope
------------------------

In most cases, publicitely accessible web applications require a
fancy responsive design for all kind of different screen sizes.

Althought there are approaches making django admin responsive like
e.g. ...
and these themes work pretty well in 95% of the admin interface,
some powerful tools, like e.g. inlines still have issues. 
Plus, most internal business applications which provide complex forms
are generally used with keyboards and lage screen an not a mobile screen.
Even the iPad just works pretty fine with the non responsive django admin.

In summary, django admin is not the right tool to build
fancy end consumer web applications, but exactly the right tool when it comes to business applications.


Using django admin does not restrict you
-----------------------------------------

Using django admin does not mean that your application is restricted
to use django admin and only django admin.
In fact, the raw django admin only provides your application with a
login and authentication and thus is the perfect basis to build upon.

Most applications do require some sort of  very custom parts, which
cannot be done with django admin - just think about an interactive 
product configurator written in Javascript. The nice thing about django
admin is, that it perfectly plays together with custom parts. 
A django admin application is nothing else than a django application.
An a django application is open to any sort of customization.
So, custom parts can be implemented and integrated as in a custom django application.


However, the advice at this point is, to use django admin where ever it is possible.
Espacially, when it comes to create, update, delete and list objects.
That is the real power of the admin interface.

That's what django admin is made for. Let's e.g.
consider an application where a company needs to automate their 
proposal process in means of scaling sales. So why
code all the views to handle a complex model data from scratch?
All the create views, change views and list views, plus form handling
and templating?

Just use django admin and profit from automated interfaces and handling
logic of

* Creating objects
* Listing objects including filters, pagination, search
* Performing actions on multiple objects
* Changing objects
* Deleting objects including a confirm dialog to show your related objects that might be deleted too
* ...

What really makes django admin so adorable, is that you can completely customize your model admins
in almost any point. In combination with the seamingless integration of custom django views
django admin suits perfect for the development of business tools.



Design
------------------------



