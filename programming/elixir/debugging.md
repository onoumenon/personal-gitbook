# typespecs, behaviours, debugging

## TLDR

you can define types with typespecs, and create custom types

```text
defmodule Person do
   @typedoc """
   A 4 digit year, e.g. 1984
   """
   @type year :: integer

   @spec current_age(year) :: integer
   def current_age(year_of_birth), do: # implementation
end
```

You can implement a behavior if two functions are essentially doing the same sorta thing:



```text
defmodule Parser do
  @doc """
  Parses a string.
  """
  @callback parse(String.t) :: {:ok, term} | {:error, String.t}

  @doc """
  Lists all supported file extensions.
  """
  @callback extensions() :: [String.t]
end
```



```text
defmodule JSONParser do
  @behaviour Parser

  @impl Parser
  def parse(str), do: {:ok, "some json " <> str} # ... parse JSON

  @impl Parser
  def extensions, do: ["json"]
end
```

#### debug

`IO.inspect` for console.log equivalent

`require IEx; IEx.pry` for prying into the code, where execution stops until `continue`

`:debugger.start()` for visual debugger, `observer.start()` for more details

### Typespecs

Elixir is a dynamically typed language, so all types in Elixir are checked at runtime. Nonetheless, Elixir comes with **typespecs**, which are a notation used for:

1. declaring typed function signatures \(also called specifications\);
2. declaring custom types.

`round/1`’s typed signature is written as:

```text
round(number()) :: integer()
```

The syntax is to put the function and its input on the left side of the `::` and the return value’s type on the right side. Be aware that types _may_ omit parentheses.

In code, function specs are written with the `@spec` attribute, typically placed immediately before the function definition. Specs can describe both public and private functions. The function name and the number of arguments used in the `@spec` attribute must match the function it describes.

Elixir supports compound types as well. For example, a list of integers has type `[integer]`, or maps that define keys and types \(see the example below\).



#### Defining custom types <a id="defining-custom-types"></a>

Defining custom types can help communicate the intention of your code and increase its readability. Custom types can be defined within modules via the `@type` attribute.

A simple example of a custom type implementation is to provide a more descriptive alias of an existing type. For example, defining `year` as a type makes your function specs more descriptive than if they had simply used `integer`:

```text
defmodule Person do
   @typedoc """
   A 4 digit year, e.g. 1984
   """
   @type year :: integer

   @spec current_age(year) :: integer
   def current_age(year_of_birth), do: # implementation
end
```



```text
@type error_map :: %{
   message: String.t,
   line_number: integer
}
```



Custom types defined through `@type` are exported and are available outside the module they’re defined in:

```text
defmodule QuietCalculator do
  @spec add(number, number) :: number
  def add(x, y), do: make_quiet(LousyCalculator.add(x, y))

  @spec make_quiet(LousyCalculator.number_with_remark) :: number
  defp make_quiet({num, _remark}), do: num
end
```

#### Static code analysis <a id="static-code-analysis"></a>

