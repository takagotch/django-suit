### django-suit
---
https://django-suit.readthedocs.io/en/develop/

https://djangosuit.com/

```py
from django.db import models
from mptt.fields import TreeForeignKey
from mptt.models import MPTTModel

class Category(MPTTModel):
  name = models.CharField(max_length=64)
  parent = TreeForeignKey('self', null=True, blank=True,
    related_name='children')
    
  order = models.PositiveIntegerField()
  
  class MPTTMeta:
    order_insertion_by = ['order']
    
  def save(self, *args, **kwargs):
    super(Category, self).save(*args, **kwargs)
    Category.objects.rebuild()
    
  def __unicode__(self):
    return self.name


from suit.admin import SortableModelAdmin
from mptt.admin import MPTTModelAdmin
from .models import Category

class CategoryAdmin(MPTTModelAdmink, SortableModelAdmin):
  mptt_level_indent = 20
  list_display = ('name', 'slug', 'is_active')
  list_editable = ('is_active',)

  sortable = 'order'

admin.site.register(Category, CategoryAdmin)

from django.db import models
class Country(models.Model):
  order = models.PositiveIntegerField()
  
from django.contrib.admin import ModelAdmin
from suit.admin import SortableTabularInline

class CountryInline(SortableTabularInline):
  model = Country
  sortable = 'order'

class ContinentAdmin(ModelAdmin):
  inlines = (CountryInline,)

from suit.admin import SortableStackedInline
from suit.admin import SortableTabularStackedInline
from suit.admin import SortableGenericStackedInline
```

```py
suit_form_tabs = (('general', 'General'), ('advanced', 'Advanced Settings'))

from django.contrib import admin
from .models import Country

class CityInline(admin.TabularInline):
  model = City
  suit_classes = 'suit-tab suit-tab-cities'

class CountryAdmin(Admin.ModelAdmin):
  inlines = (CityInline,)
  
  fieldsets = [
    (None, {
      'classes': ('suit-tab', 'suit-tab-general',),
      'fields': ['name', 'continent',]
    }),
    ('Statistics', {
      'classes': ('suit-tab', 'suit-tab-general',),
      'fields': ('area', 'population')}),
    ('Architecture', {
      'classes': ('suit-tab', 'suit-tab-cities'),
      'fields': ['architecture']})
  ]
  
  suit_form_tabs = (('general', 'General'), ('cities', 'Cities'),
    ('info', 'Info on tabs'))
  
  suit_form_includes = (
    ('admin/examples/country/custom_include.html', 'middle', 'cities'),
    ('admin/examples/coutnry/tab_info.html', '', 'info')
  )
  
  suit_form_tabs = (('general', 'General'), ('cities', 'Cities'),
    ('info', 'Info on tabs'))
  
  suit_form_includes = (
    ('admin/examples/country/custom_includes.html', 'middle', 'cities'),
    ('admin/examples/country/tab_info.html', '', 'info')
  )

admin.site.register(Country, CountryAdmin)


from django.contrib import admin
from .models import Country

class CountryAdmin(admin.ModelAdmin):
  suit_form_includes = (
    ('admin/examples/country/custom_includes.html', 'middle', 'cities'),
    ('admin/examples/country/tab_info.html', '', 'info'),
    ('admin/examples/country/disclaimer.html')
  )


from django.contrib.admin import ModelAdmin

class CountryAdmin(ModelAdmin):
  def suit_row_attributes(self, obj, request):
    return {'class': 'type-%s' % obj.type}
    
  def suit_row_attributes(self, obj, request):
    css_class = {
      1: 'success',
      0: 'warning',
      -1: 'error',
    }.get(obj.status)
    if css_class:
      return {'class': css_class, 'data': obj.name}

from django.contrib.admin import ModelAdmin

class CountryAdmin(ModelAdmin):
  def suit_cell_attributes(self, obj, column):
    if column == 'countries':
      return {'class': 'text-center'}
    elif column == 'name' and obj.status == -1:
      return {'class': 'text-error'}

from django.froms import ModelForm, TextInput
from django.contrib.admin import ModelAdmini
from .model import Country

class CountryForm(ModelForm):
  class Meta:
    widgets = {
      'name': TextInput(attrs={'class': 'input-mini'})
    }
class CountryAdmin(ModelAdmin):
  form = CountryForm
admin.site.register(Country, CountryAdmin)

from django.forms import ModelForm
from suit.widgets import NumberInput

class OrderForm(ModelForm):
  class Meta:
    widgets = {
      'count': NumberInput,
      
      'count': NumberInput(attrs={'class': 'input-mini'})
    }

from django.forms import ModelForm
from suit.widgets import HTML5Input
class ProductForm(ModelForm):
  class Meta:
    widgets = {
      'color': HTML5Input(input_type='color'),
    }

from django.forms import ModelForm
from suit.widgets import EnclosedInput

class ProductFrom(ModelFrom):
  class Meta:
    widgets = {
      'discount': EnclosedInput(append='%')
      'size': EnclosedInput(append='m<sup>2</sup>')
      
      'email': EnclosedInput(append='icon-envelope'),
      'user': EnclosedInput(prepend='icon-user'),
      
      'price': EnclosedInput(prepend='$', append='.00'),
      
      'url': EnclosedInput(prepend='icon-home', append='<input type="button" class="btn" value="Open link">'),
    }

from django.forms import ModelForm
from suit.widgets import AutosizedTextarea

class ProductForm(ModelForm):
  class Meta:
    widgets = {
      'description': AutosizedTextarea,
      
      'description': AutosizedTextarea(attrs={'rows': 3, 'class': 'input-xlarge'}),
    }

from djagno.forms import ModelsFrom
from suit.widges import SuitDateWidget, SuitTimeWidget, SuitSplitDataTimeWdiget

class UserChangeForm(ModelForm):
  class Meta:
    model = User
    widgets = {
      'last_login': SuitSplitDataTimeWidget,
      'date_joined': SuitSplitDataTimeWidget,
    }

from django.forms import ModelForm
from suit.widgets import LinkedSelect
class CountryForm(UserChangeForm):
  class Meta:
    widgets = {
      'content': LinkedSelect
    }
```

```css
.country-column .text, .country-column a {
  text-align: center;
}
```


