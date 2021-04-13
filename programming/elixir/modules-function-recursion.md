# Modules, function, recursion

## _TLDR:_

normal vs private functions: `def` and `defp`

`//` for default argument

trailing `?` for boolean returns



## _Functions_

In Elixir we group several functions into modules. We’ve already used many different modules in the previous chapters such as [the `String` module](https://hexdocs.pm/elixir/String.html):

```text
iex> String.length("hello")
5
```

In order to create our own modules in Elixir, we use the `defmodule` macro. We use the `def` macro to define functions in that module:

```text
iex> defmodule Math do
...>   def sum(a, b) do
...>     a + b
...>   end
...> end

iex> Math.sum(1, 2)
3
```

### Compilation <a id="compilation"></a>

 `math.ex` with the following contents:

```text
defmodule Math do
  def sum(a, b) do
    a + b
  end
end
```

This file can be compiled using `elixirc`:

```text
$ elixirc math.ex
```

This will generate a file named `Elixir.Math.beam` containing the bytecode for the defined module. If we start `iex` again, our module definition will be available \(provided that `iex` is started in the same directory the bytecode file is in\):

```text
iex> Math.sum(1, 2)
3
```

Elixir projects are usually organized into three directories:

* `_build` - contains compilation artifacts
* `lib` - contains Elixir code \(usually `.ex` files\)
* `test` - contains tests \(usually `.exs` files\)

When working on actual projects, the build tool called `mix` will be responsible for compiling and setting up the proper paths for you

### Scripted mode <a id="scripted-mode"></a>

{% hint style="info" %}
`.ex` files are meant to be compiled while `.exs` files are used for scripting.
{% endhint %}

```text
$ elixir math.exs
```

Because we used `elixir` instead of `elixirc`, the module was compiled and loaded into memory, but no `.beam` file was written to disk

### Named functions <a id="named-functions"></a>

{% hint style="info" %}
`def/2` for normal function

`defp/2`for private functions 
{% endhint %}

```text
defmodule Math do
  def sum(a, b) do
    do_sum(a, b)
  end

  defp do_sum(a, b) do
    a + b
  end
end

IO.puts Math.sum(1, 2)    #=> 3
IO.puts Math.do_sum(1, 2) #=> ** (UndefinedFunctionError)
```



Function declarations also support guards and multiple clauses. If a function has several clauses, Elixir will try each clause until it finds one that matches. Here is an implementation of a function that checks if the given number is zero or not:

```text
defmodule Math do
  def zero?(0) do
    true
  end

  def zero?(x) when is_integer(x) do
    false
  end
end

IO.puts Math.zero?(0)         #=> true
IO.puts Math.zero?(1)         #=> false
IO.puts Math.zero?([1, 2, 3]) #=> ** (FunctionClauseError)
IO.puts Math.zero?(0.0)       #=> ** (FunctionClauseError)
```

_The trailing question mark in `zero?` means that this function returns a boolean; see_ [_Naming Conventions_](https://hexdocs.pm/elixir/master/naming-conventions.html#trailing-question-mark-foo)_._

_The below is the same as above:_

```text
defmodule Math do
  def zero?(0), do: true
  def zero?(x) when is_integer(x), do: false
end
```



```text
iex> Math.zero?(0)
true
iex> fun = &Math.zero?/1
&Math.zero?/1
iex> is_function(fun)
true
iex> fun.(0)
true
```

Remember Elixir makes a distinction between anonymous functions and named functions, where the former must be invoked with a dot \(`.`\) between the variable name and parentheses. The capture operator bridges this gap by allowing named functions to be assigned to variables and passed as arguments in the same way we assign, invoke and pass anonymous functions.



the capture syntax can also be used as a shortcut for creating functions:

```text
iex> fun = &(&1 + 1)
#Function<6.71889879/1 in :erl_eval.expr/5>
iex> fun.(1)
2

iex> fun2 = &"Good #{&1}"
#Function<6.127694169/1 in :erl_eval.expr/5>
iex)> fun2.("morning")
"Good morning"
```

`&1` represents the first argument passed into the function. `&(&1 + 1)` above is exactly the same as `fn x -> x + 1 end`.

### Default arguments \\ <a id="default-arguments"></a>

```text
defmodule Concat do
  def join(a, b, sep \\ " ") do
    a <> sep <> b
  end
end

IO.puts Concat.join("Hello", "world")      #=> Hello world
IO.puts Concat.join("Hello", "world", "_") #=> Hello_world
```

Any expression is allowed to serve as a default value, but it won’t be evaluated during the function definition.



If a function with default values has multiple clauses, it is required to create a function head \(a function definition without a body\) for declaring defaults:

```text
defmodule Concat do
  # A function head declaring defaults
  def join(a, b \\ nil, sep \\ " ")

  def join(a, b, _sep) when is_nil(b) do
    a
  end

  def join(a, b, sep) do
    a <> sep <> b
  end
end

IO.puts Concat.join("Hello", "world")      #=> Hello world
IO.puts Concat.join("Hello", "world", "_") #=> Hello_world
IO.puts Concat.join("Hello")               #=> Hello
```

_The leading underscore in `_sep` means that the variable will be ignored in this function_

\_\_

When using default values, one must be careful to avoid overlapping function definitions. Consider the following example:

```text
defmodule Concat do
  def join(a, b) do
    IO.puts "***First join"
    a <> b
  end

  def join(a, b, sep \\ " ") do
    IO.puts "***Second join"
    a <> sep <> b
  end
end
```

Elixir will emit the following warning:

```text
concat.ex:7: warning: this clause cannot match because a previous clause at line 2 always matches
```

The compiler is telling us that invoking the `join` function with two arguments will always choose the first definition of `join` whereas the second one will only be invoked when three arguments are passed:

```text
$ iex concat.ex
```

```text
iex> Concat.join "Hello", "world"
***First join
"Helloworld"
```

```text
iex> Concat.join "Hello", "world", "_"
***Second join
"Hello_world"
```

Removing the default argument in this case will fix the warning.

### Loops through recursion <a id="loops-through-recursion"></a>

loops in Elixir \(as in any functional programming language\) are written differently from imperative languages. For example, in an imperative language like C, one would write:

```text
for(i = 0; i < sizeof(array); i++) {
  array[i] = array[i] * 2;
}
```

For this reason, functional languages rely on recursion: a function is called recursively until a condition is reached that stops the recursive action from continuing. No data is mutated in this process.

```text
defmodule Recursion do
  def print_multiple_times(msg, n) when n <= 1 do
    IO.puts msg
  end

  def print_multiple_times(msg, n) do
    IO.puts msg
    print_multiple_times(msg, n - 1)
  end
end

Recursion.print_multiple_times("Hello!", 3)
# Hello!
# Hello!
# Hello!
```

### Reduce and map algorithms <a id="reduce-and-map-algorithms"></a>

```text
defmodule Math do
  def sum_list([head | tail], accumulator) do
    sum_list(tail, head + accumulator)
  end

  def sum_list([], accumulator) do
    accumulator
  end
end

IO.puts Math.sum_list([1, 2, 3], 0) #=> 6
```

```text
defmodule Math do
  def double_each([head | tail]) do
    [head * 2 | double_each(tail)]
  end

  def double_each([]) do
    []
  end
end
```



The [`Enum` module](https://hexdocs.pm/elixir/Enum.html), which we’re going to see in the next chapter, already provides many conveniences for working with lists. For instance, the examples above could be written as:

```text
iex> Enum.reduce([1, 2, 3], 0, fn(x, acc) -> x + acc end)
6
iex> Enum.map([1, 2, 3], fn(x) -> x * 2 end)
[2, 4, 6]
```

Or, using the capture syntax:

```text
iex> Enum.reduce([1, 2, 3], 0, &+/2)
6
iex> Enum.map([1, 2, 3], &(&1 * 2))
[2, 4, 6]
```

