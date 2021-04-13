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

Protocols are a mechanism to achieve polymorphism in Elixir when you want behavior to vary depending on the data type. We are already familiar with one way of solving this type of problem: via pattern matching and guard clauses.

However, this code could be problematic if it were shared as a dependency by multiple apps because there would be no easy way to extend its functionality.

This is where protocols can help us: protocols allow us to extend the original behavior for as many data types as we need. That’s because **dispatching on a protocol is available to any data type that has implemented the protocol** and a protocol can be implemented by anyone, at any time.



```text
defprotocol Utility do
  @spec type(t) :: String.t()
  def type(value)
end

defimpl Utility, for: BitString do
  def type(_value), do: "string"
end

defimpl Utility, for: Integer do
  def type(_value), do: "integer"
end
```

We define the protocol using `defprotocol` - its functions and specs may look similar to interfaces or abstract base classes in other languages. We can add as many implementations as we like using `defimpl`. The output is exactly the same as if we had a single module with multiple functions:

```text
iex> Utility.type("foo")
"string"
iex> Utility.type(123)
"integer"
```

Functions defined in a protocol may have more than one input, but the **dispatching will always be based on the data type of the first input**.



It’s possible to implement protocols for all Elixir data types:

* `Atom`
* `BitString`
* `Float`
* `Function`
* `Integer`
* `List`
* `Map`
* `PID`
* `Port`
* `Reference`
* `Tuple`

### Protocols and structs <a id="protocols-and-structs"></a>

The power of Elixir’s extensibility comes when protocols and structs are used together.

In the [previous chapter](https://elixir-lang.org/getting-started/structs.html), we have learned that although structs are maps, they do not share protocol implementations with maps.



If desired, you could come up with your own semantics for the size of your struct. Not only that, you could use structs to build more robust data types, like queues, and implement all relevant protocols, such as `Enumerable` and possibly `Size`, for this data type.

```text
defmodule User do
  defstruct [:name, :age]
end

defimpl Size, for: User do
  def size(_user), do: 2
end
```



### Implementing `Any` <a id="implementing-any"></a>

Manually implementing protocols for all types can quickly become repetitive and tedious. In such cases, Elixir provides two options: we can explicitly derive the protocol implementation for our types or automatically implement the protocol for all types. In both cases, we need to implement the protocol for `Any`.

#### Deriving <a id="deriving"></a>

Elixir allows us to derive a protocol implementation based on the `Any` implementation. Let’s first implement `Any` as follows:

```text
defimpl Size, for: Any do
  def size(_), do: 0
end

```

The implementation above is arguably not a reasonable one. For example, it makes no sense to say that the size of a `PID` or an `Integer` is `0`.

However, should we be fine with the implementation for `Any`, in order to use such implementation we would need to tell our struct to explicitly derive the `Size` protocol:

```text
defmodule OtherUser do
  @derive [Size]
  defstruct [:name, :age]
end
```

When deriving, Elixir will implement the `Size` protocol for `OtherUser` based on the implementation provided for `Any`.



#### Fallback to `Any` <a id="fallback-to-any"></a>

Another alternative to `@derive` is to explicitly tell the protocol to fallback to `Any` when an implementation cannot be found. This can be achieved by setting `@fallback_to_any` to `true` in the protocol definition:

```text
defprotocol Size do
  @fallback_to_any true
  def size(data)
end
```

As we said in the previous section, the implementation of `Size` for `Any` is not one that can apply to any data type. That’s one of the reasons why `@fallback_to_any` is an opt-in behaviour. For the majority of protocols, raising an error when a protocol is not implemented is the proper behaviour. That said, assuming we have implemented `Any` as in the previous section:

```text
defimpl Size, for: Any do
  def size(_), do: 0
end
```

Which technique is best between deriving and falling back to any depends on the use case but, given Elixir developers prefer explicit over implicit, you may see many libraries pushing towards the `@derive` approach.  


### Built-in protocols <a id="built-in-protocols"></a>

Elixir ships with some built-in protocols. In previous chapters, we have discussed the `Enum` module which provides many functions that work with any data structure that implements the `Enumerable` protocol:

```text
iex> Enum.map [1, 2, 3], fn(x) -> x * 2 end
[2, 4, 6]
iex> Enum.reduce 1..3, 0, fn(x, acc) -> x + acc end
6
```

Another useful example is the `String.Chars` protocol, which specifies how to convert a data structure to its human representation as a string. It’s exposed via the `to_string` function:

```text
iex> to_string :hello
"hello"
```



When there is a need to “print” a more complex data structure, one can use the `inspect` function, based on the `Inspect` protocol:

```text
iex> "tuple: #{inspect tuple}"
"tuple: {1, 2, 3}"
```

The `Inspect` protocol is the protocol used to transform any data structure into a readable textual representation. This is what tools like IEx use to print results:

```text
iex> {1, 2, 3}
{1, 2, 3}
iex> %User{}
%User{name: "john", age: 27}
```

