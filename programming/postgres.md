# Postgresql commands

{% embed url="https://intellipaat.com/community/64100/how-to-use-homebrew-to-downgrade-postgresql-from-10-1-to-9-6-on-mac-os" %}

Note that mac doesn't have 'default' user postgres

_╰─$ createuser postgres_&#x20;

╰─$ psql -U postgres -c "drop database db\_name;"

╰─$ psql -U postgres -c "create database db\_name;"
