# I/O, file system, require, import, alias

## TLDR:

`IO.puts` and `IO.gets` for standard I/O (:stdio)

`File.read` to read file

Use `Path` module to provide paths for `File` operations

Prefer passing binary (double-quoted strings) to `IO` as lists are accepted but encode differently based on settings passed in.

`alias` `require` `import` are lexically scoped

```
# Alias the module so it can be called as Bar instead of Foo.Bar
# Note that it 'overrides' Elixir modules, so if alias Math.List,
# calling original List methods will be Elixir.List 
alias Foo.Bar, as: Bar
alias MyApp.{Foo, Bar, Baz}

# Require the module in order to use its macros
require Foo

# Import functions from Foo so they can be called without the `Foo.` prefix
# :only is optional, but recommended
# prefer alias over import
import Foo, only: [duplicate: 2]

# Invokes the custom code defined in Foo as an extension point
# prefer alias and import over use
use Foo
```

### The `IO` module <a href="the-io-module" id="the-io-module"></a>

The [`IO`](https://hexdocs.pm/elixir/IO.html) module is the main mechanism in Elixir for reading and writing to standard input/output (`:stdio`), standard error (`:stderr`), files, and other IO devices. Usage of the module is pretty straightforward:

```
iex> IO.puts("hello world")
hello world
:ok
iex> IO.gets("yes or no? ")
yes or no? yes
"yes\n"
```

### The `File` module <a href="the-file-module" id="the-file-module"></a>

The [`File`](https://hexdocs.pm/elixir/File.html) module contains functions that allow us to open files as IO devices. By default, files are opened in binary mode, which requires developers to use the specific `IO.binread/2` and `IO.binwrite/2` functions from the `IO` module:

```
iex> {:ok, file} = File.open("hello", [:write])
{:ok, #PID<0.47.0>}
iex> IO.binwrite(file, "world")
:ok
iex> File.close(file)
:ok
iex> File.read("hello")
{:ok, "world"}
```

A file can also be opened with `:utf8` encoding, which tells the `File` module to interpret the bytes read from the file as UTF-8-encoded bytes.

Besides functions for opening, reading and writing files, the `File` module has many functions to work with the file system. Those functions are named after their UNIX equivalents.

```
iex> File.read("hello")
{:ok, "world"}
iex> File.read!("hello")
"world"
iex> File.read("unknown")
{:error, :enoent}
iex> File.read!("unknown")
** (File.Error) could not read file "unknown": no such file or directory
```

Notice that the version with `!` returns the contents of the file instead of a tuple, and if anything goes wrong the function raises an error.

The version without `!` is preferred when you want to handle different outcomes using pattern matching:

```
case File.read(file) do
  {:ok, body}      -> # do something with the `body`
  {:error, reason} -> # handle the error caused by `reason`
end
```

However, if you expect the file to be there, the bang variation is more useful as it raises a meaningful error message. Avoid writing:

```
{:ok, body} = File.read(file)
```

as, in case of an error, `File.read/1` will return `{:error, reason}` and the pattern matching will fail. You will still get the desired result (a raised error), but the message will be about the pattern which doesn’t match (thus being cryptic in respect to what the error actually is about).

Therefore, if you don’t want to handle the error outcomes, prefer using `File.read!/1`.



### The `Path` module <a href="the-path-module" id="the-path-module"></a>

The majority of the functions in the `File` module expect paths as arguments. Most commonly, those paths will be regular binaries. The [`Path`](https://hexdocs.pm/elixir/Path.html) module provides facilities for working with such paths:

```
iex> Path.join("foo", "bar")
"foo/bar"
iex> Path.expand("~/hello")
"/Users/jose/hello"
```



### Processes <a href="processes" id="processes"></a>

You may have noticed that `File.open/2` returns a tuple like `{:ok, pid}`:

```
iex> {:ok, file} = File.open("hello", [:write])
{:ok, #PID<0.47.0>}
```

That happens because the `IO` module actually works with processes (see [chapter 11](https://elixir-lang.org/getting-started/processes.html)). Given a file is a process, when you write to a file that has been closed, you are actually sending a message to a process which has been terminated:

```
iex> File.close(file)
:ok
iex> IO.write(file, "is anybody out there")
{:error, :terminated}
```

Let’s see in more detail what happens when you request `IO.write(pid, binary)`. The `IO` module sends a message to the process identified by `pid` with the desired operation. A small ad-hoc process can help us see it:

```
iex> pid = spawn(fn ->
...>  receive do: (msg -> IO.inspect msg)
...> end)
#PID<0.57.0>
iex> IO.write(pid, "hello")
{:io_request, #PID<0.41.0>, #Reference<0.0.8.91>,
 {:put_chars, :unicode, "hello"}}
** (ErlangError) erlang error: :terminated
```

After `IO.write/2`, we can see the request sent by the `IO` module printed out (a four-elements tuple). Soon after that, we see that it fails since the `IO` module expected some kind of result, which we did not supply.

### `iodata` and `chardata` <a href="iodata-and-chardata" id="iodata-and-chardata"></a>

In all of the examples above, we used binaries when writing to files. In the chapter [“Binaries, strings, and charlists”](https://elixir-lang.org/getting-started/binaries-strings-and-char-lists.html), we mentioned how strings are made of bytes while charlists are lists with Unicode codepoints.

The functions in `IO` and `File` also allow lists to be given as arguments. Not only that, they also allow a mixed list of lists, integers, and binaries to be given:

```
iex> IO.puts('hello world')
hello world
:ok
iex> IO.puts(['hello', ?\s, "world"])
hello world
:ok
```

However, using lists in IO operations requires some attention. A list may represent either a bunch of bytes or a bunch of characters and which one to use depends on the encoding of the IO device. If the file is opened without encoding, the file is expected to be in raw mode, and the functions in the `IO` module starting with `bin*` must be used. Those functions expect an `iodata` as an argument; i.e., they expect a list of integers representing bytes or binaries to be given.

On the other hand, `:stdio` and files opened with `:utf8` encoding work with the remaining functions in the `IO` module. Those functions expect a `chardata` as an argument, that is, a list of characters or strings.

Although this is a subtle difference, you only need to worry about these details if you intend to pass lists to those functions. Binaries are already represented by the underlying bytes and as such their representation is always “raw”.

### Alias, require, import

```
# Alias the module so it can be called as Bar instead of Foo.Bar
alias Foo.Bar, as: Bar

# Require the module in order to use its macros
require Foo

# Import functions from Foo so they can be called without the `Foo.` prefix
import Foo

# Invokes the custom code defined in Foo as an extension point
use Foo
```

Imagine a module uses a specialized list implemented in `Math.List`. The `alias` directive allows referring to `Math.List` just as `List` within the module definition:

```
defmodule Stats do
  alias Math.List, as: List
  # In the remaining module definition List expands to Math.List.
end
```

The original `List` can still be accessed within `Stats` by the fully-qualified name `Elixir.List`.

Aliases are frequently used to define shortcuts. In fact, calling `alias` without an `:as` option sets the alias automatically to the last part of the module name, for example:

```
alias Math.List
```

Is the same as:

```
alias Math.List, as: List
```

Note that `alias` is **lexically scoped**, which allows you to set aliases inside specific functions:

```
defmodule Math do
  def plus(a, b) do
    alias Math.List
    # ...
  end

  def minus(a, b) do
    # ...
  end
end
```

In the example above, since we are invoking `alias` inside the function `plus/2`, the alias will be valid only inside the function `plus/2`. `minus/2` won’t be affected at all.

### require <a href="require" id="require"></a>

Elixir provides macros as a mechanism for meta-programming (writing code that generates code). Macros are expanded at compile time.

Public functions in modules are globally available, but in order to use macros, you need to opt-in by requiring the module they are defined in.

```
iex> Integer.is_odd(3)
** (CompileError) iex:1: you must require Integer before invoking the macro Integer.is_odd/1
    (elixir) src/elixir_dispatch.erl:97: :elixir_dispatch.dispatch_require/6
iex> require Integer
Integer
iex> Integer.is_odd(3)
true
```

In Elixir, `Integer.is_odd/1` is defined as a macro so that it can be used as a guard. This means that, in order to invoke `Integer.is_odd/1`, we need to first require the `Integer` module.

Note that like the `alias` directive, `require` is also lexically scoped. We will talk more about macros in a later chapter.

### use



The `use` macro is frequently used as an extension point. This means that, when you `use` a module `FooBar`, you allow that module to inject _any_ code in the current module, such as importing itself or other modules, defining new functions, setting a module state, etc.

For example, in order to write tests using the ExUnit framework, a developer should use the `ExUnit.Case` module:

```
defmodule AssertionTest do
  use ExUnit.Case, async: true

  test "always pass" do
    assert true
  end
end
```



Generally speaking, the following module:

```
defmodule Example do
  use Feature, option: :value
end
```

is compiled into

```
defmodule Example do
  require Feature
  Feature.__using__(option: :value)
end
```

Since `use` allows any code to run, we can’t really know the side-effects of using a module without reading its documentation. Therefore use this function with care and only if strictly required. Don’t use `use` where an `import` or `alias` would do.

### What is an Alias?

```
iex> is_atom(String)
true
iex> to_string(String)
"Elixir.String"
iex> :"Elixir.String" == String
true
```

An alias in Elixir is a capitalized identifier (like `String`, `Keyword`, etc) which is converted to an atom during compilation. For instance, the `String` alias translates by default to the atom `:"Elixir.String"`:

### Module nesting <a href="module-nesting" id="module-nesting"></a>

Now that we have talked about aliases, we can talk about nesting and how it works in Elixir. Consider the following example:

```
defmodule Foo do
  defmodule Bar do
  end
end
```

The example above will define two modules: `Foo` and `Foo.Bar`. The second can be accessed as `Bar` inside `Foo` as long as they are in the same lexical scope.

If, later, the `Bar` module is moved outside the `Foo` module definition, it must be referenced by its full name (`Foo.Bar`) or an alias must be set using the `alias` directive discussed above.\




**Note**: in Elixir, you don’t have to define the `Foo` module before being able to define the `Foo.Bar` module, as they are effectively independent. The above could also be written as:

```
defmodule Foo.Bar do
end

defmodule Foo do
  alias Foo.Bar
  # Can still access it as `Bar`
end
```



### Multi alias/import/require/use <a href="multi-aliasimportrequireuse" id="multi-aliasimportrequireuse"></a>

It is possible to alias, import or require multiple modules at once. This is particularly useful once we start nesting modules, which is very common when building Elixir applications. For example, imagine you have an application where all modules are nested under `MyApp`, you can alias the modules `MyApp.Foo`, `MyApp.Bar` and `MyApp.Baz` at once as follows:

```
alias MyApp.{Foo, Bar, Baz}
```
