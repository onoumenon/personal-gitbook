# Basic types, basic operators

## TLDR:

{% hint style="info" %}
`and` `or` `not` if boolean is expected \(stricter\)

`&&` `||` `!` otherwise

use " " for utf binary string and ' ' for char list

mathematical operators are similar except % returns float

use div\(\) or rem\(\) instead if you need the integer results

dot notation to call anonymous fn:

```text
add = fn a, b -> a + b end
add.(1, 2)
```

different data types can be sorted:

```text
1 < :atom
true
```

`++ (concat)` and `-- (subtract) to manipulate lists`
{% endhint %}

```text
iex> 1          # integer
iex> 0x1F       # integer
iex> 1.0        # float
iex> true       # boolean
iex> :atom      # atom / symbol
iex> "elixir"   # string
iex> [1, 2, 3]  # list
iex> {1, 2, 3}  # tuple
```

```text
iex> 10 / 2
5.0
```

In Elixir, the operator `/` always returns a float. If you want to do integer division or get the division remainder, you can invoke the `div` and `rem` functions:

```text
iex> div(10, 2)
5
iex> div 10, 2
5
iex> rem 10, 3
1
iex> rem 9, 3
0
```

Can drop parenthesis but elixir developers generally prefer to use parentheses.

```text
iex> 1.0e-10
1.0e-10
```

`e` for scientific notation

 `round` function to get the closest integer to a given float, or the `trunc` function to get the integer part of a float.

```text
iex> round(3.58)
4
iex> trunc(3.58)
3
```

## Documentation

The arity of a function describes the number of arguments that the function takes. From this point on we will use both the function name and its arity to describe functions throughout the documentation. `trunc/1` identifies the function which is named `trunc` and takes `1` argument, whereas `trunc/2` identifies a different \(nonexistent\) function with the same name but with an arity of `2`.

`h function/arity` for definition

```text
iex> h trunc/1
                             def trunc()

Returns the integer part of number.
```

## Boolean

```text
iex> is_boolean(true)
true
iex> is_boolean(1)
false
```



## Atoms

An atom is a constant whose value is its own name. Some other languages call these symbols. They are often useful to enumerate over distinct values, such as:

```text
iex> :apple
:apple
iex> :orange
:orange
iex> :watermelon
:watermelon
```

Atoms are equal if their names are equal.

```text
iex> :apple == :apple
true
iex> :apple == :orange
false
```

Often they are used to express the state of an operation, by using values such as `:ok` and `:error`.

The booleans `true` and `false` are also atoms:

```text
iex> true == :true
true
iex> is_atom(false)
true
iex> is_boolean(:false)
true
```

Elixir allows you to skip the leading `:` for the atoms `false`, `true` and `nil`.  


Finally, Elixir has a construct called aliases which we will explore later. Aliases start in upper case and are also atoms:

```text
iex> is_atom(Hello)
true
```



### Strings <a id="strings"></a>

Strings in Elixir are delimited by double quotes, and they are encoded in UTF-8:

```text
iex> "hellö"
"hellö"
```

Notice that the `IO.puts/1` function returns the atom `:ok` after printing.  


```text
iex> byte_size("hellö")
6
```

Notice that the number of bytes in that string is 6, even though it has 5 graphemes. That’s because the grapheme “ö” takes 2 bytes to be represented in UTF-8.

 We can get the actual length of the string, based on the number of graphemes, by using the `String.length/1` function:

```text
iex> String.length("hellö")
5
```

  


### Anonymous functions <a id="anonymous-functions"></a>

Elixir also provides anonymous functions. Anonymous functions allow us to store and pass executable code around as if it was an integer or a string. They are delimited by the keywords `fn` and `end`:

```text
iex> add = fn a, b -> a + b end
#Function<12.71889879/2 in :erl_eval.expr/5>
iex> add.(1, 2)
3
iex> is_function(add)
true
```

