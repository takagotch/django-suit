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

```
```

```
```


