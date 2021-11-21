# module attributes

## TLDR:

Module attributes in Elixir serve three purposes:

1. They serve to annotate the module, often with information to be used by the user or the VM.
2. They work as constants.
3. They work as a temporary module storage to be used during compilation.

### Annotations

* `@moduledoc` - provides documentation for the current module.
* `@doc` - provides documentation for the function or macro that follows the attribute.
* `@behaviour` - (notice the British spelling) used for specifying an OTP or user-defined behaviour.
* `@before_compile` - provides a hook that will be invoked before the module is compiled. This makes it possible to inject functions inside the module exactly before compilation.

Functions may be called when defining a module attribute, but will be called at compile time, not run time.

* good for pre-computing constants but not for runtime values
* when attribute is read in a function, a snapshot is made of the current value
* reading the same attribute multiple times create multiple copies
* if the attribute is cheap to compute, prefer computing it in the function

### As annotations <a href="as-annotations" id="as-annotations"></a>

Elixir brings the concept of module attributes from Erlang. For example:

```
defmodule MyServer do
  @moduledoc "My server code."
end
```

In the example above, we are defining the module documentation by using the module attribute syntax. Elixir has a handful of reserved attributes. Here are a few of them, the most commonly used ones:

* `@moduledoc` - provides documentation for the current module.
* `@doc` - provides documentation for the function or macro that follows the attribute.
* `@behaviour` - (notice the British spelling) used for specifying an OTP or user-defined behaviour.
* `@before_compile` - provides a hook that will be invoked before the module is compiled. This makes it possible to inject functions inside the module exactly before compilation.

`@moduledoc` and `@doc` are by far the most used attributes, and we expect you to use them a lot. Elixir treats documentation as first-class and provides many functions to access documentation. You can read more about [writing documentation in Elixir in our official documentation](https://hexdocs.pm/elixir/writing-documentation.html).



```
defmodule Math do
  @moduledoc """
  Provides math-related functions.

  ## Examples

      iex> Math.sum(1, 2)
      3

  """

  @doc """
  Calculates the sum of two numbers.
  """
  def sum(a, b), do: a + b
end
```



Elixir promotes the use of Markdown with heredocs to write readable documentation. Heredocs are multi-line strings, they start and end with triple double-quotes, keeping the formatting of the inner text. We can access the documentation of any compiled module directly from IEx:

```
$ elixirc math.ex
$ iex
```

```
iex> h Math # Access the docs for the module Math
...
iex> h Math.sum # Access the docs for the sum function
...
```



### As “constants” <a href="as-constants" id="as-constants"></a>

Elixir developers often use module attributes when they wish to make a value more visible or reusable:

```
defmodule MyServer do
  @initial_state %{host: "127.0.0.1", port: 3456}
  IO.inspect @initial_state
end
```

Trying to access an attribute that was not defined will print a warning:

```
defmodule MyServer do
  @unknown
end
warning: undefined module attribute @unknown, please remove access to @unknown or explicitly set it before access
```



Attributes can also be read inside functions:

```
defmodule MyServer do
  @my_data 14
  def first_data, do: @my_data
  @my_data 13
  def second_data, do: @my_data
end

MyServer.first_data #=> 14
MyServer.second_data #=> 13
```

{% hint style="warning" %}
> Note: do not add a newline between the attribute and its value, otherwise Elixir will assume you are reading the value, rather than setting it.
{% endhint %}



Functions may be called when defining a module attribute:

```
defmodule MyApp.Status do
  @service URI.parse("https://example.com")
  def status(email) do
    SomeHttpClient.get(@service)
  end
end
```



The function above will be called at compilation time and its _return value_, not the function call itself, is what will be substituted in for the attribute. So the above will effectively compile to this:

```
defmodule MyApp.Status do
  def status(email) do
    SomeHttpClient.get(%URI{
      authority: "example.com",
      host: "example.com",
      port: 443,
      scheme: "https"
    })
  end
end
```

This can be useful for pre-computing constant values, but it can also cause problems if you’re expecting the function to be called at runtime. For example, if you are reading a value from a database or an environment variable inside an attribute, be aware that it will read that value only at compilation time. Be careful, however: _functions defined in the same module as the attribute itself cannot be called_ because they have not yet been compiled when the attribute is being defined.

Every time an attribute is read inside a function, Elixir takes a snapshot of its current value. Therefore if you read the same attribute multiple times inside multiple functions, you may end-up making multiple copies of it. That’s usually not an issue, but if you are using functions to compute large module attributes, that can slow down compilation. The solution is to move the attribute to shared function. For example, instead of this:

```
def some_function, do: do_something_with(@example)
def another_function, do: do_something_else_with(@example)
```

Prefer this:

```
def some_function, do: do_something_with(example())
def another_function, do: do_something_else_with(example())
defp example, do: @example
```

If `@example` is cheap to compute, it may be even better to skip the module attribute altogether, and compute its value inside the function.

#### Accumulating attributes <a href="accumulating-attributes" id="accumulating-attributes"></a>

Normally, repeating a module attribute will cause its value to be reassigned, but there are circumstances where you may want to [configure the module attribute](https://hexdocs.pm/elixir/Module.html#register\_attribute/3) so that its values are accumulated:

```
defmodule Foo do
  Module.register_attribute __MODULE__, :param, accumulate: true

  @param :foo
  @param :bar
  # here @param == [:bar, :foo]
end
```



### As temporary storage <a href="as-temporary-storage" id="as-temporary-storage"></a>

To see an example of using module attributes as for storage, look no further than Elixir’s unit test framework called [ExUnit](https://hexdocs.pm/ex\_unit/). ExUnit uses module attributes for multiple different purposes:

```
defmodule MyTest do
  use ExUnit.Case, async: true

  @tag :external
  @tag os: :unix
  test "contacts external service" do
    # ...
  end
end
```

In the example above, `ExUnit` stores the value of `async: true` in a module attribute to change how the module is compiled. Tags are also defined as `accumulate: true` attributes, and they store tags that can be used to setup and filter tests. For example, you can avoid running external tests on your machine because they are slow and dependent on other services, while they can still be enabled in your build system.