We can invoke anonymous functions by passing arguments to it. Note that a dot \(`.`\) between the variable and parentheses is required to invoke an anonymous function. The dot ensures there is no ambiguity between calling the anonymous function matched to a variable `add` and a named function `add/2`. 

Finally, anonymous functions can also access variables that are in scope when the function is defined. This is typically refered to as closures, as they close over their scope. Let’s define a new anonymous function that uses the `add` anonymous function we have previously defined:

```text
iex> double = fn a -> add.(a, a) end
#Function<6.71889879/1 in :erl_eval.expr/5>
iex> double.(2)
4
```

variable assigned inside a function does not affect outside its scope

```text
iex> x = 42
42
iex> (fn -> x = 0 end).()
0
iex> x
42
```

### \(Linked\) Lists <a id="linked-lists"></a>

can be of mixed types:

```text
iex> [1, 2, true, 3]
[1, 2, true, 3]
iex> length [1, 2, 3]
3
```

can be concatenated or subtracted using the `++/2` and `--/2` operators respectively:

```text
iex> [1, 2, 3] ++ [4, 5, 6]
[1, 2, 3, 4, 5, 6]
iex> [1, true, 2, false, 3, true] -- [true, false]
[1, 2, 3, true]
```

{% hint style="warning" %}
List operators never modify the existing list.
{% endhint %}

Concatenating to or removing elements from a list returns a new list. We say that Elixir data structures are _immutable_. One advantage of immutability is that it leads to clearer code. You can freely pass the data around with the guarantee no one will mutate it in memory - only transform it.

head and tail of list:

```text
iex> list = [1, 2, 3]
iex> hd(list)
1
iex> tl(list)
[2, 3]
```

Getting the head or the tail of an empty list throws an error:

```text
iex> hd([])
** (ArgumentError) argument error
```



Sometimes you will create a list and it will return a value in single quotes. For example:

```text
iex> [11, 12, 13]
'\v\f\r'
iex> [104, 101, 108, 108, 111]
'hello'
```



When Elixir sees a list of printable ASCII numbers, Elixir will print that as a charlist \(literally a list of characters\). Charlists are quite common when interfacing with existing Erlang code. Whenever you see a value in IEx and you are not quite sure what it is, you can use the `i/1` to retrieve information about it:

```text
iex> i 'hello'
Term
  'hello'
Data type
  List
Description
  ...
Raw representation
  [104, 101, 108, 108, 111]
Reference modules
  List
Implemented protocols
  ...
```

{% hint style="warning" %}
Keep in mind single-quoted and double-quoted representations are not equivalent in Elixir 
{% endhint %}

as they are represented by different types:

```text
iex> 'hello' == "hello"
false
```

