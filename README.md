### Django Forms
* Python 3.8.6
* Django 4.2.13
* Pip 20.3.3

```
$ pip install virtualenv
$ virtualenv venv
$ ./venv/Scripts/activate (for windows)
$ source venv/bin/activate (for mac)
$ pip install django
$ django-admin startproject pizzahouse
$ mv pizzahouse pizzahouse-project
$ cd pizzahouse-project
$ python manage.py runserver 9005
```

#### Make a new app

```
$ cd pizzahouse-project
$ django-admin startapp pizza
```

### Simpler way to create form
```python

# models.py
from django.db import models

class Size(models.Model):
    title = models.CharField(max_length=100)
    def __str__(self):
        return self.title


class Pizza(models.Model):
    topping1 = models.CharField(max_length=100)
    topping2 = models.CharField(max_length=100)
    size = models.ForeignKey(Size,on_delete=models.CASCADE)

# forms.py
from django import forms
from .models import Pizza

# class PizzaForm(forms.Form):
#     topping1 = forms.CharField(label='Topping 1', max_length=100)
#     topping2 = forms.CharField(label='Topping 2', max_length=100)
#     size = forms.ChoiceField(label='Size', choices=[('Small', 'Small'), ('Medium', 'Medium'), ('Large', 'Large')])

class PizzaForm(forms.ModelForm):
    class Meta:
            model = Pizza
            fields = ['topping1', 'topping2', 'size']
            labels = {
            "topping1": "Topping 1",
            "topping2": "Topping 2",
            }


```

#### Run the migrations
```
$ python manage.py makemigrations
$ python manage.py migrate
$ python manage.py createsuperuser

```

* Forms
* * Customize forms using labels
* * Customize forms using widgets (for multiple checkbox, select dropdown, text area, password)
* Forms & Files
* * $ pip install pillow


```python
# forms.py

from django import forms
from .models import Pizza, Size

# class PizzaForm(forms.Form):
#     topping1 = forms.CharField(label='Topping 1', max_length=100)
#     topping2 = forms.CharField(label='Topping 2', max_length=100)
#     size = forms.ChoiceField(label='Size', choices=[('Small', 'Small'), ('Medium', 'Medium'), ('Large', 'Large')])

class PizzaForm(forms.ModelForm):

    image = forms.ImageField()

    class Meta:
            model = Pizza
            fields = ['topping1', 'topping2', 'size']
            labels = {
            "topping1": "Topping 1",
            "topping2": "Topping 2",
            }

class MultiplePizzaForm(forms.Form):
    number = forms.IntegerField(min_value=2, max_value=6)

# order.html



    <form enctype="multipart/form-data" {% url 'order' %}" method="post">
        {% csrf_token %}
        {{ pizzaform }}
        <input type="submit" value="Order Pizza">
    </form>

<br><br>
Want more than one pizza?
<form action="{% url 'pizzas' %}" method="get">
  {% csrf_token %}
  {{ multiple_form }}
  <input type="submit" value="Get Pizzas">
</form>



```

* Add in "novalidate" to form tag, if you do not want built-in validation to work
* Forms can be rendered as
* * forms.as_ul
* * forms.as_p
* * forms.as_table (table tag needs to surround the form table elements)
* Field by field form element display
```html
{{ pizzaform.topping1.label_tag  }}
{{ pizzaform.topping1 }}
{{ pizzaform.topping1.errors }}
```