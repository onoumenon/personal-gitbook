# Pattern matching, case, cond, and if

## Pattern matching

`=` operator in Elixir is actually a match operator and how to use it to pattern match inside data structures. Finally, we will learn about the pin operator `^` used to access previously bound values.

In Elixir, the `=` operator is actually called _the match operator_. Let’s see why:

```text
iex> x = 1
1
iex> 1 = x
1
iex> 2 = x
** (MatchError) no match of right hand side value: 1
```

A variable can only be assigned on the left side of `=`:

```text
iex> 1 = unknown
** (CompileError) iex:1: undefined function unknown/0
```



The match operator is not only used to match against simple values, but it is also useful for destructuring more complex data types. For example, we can pattern match on tuples:

```text
iex> {a, b, c} = {:hello, "world", 42}
{:hello, "world", 42}
iex> a
:hello
iex> b
"world"
```



A pattern match error will occur if the sides can’t be matched, for example if the tuples have different sizes:

```text
iex> {a, b, c} = {:hello, "world"}
** (MatchError) no match of right hand side value: {:hello, "world"}
```

And also when comparing different types:

```text
iex> {a, b, c} = [:hello, "world", 42]
** (MatchError) no match of right hand side value: [:hello, "world", 42]
```



More interestingly, we can match on specific values. The example below asserts that the left side will only match the right side when the right side is a tuple that starts with the atom `:ok`:

```text
iex> {:ok, result} = {:ok, 13}
{:ok, 13}
iex> result
13

iex> {:ok, result} = {:error, :oops}
** (MatchError) no match of right hand side value: {:error, :oops}
```



We can pattern match on lists:

```text
iex> [a, b, c] = [1, 2, 3]
[1, 2, 3]
iex> a
1
```

A list also supports matching on its own head and tail:

```text
iex> [head | tail] = [1, 2, 3]
[1, 2, 3]
iex> head
1
iex> tail
[2, 3]
```



```text
iex> [head | tail] = []
** (MatchError) no match of right hand side value: []
```

The `[head | tail]` format is not only used on pattern matching but also for prepending items to a list:

```text
iex> list = [1, 2, 3]
[1, 2, 3]
iex> [0 | list]
[0, 1, 2, 3]
```



### The pin operator <a id="the-pin-operator"></a>

Variables in Elixir can be rebound:

```text
iex> x = 1
1
iex> x = 2
2
```



However, there are times when we don’t want variables to be rebound.

Use the pin operator `^` when you want to pattern match against a variable’s _existing value_ rather than rebinding the variable.

```text
iex> x = 1
1
iex> ^x = 2
** (MatchError) no match of right hand side value: 2
```

Because we have pinned `x` when it was bound to the value of `1`, it is equivalent to the following:

```text
iex> 1 = 2
** (MatchError) no match of right hand side value: 2
```

We can use the pin operator inside other pattern matches, such as tuples or lists:

```text
iex> x = 1
1
iex> [^x, 2, 3] = [1, 2, 3]
[1, 2, 3]
iex> {y, ^x} = {2, 1}
{2, 1}
iex> y
2
iex> {y, ^x} = {2, 2}
** (MatchError) no match of right hand side value: {2, 2}
```

 `x` was bound to the value of `1` when it was pinned, this last example is the same as:

```text
iex> {y, 1} = {2, 2}
** (MatchError) no match of right hand side value: {2, 2}
```



If a variable is mentioned more than once in a pattern, all references should bind to the same value:

```text
iex> {x, x} = {1, 1}
{1, 1}
iex> {x, x} = {1, 2}
** (MatchError) no match of right hand side value: {1, 2}
```



In some cases, you don’t care about a particular value in a pattern. It is a common practice to bind those values to the underscore, `_`. For example, if only the head of the list matters to us, we can assign the tail to underscore:

```text
iex> [head | _] = [1, 2, 3]
[1, 2, 3]
iex> head
1
```

The variable `_` is special in that it can never be read from. Trying to read from it gives a compile error:

```text
iex> _
** (CompileError) iex:1: invalid use of _. "_" represents a value to be ignored in a pattern and cannot be used in expressions
```

Although pattern matching allows us to build powerful constructs, its usage is limited. For instance, you cannot make function calls on the left side of a match. The following example is invalid:

```text
iex> length([1, [2], 3]) = 3
** (CompileError) iex:1: cannot invoke remote function :erlang.length/1 inside match
```

## case



`case` allows us to compare a value against many patterns until we find a matching one:

```text
iex> case {1, 2, 3} do
...>   {4, 5, 6} ->
...>     "This clause won't match"
...>   {1, x, 3} ->
...>     "This clause will match and bind x to 2 in this clause"
...>   _ ->
...>     "This clause would match any value"
...> end
"This clause will match and bind x to 2 in this clause"
```