Single quotes are charlists, double quotes are strings. We will talk more about them in the [“Binaries, strings and charlists”](https://elixir-lang.org/getting-started/binaries-strings-and-char-lists.html) chapter.



### Tuples <a id="tuples"></a>

Elixir uses curly brackets to define tuples. Like lists, tuples can hold any value:

```text
iex> {:ok, "hello"}
{:ok, "hello"}
iex> tuple_size {:ok, "hello"}
2
```

Tuples store elements contiguously in memory. This means accessing a tuple element by index or getting the tuple size is a fast operation. Indexes start from zero:

```text
iex> tuple = {:ok, "hello"}
{:ok, "hello"}
iex> elem(tuple, 1)
"hello"
iex> tuple_size(tuple)
2
```

It is also possible to put an element at a particular index in a tuple with `put_elem/3`:

```text
iex> tuple = {:ok, "hello"}
{:ok, "hello"}
iex> put_elem(tuple, 1, "world")
{:ok, "world"}
iex> tuple
{:ok, "hello"}
```

Notice that `put_elem/3` returned a new tuple. The original tuple stored in the `tuple` variable was not modified. Like lists, tuples are also immutable. Every operation on a tuple returns a new tuple, it never changes the given one.



### Lists or tuples? <a id="lists-or-tuples"></a>

What is the difference between lists and tuples?

Lists are stored in memory as linked lists, we need to traverse the whole list in order to figure out its size.

The performance of list concatenation depends on the length of the left-hand list:

```text
iex> list = [1, 2, 3]

# This is fast as we only need to traverse `[0]` to prepend to `list`
iex> [0] ++ list
[0, 1, 2, 3]

# This is slow as we need to traverse `list` to append 4
iex> list ++ [4]
[1, 2, 3, 4]
```



Tuples are stored contiguously in memory. This means getting the tuple size or accessing an element by index is fast. However, updating or adding elements to tuples is expensive because it requires creating a new tuple in memory:

```text
iex> tuple = {:a, :b, :c, :d}
iex> put_elem(tuple, 2, :e)
{:a, :b, :e, :d}
```

Note that this applies only to the tuple itself, not its contents. When you update a tuple, all entries are shared between the old and the new tuple, except for the entry that has been replaced. 

Those performance characteristics dictate the usage of those data structures. One very common use case for tuples is to use them to return extra information from a function. For example, `File.read/1` is a function that can be used to read file contents. It returns a tuple:

```text
iex> File.read("path/to/existing/file")
{:ok, "... contents ..."}
iex> File.read("path/to/unknown/file")
{:error, :enoent}
```

If the path given to `File.read/1` exists, it returns a tuple with the atom `:ok` as the first element and the file contents as the second. Otherwise, it returns a tuple with `:error` and the error description.

When counting the elements in a data structure, Elixir also abides by a simple rule: the function is named `size` if the operation is in constant time \(i.e. the value is pre-calculated\) or `length` if the operation is linear.

## Basic operators

 `+`, `-`, `*`, `/` as arithmetic operators, plus the functions `div/2` and `rem/2` for integer division and remainder.

Elixir also provides `++` and `--` to manipulate lists

```text
iex> [1, 2, 3] ++ [4, 5, 6]
[1, 2, 3, 4, 5, 6]
iex> [1, 2, 3] -- [2]
[1, 3]
```

String concatenation is done with `<>`:

```text
iex> "foo" <> "bar"
"foobar"
```



Elixir also provides three boolean operators: `or`, `and` and `not`. These operators are strict in the sense that they expect something that evaluates to a boolean \(`true` or `false`\) as their first argument:

```text
iex> true and true
true
iex> false or is_atom(:example)
true
```

Providing a non-boolean will raise an exception:

```text
iex> 1 and true
** (BadBooleanError) expected a boolean on left-side of "and", got: 1
```

`or` and `and` are short circuit operators

```text
iex> false and raise("This error will never be raised")
false
iex> true or raise("This error will never be raised")
true
```



Besides these boolean operators, Elixir also provides `||`, `&&` and `!` which accept arguments of any type. For these operators, all values except `false` and `nil` will evaluate to true:

```text
# or
iex> 1 || true
1
iex> false || 11
11

# and
iex> nil && 13
nil
iex> true && 17
17

# not
iex> !true
false
iex> !1
false
iex> !nil
true
```

{% hint style="info" %}
`and` `or` `not` if boolean is expected \(stricter\)

`&&` `||` `!` otherwise
{% endhint %}



`==`, `!=`, `===`, `!==`, `<=`, `>=`, `<` and `>` as comparison operators:

```text
iex> 1 == 1
true
iex> 1 != 2
true
iex> 1 < 2
true
```



The difference between `==` and `===` is that the latter is more strict when comparing integers and floats:

```text
iex> 1 == 1.0
true
iex> 1 === 1.0
false
```



In Elixir, we can compare two different data types:

```text
iex> 1 < :atom
true
```

The reason we can compare different data types is pragmatism. Sorting algorithms don’t need to worry about different data types in order to sort. The overall sorting order is defined below:

```text
number < atom < reference < function < port < pid < tuple < map < list < bitstring
```

{% hint style="info" %}
You don’t actually need to memorize this ordering; it’s enough to know that this ordering exists.
{% endhint %}



