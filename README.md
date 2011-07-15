wapo-django-autocomplete
========================
Autocomplete is an installable app which overrides the raw ID widget with an autocomplete search box.
-----------------------------------------------------

Installation
-------
* The autocomplete templates need to be located by Django, so it's recommended that you add it to your installed apps.
* It's recommended to install this as a git submodule in your existing app.

Code examples
-----
Here's an example of how you would use wapo-django-autocomplete to create an autocomplete field on a foreign key field.

**models.py:**
    
    class Bar(models.Model):
        name = models.CharField(max_length=255)
        ...
        
    class Baz(models.Model):
        name = models.CharField(max_length=255)
        bar = models.ForeignKey(Bar)
        ...
        
    class Foo(models.Model):
        bar = models.ForeignKey(Bar)
        baz = models.ForeignKey(Baz)
        ...

**admin.py**
    
    class FooAdmin(NoLookupsForeignKeyAutocompleteAdmin):
        model = Foo
        related_search_fields = {
            'bar': ('name',),       #search against the name field in the FK bar.
            'baz': ('bar__name',),  #traverse a second foreign key to search against the name of bar.
        }

Todos
-----
* Fix static media: Should be more organically connected to an app.
* Test to make sure templates are properly installing.
* Identify ways to customize returned items in the widget, e.g., limit to 10, alphabetize, etc.