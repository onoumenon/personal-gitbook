# Django Groups

{% embed url="https://en.wikipedia.org/wiki/Role-based_access_control" %}

{% embed url="https://cheat.readthedocs.io/en/latest/django/permissions.html" %}

{% embed url="https://stackoverflow.com/questions/18178978/django-assign-perm-w-post-save-signal" %}

{% embed url="https://medium.com/innoventes/django-protect-apis-using-djangomodelpermissions-cb1bf65c5b58" %}

{% embed url="https://eshlox.net/2019/07/27/adding-groups-via-django-db-migration-script-and-assigning-a-group-to-all-existing--users" %}

{% embed url="https://www.py4u.net/discuss/1256468" %}

{% embed url="https://stackoverflow.com/questions/52267646/django-rest-serialize-user-get-all-permissions" %}

TLDR

```
from rest_framework.permissions import (DjangoModelPermissions)
class CustomDjangoModelPermissions(DjangoModelPermissions):
    def __init__(self):
        self.perms_map['GET'] = ['%(app_label)s.view_%       (model_name)s']
```

```python
python manage.py makemigrations users --empty
```

Next, I modified the created file to add proper migration

```
from django.db import migrations


def apply_migration(apps, schema_editor):
    db_alias = schema_editor.connection.alias

    Group = apps.get_model("auth", "Group")
    Group.objects.using(db_alias).bulk_create(
        [Group(name="group1"), Group(name="group2")]
    )
def revert_migration(apps, schema_editor):
    Group = apps.get_model("auth", "Group")
    Group.objects.filter(name__in=["group1", "group2"]).delete()
    
class Migration(migrations.Migration):
    operations = [migrations.RunPython(apply_migration, revert_migration)]
```
