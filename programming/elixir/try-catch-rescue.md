# try catch rescue

### TLDR <a href="errors" id="errors"></a>

not as used in elixir due to 'fail fast' philosophy. can be useful for 3rd party APIs not playing by the rules.

### Errors <a href="errors" id="errors"></a>

Errors (or _exceptions_) are used when exceptional things happen in the code. A sample error can be retrieved by trying to add a number into an atom:

```
iex> :foo + 1
** (ArithmeticError) bad argument in arithmetic expression
     :erlang.+(:foo, 1)
```

A runtime error can be raised any time by using `raise/1`:

```
iex> raise "oops"
** (RuntimeError) oops
```

Other errors can be raised with `raise/2` passing the error name and a list of keyword arguments:

```
iex> raise ArgumentError, message: "invalid argument foo"
** (ArgumentError) invalid argument foo
```



```
iex> defmodule MyError do
iex>   defexception message: "default message"
iex> end
iex> raise MyError
** (MyError) default message
iex> raise MyError, message: "custom message"
** (MyError) custom message
```

Errors can be **rescued** using the `try/rescue` construct:

```
iex> try do
...>   raise "oops"
...> rescue
...>   e in RuntimeError -> e
...> end
%RuntimeError{message: "oops"}
```



In practice, however, Elixir developers rarely use the `try/rescue` construct. For example, many languages would force you to rescue an error when a file cannot be opened successfully. Elixir instead provides a `File.read/1` function which returns a tuple containing information about whether the file was opened successfully:

```
iex> File.read("hello")
{:error, :enoent}
iex> File.write("hello", "world")
:ok
iex> File.read("hello")
{:ok, "world"}
```

In case you want to handle multiple outcomes of opening a file, you can use pattern matching within the `case` construct:

```
iex> case File.read("hello") do
...>   {:ok, body} -> IO.puts("Success: #{body}")
...>   {:error, reason} -> IO.puts("Error: #{reason}")
...> end
```



For the cases where you do expect a file to exist (and the lack of that file is truly an _error_) you may use `File.read!/1`:

```
iex> File.read!("unknown")
** (File.Error) could not read file unknown: no such file or directory
    (elixir) lib/file.ex:272: File.read!/1
```

{% hint style="info" %}
In Elixir, we avoid using `try/rescue` because **we don’t use errors for control flow**. We take errors literally: they are reserved for unexpected and/or exceptional situations. In case you actually need flow control constructs, _throws_ should be used.
{% endhint %}

### Throws <a href="throws" id="throws"></a>

In Elixir, a value can be thrown and later be caught. `throw` and `catch` are reserved for situations where it is not possible to retrieve a value unless by using `throw` and `catch`.

Those situations are quite uncommon in practice except when interfacing with libraries that do not provide a proper API.&#x20;

let’s imagine the `Enum` module did not provide any API for finding a value and that we needed to find the first multiple of 13 in a list of numbers:

```
iex> try do
...>   Enum.each(-50..50, fn(x) ->
...>     if rem(x, 13) == 0, do: throw(x)
...>   end)
...>   "Got nothing"
...> catch
...>   x -> "Got #{x}"
...> end
"Got -39"
```



### Exits <a href="exits" id="exits"></a>

All Elixir code runs inside processes that communicate with each other. When a process dies of “natural causes” (e.g., unhandled exceptions), it sends an `exit` signal. A process can also die by explicitly sending an `exit` signal:

```
iex> spawn_link(fn -> exit(1) end)
** (EXIT from #PID<0.56.0>) evaluator process exited with reason: 1
```

`exit` signals are an important part of the fault tolerant system provided by the Erlang VM. Processes usually run under supervision trees which are themselves processes that listen to `exit` signals from the supervised processes. Once an `exit` signal is received, the supervision strategy kicks in and the supervised process is restarted.

It is exactly this supervision system that makes constructs like `try/catch` and `try/rescue` so uncommon in Elixir. Instead of rescuing an error, we’d rather “fail fast” since the supervision tree will guarantee our application will go back to a known initial state after the error.

### After <a href="after" id="after"></a>

Sometimes it’s necessary to ensure that a resource is cleaned up after some action that could potentially raise an error. The `try/after` construct allows you to do that. For example, we can open a file and use an `after` clause to close it–even if something goes wrong:

```
iex> {:ok, file} = File.open("sample", [:utf8, :write])
iex> try do
...>   IO.write(file, "olá")
...>   raise "oops, something went wrong"
...> after
...>   File.close(file)
...> end
** (RuntimeError) oops, something went wrong
```

The `after` clause will be executed regardless of whether or not the tried block succeeds. Note, however, that if a linked process exits, this process will exit and the `after` clause will not get run. Thus `after` provides only a soft guarantee.&#x20;

Files and other resources like ETS tables, sockets, ports will close if the current process crashes.

Sometimes you may want to wrap the entire body of a function in a `try` construct, often to guarantee some code will be executed afterwards. In such cases, Elixir allows you to omit the `try` line:

```
iex> defmodule RunAfter do
...>   def without_even_trying do
...>     raise "oops"
...>   after
...>     IO.puts "cleaning up!"
...>   end
...> end
iex> RunAfter.without_even_trying
cleaning up!
** (RuntimeError) oops
```

Elixir will automatically wrap the function body in a `try` whenever one of `after`, `rescue` or `catch` is specified.



### Else <a href="else" id="else"></a>

If an `else` block is present, it will match on the results of the `try` block whenever the `try` block finishes without a throw or an error.

```
iex> x = 2
2
iex> try do
...>   1 / x
...> rescue
...>   ArithmeticError ->
...>     :infinity
...> else
...>   y when y < 1 and y > -1 ->
...>     :small
...>   _ ->
...>     :large
...> end
:small
```



```
iex> try do
...>   raise "fail"
...>   what_happened = :did_not_raise
...> rescue
...>   _ -> what_happened = :rescued
...> end
iex> what_happened
** (RuntimeError) undefined function: what_happened/0
```

Instead, you can store the value of the `try` expression:

```
iex> what_happened =
...>   try do
...>     raise "fail"
...>     :did_not_raise
...>   rescue
...>     _ -> :rescued
...>   end
iex> what_happened
:rescued
```
