# Phoenix

To create new application:

{% embed url="https://hexdocs.pm/phoenix/up_and_running.html" %}

On Mac, homebrew doesn't set up the default db username as 'postgres', but the system's username so use `whoami` to check:&#x20;

{% embed url="https://elixirforum.com/t/mix-ecto-create-errors-with-role-postgres-does-not-exist/20624" %}

{% embed url="https://hexdocs.pm/phoenix/directory_structure.html" %}

`_build` - compilation artefacts

`assets` - front-end assets

`config` - project configuration

`deps` - a directory with all of our Mix dependencies.&#x20;

`lib` - a directory that holds your application source code

`test` - a directory with all of our application tests. It often mirrors the same structure found in `lib`