If you want to pattern match against an existing variable, you need to use the `^` operator:

```text
iex> x = 1
1
iex> case 10 do
...>   ^x -> "Won't match"
...>   _ -> "Will match"
...> end
"Will match"
```

clauses \(when\):

```text
iex> case {1, 2, 3} do
...>   {1, x, 3} when x > 0 ->
...>     "Will match"
...>   _ ->
...>     "Would match, if guard condition were not satisfied"
...> end
"Will match"
```

{% hint style="warning" %}
Keep in mind errors in guards do not leak but simply make the guard fail
{% endhint %}

```text
iex> hd(1)
** (ArgumentError) argument error
iex> case 1 do
...>   x when hd(x) -> "Won't match"
...>   x -> "Got #{x}"
...> end
"Got 1"
```

If none of the clauses match, an error is raised:

```text
iex> case :ok do
...>   :error -> "Won't match"
...> end
** (CaseClauseError) no case clause matching: :ok
```



Note anonymous functions can also have multiple clauses and guards:

```text
iex> f = fn
...>   x, y when x > 0 -> x + y
...>   x, y -> x * y
...> end
#Function<12.71889879/2 in :erl_eval.expr/5>
iex> f.(1, 3)
4
iex> f.(-1, 3)
-3
```

The number of arguments in each anonymous function clause needs to be the same, otherwise an error is raised.

```text
iex> f2 = fn
...>   x, y when x > 0 -> x + y
...>   x, y, z -> x * y + z
...> end
** (CompileError) iex:1: cannot mix clauses with different arities in anonymous functions
```

## cond

{% hint style="info" %}
* use cond if you need to check different conditions and find that the first is not nil/ false
* use case if you wanna match against different values
{% endhint %}



```text
iex> cond do
...>   2 + 2 == 5 ->
...>     "This will not be true"
...>   2 * 2 == 3 ->
...>     "Nor this"
...>   1 + 1 == 2 ->
...>     "But this will"
...> end
"But this will"
```



If all of the conditions return `nil` or `false`, an error \(`CondClauseError`\) is raised. For this reason, it may be necessary to add a final condition, equal to `true`, which will always match:

```text
iex> cond do
...>   2 + 2 == 5 ->
...>     "This is never true"
...>   2 * 2 == 3 ->
...>     "Nor this"
...>   true ->
...>     "This is always true (equivalent to else)"
...> end
"This is always true (equivalent to else)"
```

{% hint style="warning" %}
`cond` considers any value besides `nil` and `false` to be true:
{% endhint %}

```text
iex> cond do
...>   hd([1, 2, 3]) ->
...>     "1 is considered as true"
...> end
"1 is considered as true"
```

## if and unless



```text
iex> if true do
...>   "This works!"
...> end
"This works!"
iex> unless true do
...>   "This will never be seen"
...> end
nil
```

If the condition given to `if/2` returns `false` or `nil`, the body given between `do/end` is not executed and instead it returns `nil`. The opposite happens with `unless/2`.

They also support `else` blocks:

```text
iex> if nil do
...>   "This won't be seen"
...> else
...>   "This will"
...> end
"This will"
```

{% hint style="info" %}
Note: An interesting note regarding `if/2` and `unless/2` is that they are implemented as macros in the language; they aren’t special language constructs as they would be in many languages. You can check the documentation and the source of `if/2` in [the `Kernel` module docs](https://hexdocs.pm/elixir/Kernel.html). The `Kernel` module is also where operators like `+/2` and functions like `is_function/2` are defined, all automatically imported and available in your code by default.
{% endhint %}



### `do/end` blocks <a id="doend-blocks"></a>

At this point, we have learned four control structures: `case`, `cond`, `if`, and `unless`, and they were all wrapped in `do/end` blocks. It happens we could also write `if` as follows:

```text
iex> if true, do: 1 + 2
3
```

Notice how the example above has a comma between `true` and `do:`, that’s because it is using Elixir’s regular syntax where each argument is separated by a comma. We say this syntax is using _keyword lists_. We can pass `else` using keywords too:

```text
iex> if false, do: :this, else: :that
:that
```



`do/end` blocks are a syntactic convenience built on top of the keyword ones. That’s why `do/end` blocks do not require a comma between the previous argument and the block. They are useful exactly because they remove the verbosity when writing blocks of code. These are equivalent:

```text
iex> if true do
...>   a = 1 + 2
...>   a + 10
...> end
13
iex> if true, do: (
...>   a = 1 + 2
...>   a + 10
...> )
13
```

