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

{% embed url="https://stackoverflow.com/questions/42743825/how-to-create-groups-and-assign-permission-during-project-setup-in-django" %}

{% code title="kyc/apps.py" %}
```
from django.apps import AppConfig
from django.db.models.signals import post_migrate

from genbase.management import assign_group_permissions

class KycConfig(AppConfig):
    name = 'kyc'

    def ready(self):
        post_migrate.connect(assign_group_permissions(
            ProfileRole.CORPORATE_SECRETARY.value,
            [
                'kyc.add_kyc',
                'kyc.delete_kyc',
                'kyc.view_kyc'
            ],
        ))
```
{% endcode %}

{% code title="genbase/management.py" %}
```
from django import apps as global_apps
from django.db.models import Q

def assign_group_permissions(group_name, permissions):
    def receiver(*args, apps=global_apps, **kwargs):
        try:
            Group = apps.get_model('auth', 'Group')
            Permission = apps.get_model('auth', 'Permission')
        except LookupError:
            return

        perm_q = Q()
        for perm in permissions:
            app_label, codename = perm.split('.')
            perm_q |= Q(content_type__app_label=app_label) & Q(codename=codename)

        group, _ = Group.objects.get_or_create(name=group_name)
        group.permissions.add(
            *Permission.objects.filter(perm_q)
        )

    return receiver

```
{% endcode %}