Typespecs are not only useful to developers as additional documentation. The Erlang tool [Dialyzer](http://www.erlang.org/doc/man/dialyzer.html), for example, uses typespecs in order to perform static analysis of code. That’s why, in the `QuietCalculator` example, we wrote a spec for the `make_quiet/1` function even though it was defined as a private function.  


### Behaviours <a id="behaviours"></a>

Many modules share the same public API. Take a look at [Plug](https://github.com/elixir-lang/plug), which, as its description states, is a **specification** for composable modules in web applications. Each _plug_ is a module which **has to** implement at least two public functions: `init/1` and `call/2`.

Behaviours provide a way to:

* define a set of functions that have to be implemented by a module;
* ensure that a module implements all the functions in that set.



**Defining behaviours**

Say we want to implement a bunch of parsers, each parsing structured data: for example, a JSON parser and a MessagePack parser. Each of these two parsers will _behave_ the same way: both will provide a `parse/1` function and an `extensions/0` function. The `parse/1` function will return an Elixir representation of the structured data, while the `extensions/0` function will return a list of file extensions that can be used for each type of data \(e.g., `.json` for JSON files\).

We can create a `Parser` behaviour:

```text
defmodule Parser do
  @doc """
  Parses a string.
  """
  @callback parse(String.t) :: {:ok, term} | {:error, String.t}

  @doc """
  Lists all supported file extensions.
  """
  @callback extensions() :: [String.t]
end
```

Modules adopting the `Parser` behaviour will have to implement all the functions defined with the `@callback` attribute. As you can see, `@callback` expects a function name but also a function specification like the ones used with the `@spec` attribute we saw above. Also note that the `term` type is used to represent the parsed value. In Elixir, the `term` type is a shortcut to represent any type.  
Adopting behaviours

Adopting a behaviour is straightforward:

```text
defmodule JSONParser do
  @behaviour Parser

  @impl Parser
  def parse(str), do: {:ok, "some json " <> str} # ... parse JSON

  @impl Parser
  def extensions, do: ["json"]
end
```

```text
defmodule YAMLParser do
  @behaviour Parser

  @impl Parser
  def parse(str), do: {:ok, "some yaml " <> str} # ... parse YAML

  @impl Parser
  def extensions, do: ["yml"]
end
```

If a module adopting a given behaviour doesn’t implement one of the callbacks required by that behaviour, a compile-time warning will be generated.



#### Dynamic dispatch <a id="dynamic-dispatch"></a>

Behaviours are frequently used with dynamic dispatching. For example, we could add a `parse!` function to the `Parser` module that dispatches to the given implementation and returns the `:ok` result or raises in cases of `:error`:

```text
defmodule Parser do
  @callback parse(String.t) :: {:ok, term} | {:error, String.t}
  @callback extensions() :: [String.t]

  def parse!(implementation, contents) do
    case implementation.parse(contents) do
      {:ok, data} -> data
      {:error, error} -> raise ArgumentError, "parsing error: #{error}"
    end
  end
end
```

### IO.inspect/2 <a id="ioinspect2"></a>

What makes `IO.inspect(item, opts \\ [])` really useful in debugging is that it returns the `item` argument passed to it without affecting the behavior of the original code. Let’s see an example.

```text
(1..10)
|> IO.inspect
|> Enum.map(fn x -> x * 2 end)
|> IO.inspect
|> Enum.sum
|> IO.inspect
```

Prints:

```text
1..10
[2, 4, 6, 8, 10, 12, 14, 16, 18, 20]
110
```



```text
[1, 2, 3]
|> IO.inspect(label: "before")
|> Enum.map(&(&1 * 2))
|> IO.inspect(label: "after")
|> Enum.sum
```

Prints:

```text
before: [1, 2, 3]
after: [2, 4, 6]
```

It is also very common to use `IO.inspect/2` with [`binding()`](https://hexdocs.pm/elixir/Kernel.html#binding/0), which returns all variable names and their values:

```text
def some_fun(a, b, c) do
  IO.inspect binding()
  ...
end
```

### `IEx.pry/0` and `IEx.break!/2` <a id="iexpry0-and-iexbreak2"></a>

While `IO.inspect/2` is static, Elixir’s interactive shell provides more dynamic ways to interact with debugged code.

The first one is with [`IEx.pry/0`](https://hexdocs.pm/iex/IEx.html#pry/0) which we can use instead of `IO.inspect binding()`:

```text
def some_fun(a, b, c) do
  require IEx; IEx.pry
  ...
end
```

Once the code above is executed inside an `iex` session, IEx will ask if we want to pry into the current code. If accepted, we will be able to access all variables, as well as imports and aliases from the code, directly From IEx. While pry is running, the code execution stops, until `continue` is called. Remember you can always run `iex` in the context of a project with `iex -S mix TASK`.

Unfortunately, similar to `IO.inspect/2`, `IEx.pry/0` also requires us to change the code we intend to debug. Luckily IEx also provides a [`break!/2`](https://hexdocs.pm/iex/IEx.html#break!/2) function which allows you set and manage breakpoints on any Elixir code without modifying its source:  


### Debugger <a id="debugger"></a>

For those who enjoy breakpoints but are rather interested in a visual debugger, Erlang/OTP ships with a graphical debugger conveniently named `:debugger`. Let’s define a module in a file named `example.ex`:

```text
defmodule Example do
  def double_sum(x, y) do
    hard_work(x, y)
  end

  defp hard_work(x, y) do
    x = 2 * x
    y = 2 * y

    x + y
  end
end
```

Now let’s compile the file and run an IEx session:

```text
$ elixirc example.ex
$ iex
```

Then start the debugger:

```text
iex(1)> :debugger.start()
{:ok, #PID<0.87.0>}
iex(2)> :int.ni(Example)
{:module, Example}
iex(3)> :int.break(Example, 3)
:ok
iex(4)> Example.double_sum(1, 2)
```

