# Structs, Protocols

## TLDR:

`defstruct key: value` in module

Structs are extensions built on top of maps that provide compile-time checks and default values.

same syntax as Map to update/ access

nil is default value if no default is specified, and implicit `nil` values must come first

`@enforce_keys` to enforce keys

### Defining structs <a id="defining-structs"></a>

To define a struct, the `defstruct` construct is used:

```text
iex> defmodule User do
...>   defstruct name: "John", age: 27
...> end
```

The keyword list used with `defstruct` defines what fields the struct will have along with their default values.

Structs take the name of the module they’re defined in. In the example above, we defined a struct named `User`.

you can compile the file inside IEx before proceeding by running `c "file.exs"`; be aware you may get an error saying `the struct was not yet defined` if you try the below example in a file directly due to when definitions are resolved\):

```text
iex> %User{}
%User{age: 27, name: "John"}
iex> %User{name: "Jane"}
%User{age: 27, name: "Jane"}
```

Structs provide _compile-time_ guarantees that only the fields \(and _all_ of them\) defined through `defstruct` will be allowed to exist in a struct:

```text
iex> %User{oops: :field}
** (KeyError) key :oops not found in: %User{age: 27, name: "John"}
```



### Accessing and updating structs <a id="accessing-and-updating-structs"></a>

When we discussed maps, we showed how we can access and update the fields of a map. The same techniques \(and the same syntax\) apply to structs as well:

```text
iex> john = %User{}
%User{age: 27, name: "John"}
iex> john.name
"John"
iex> jane = %{john | name: "Jane"}
%User{age: 27, name: "Jane"}
iex> %{jane | oops: :field}
** (KeyError) key :oops not found in: %User{age: 27, name: "Jane"}
```

### Structs are bare maps underneath <a id="structs-are-bare-maps-underneath"></a>

In the example above, pattern matching works because underneath structs are bare maps with a fixed set of fields. As maps, structs store a “special” field named `__struct__` that holds the name of the struct:

```text
iex> is_map(john)
true
iex> john.__struct__
User
```

Notice that we referred to structs as **bare** maps because none of the protocols implemented for maps are available for structs. For example, you can neither enumerate nor access a struct:



However, since structs are just maps, they work with the functions from the `Map` module:

```text
iex> jane = Map.put(%User{}, :name, "Jane")
%User{age: 27, name: "Jane"}
iex> Map.merge(jane, %User{name: "John"})
%User{age: 27, name: "John"}
iex> Map.keys(jane)
[:__struct__, :age, :name]
```

Structs alongside protocols provide one of the most important features for Elixir developers: data polymorphism



### Default values and required keys <a id="default-values-and-required-keys"></a>

If you don’t specify a default key value when defining a struct, `nil` will be assumed:

```text
iex> defmodule Product do
...>   defstruct [:name]
...> end
iex> %Product{}
%Product{name: nil}
```



You can define a structure combining both fields with explicit default values, and implicit `nil` values. In this case you must first specify the fields which implicitly default to nil:

```text
iex> defmodule User do
...>   defstruct [:email, name: "John", age: 27]
...> end
iex> %User{}
%User{age: 27, email: nil, name: "John"}
```

Doing it in reverse order will raise a syntax error:

```text
iex> defmodule User do
...>   defstruct [name: "John", age: 27, :email]
...> end
** (SyntaxError) iex:107: syntax error before: email
```



You can also enforce that certain keys have to be specified when creating the struct:

```text
iex> defmodule Car do
...>   @enforce_keys [:make]
...>   defstruct [:model, :make]
...> end
iex> %Car{}
** (ArgumentError) the following keys must also be given when building struct Car: [:make]
    expanding struct: Car.__struct__/1
```

## Protocols



