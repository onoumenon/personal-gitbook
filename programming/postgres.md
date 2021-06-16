# postgres

{% embed url="https://intellipaat.com/community/64100/how-to-use-homebrew-to-downgrade-postgresql-from-10-1-to-9-6-on-mac-os" %}



_╰─$ createuser postgres_ 

╰─$ psql -U postgres -c "drop database dpms\_01\_latest;"

╰─$ psql -U postgres -c "create database dpms\_01\_latest;"

