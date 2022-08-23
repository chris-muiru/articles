## installed app
holds name of all  django appd that are activated in django instace

#### creating models
```py
from django.db import models


class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```
## syntax
- **model**
  - fields > field types > field options

### common field types
- TextField
- CharField
- DateField
- EmailField

## field options
`choices` - sequence of two tuples to use as choices for this field.choices look like this
```py
YEAR_IN_SCHOOL_CHOICES = [
    ('FR', 'Freshman'),
    ('SO', 'Sophomore'),
    ('JR', 'Junior'),
    ('SR', 'Senior'),
    ('GR', 'Graduate'),
]
```
- first element in tuple stored in database,second displayed by field's form widget.
```py
from django.db import models

class Person(models.Model):
    SHIRT_SIZES = (
        ('S', 'Small'),
        ('M', 'Medium'),
        ('L', 'Large'),
    )
    name = models.CharField(max_length=60)
    shirt_size = models.CharField(max_length=1, choices=SHIRT_SIZES)


>>> p = Person(name="Fred Flintstone", shirt_size="L")
>>> p.save()
>>> p.shirt_size
'L'
>>> p.get_shirt_size_display()
'Large'

```
- **primary_key**
if `True`, this field is the primary key for the model. 
if you dont add `primary_key=True`,django will automatically add an `Integerfield` to hold the primary key.
```py
class Fruit(models.Model):
    name=models.CharField(max_length=100,primary_key=True)
```
- **unique** - if true field must be unique throughout the table.


### Relationships
**Many to one relationships**

To define a many to one rship,use `django.db.models.ForeignKey`. You use it just like any other `Field` type by including it as a class attribute of your model.

For example, if a `Car` model has a `Manufacturer` – that is, a `Manufacturer` makes multiple cars but each `Car` only has one `Manufacturer` – use the following definitions:
```py
from django.db import models

class Manufacturer(models.Model):
    # ...
    pass

class Car(models.Model):
    manufacturer = models.ForeignKey(Manufacturer, on_delete=models.CASCADE)
    # ...
```
(we shall cover recursive rships when we need them)
It’s suggested, but not required, that the name of a `ForeignKey` field (manufacturer in the example above) be the name of the model, lowercase.

**Many to many rship**
use `ManyToManyField`.

For example, if a `Pizza` has multiple `Topping` objects – that is, a `Topping` can be on multiple `pizzas` and each `Pizza` has multiple `toppings` – here’s how you’d represent that:
```py
from django.db import models

class Topping(models.Model):
    # ...
    pass

class Pizza(models.Model):
    # ...
    toppings = models.ManyToManyField(Topping)

```
It’s suggested, but not required, that the name of a `ManyToManyField` (toppings in the example above) be a `plural` describing the set of related model objects.
it doesn't matter the model you put the field but it should be only one.

Generally, `ManyToManyField` instances should go in the object that’s going to be edited on a form. In the above example, toppings is in Pizza (rather than Topping having a pizzas ManyToManyField ) because it’s more natural to think about a pizza having toppings than a topping being on multiple pizzas. The way it’s set up above, the Pizza form would let users select the toppings.   


**one to one rship**

This is most useful on the primary key of an object when that object “extends” another object in some way.

uses `OneToOneField`.
For example, if you were building a database of “places”, you would build pretty standard stuff such as address, phone number, etc. in the database. Then, if you wanted to build a database of restaurants on top of the places, instead of repeating yourself and replicating those fields in the `Restaurant` model, you could make `Restaurant` have a `OneToOneField` to `Place` (because a restaurant “is a” place; in fact, to handle this you’d typically use inheritance, which involves an implicit one-to-one relation).

OneToOneField classes used to automatically become the primary key on a model. This is no longer true (although you can manually pass in the primary_key argument if you like). Thus, it’s now possible to have multiple fields of type OneToOneField on a single model.

## meta options 
Give your model metadata by using an inner class `Meta`
```py
from django.db import models

class Ox(models.Model):
    horn_length = models.IntegerField()

    class Meta:
        ordering = ["horn_length"]
        verbose_name_plural = "oxen"

```
Model metadata is “anything that’s not a field”, such as ordering options (ordering), database table name (db_table), or human-readable singular and plural names (verbose_name and verbose_name_plural). None are required, and adding class Meta to a model is completely optional.