# Binaries, strings, charlists, keyword lists, maps



{% embed url="https://elixir-lang.org/getting-started/binaries-strings-and-char-lists.html" %}

{% embed url="https://mortoray.com/2013/11/27/the-string-type-is-broken/" %}

{% embed url="https://dashbit.co/blog/writing-assertive-code-with-elixir" %}



## **String TLDR**

* double quote strings are utf-8 binary (preferred)
* single quote strings are a list of chars



```
'hello' ++ ' ' ++ 'world'
```

* Double quoted string, is a UTF-8 encoded ‘binary’, aka a series of bytes. Their concatenation is done via ‘<>’.

```
"hello" <> " " <> "world"
```

functions to convert

```
to_charlist("hello")
to_string('hello')to_string('hello')
```

{% embed url="https://medium.com/@junwan01/elixir-strings-in-single-quote-vs-double-quote-c0a61d3e5acd" %}

## Associative datatypes TLDR:

Keyword lists are used in Elixir mainly for passing optional values. If you need to store many items or guarantee one-key associates with at maximum one-value, you should use maps instead.

```
iex> list == [a: 1, b: 2]
iex> new_list = [a: 0] ++ list
[a: 0, a: 1, b: 2]
iex> new_list[:a]
0
```

Keyword lists are important because they have three special characteristics:

* Keys must be atoms.
* Keys are ordered, as specified by the developer.
* Keys can be given more than once.

Maps:

* Maps allow any value as a key.
* Maps’ keys do not follow any ordering.

```
iex> map = %{:a => 1, 2 => :b}
%{2 => :b, :a => 1}
iex> map[:a]
1
iex> map[2]
:b
iex> map[:c]
nil
iex> %{} = %{:a => 1, 2 => :b}
%{2 => :b, :a => 1}
iex> %{:a => a} = %{:a => 1, 2 => :b}
%{2 => :b, :a => 1}
iex> a
1
iex> %{:c => c} = %{:a => 1, 2 => :b}
** (MatchError) no match of right hand side value: %{2 => :b, :a => 1}
```

If all keys are atoms:&#x20;

```
iex> map = %{a: 1, b: 2}
%{a: 1, b: 2}

iex> map.a
1
iex> map.c
** (KeyError) key :c not found in: %{2 => :b, :a => 1}
```

## Single quote

To understand strings in Elixir, we have to educate ourselves about [Unicode](https://en.wikipedia.org/wiki/Unicode) and character encodings, specifically the [UTF-8](https://en.wikipedia.org/wiki/UTF-8) encoding

Unicode organizes all of the characters in its repertoire into code charts, and each character is given a unique numerical index. This numerical index is known as a [Code Point](https://en.wikipedia.org/wiki/Code\_point).

In Elixir you can use a `?` in front of a character literal to reveal its code point:

```
iex> ?a
97
iex> ?ł
322
```

Whereas the code point is **what** we store, an encoding deals with **how** we store it: encoding is an implementation. In other words, we need a mechanism to convert the code point numbers into bytes so they can be stored in memory, written to disk, etc.

Elixir uses UTF-8 to encode its strings, which means that code points are encoded as a series of 8-bit bytes. UTF-8 is a **variable width** character encoding that uses one to four bytes to store each code point; it is capable of encoding all valid Unicode code points.

Besides defining characters, UTF-8 also provides a notion of graphemes. Graphemes may consist of multiple characters that are often perceived as one. For example, `é` can be represented in Unicode as a single character. It can also be represented as the combination of the character `e` and the acute accent character `´` into a single grapheme.

```
iex> string = "hełło"
"hełło"
iex> String.length(string)
5
iex> byte_size(string)
7
```

see the inner binary representation of a string is to concatenate the null byte `<<0>>` to it:

```
iex> "hełło" <> <<0>>
<<104, 101, 197, 130, 197, 130, 111, 0>>
```

### Bitstrings <a href="bitstrings" id="bitstrings"></a>

Although we have covered code points and UTF-8 encoding, we still need to go a bit deeper into how exactly we store the encoded bytes, and this is where we introduce the **bitstring**. A bitstring is a fundamental data type in Elixir, denoted with the `<<>>` syntax. **A bitstring is a contiguous sequence of bits in memory.**

****

```
iex> <<42>> === <<42::8>>
true
iex> <<3::4>>
<<3::size(4)>>
```

For example, the decimal number `3` when represented with 4 bits in base 2 would be `0011`, which is equivalent to the values `0`, `0`, `1`, `1`, each stored using 1 bit:

```
iex> <<0::1, 0::1, 1::1, 1::1>> == <<3::4>>
true
```

Any value that exceeds what can be stored by the number of bits provisioned is truncated:

```
iex> <<1>> === <<257>>
true
```

Here, 257 in base 2 would be represented as `100000001`, but since we have reserved only 8 bits for its representation (by default), the left-most bit is ignored and the value becomes truncated to `00000001`, or simply `1` in decimal.



### Binaries <a href="binaries" id="binaries"></a>

**A binary is a bitstring where the number of bits is divisible by 8.** That means that every binary is a bitstring, but not every bitstring is a binary. We can use the `is_bitstring/1` and `is_binary/1` functions to demonstrate this.

```
iex> is_bitstring(<<3::4>>)
true
iex> is_binary(<<3::4>>)
false
iex> is_bitstring(<<0, 255, 42>>)
true
iex> is_binary(<<0, 255, 42>>)
true
iex> is_binary(<<42::16>>)
true
```



Note that unless you explicitly use `::` modifiers, each entry in the binary pattern is expected to match a single byte (exactly 8 bits). If we want to match on a binary of unknown size, we can use the `binary` modifier at the end of the pattern:

```
iex> <<0, 1, x::binary>> = <<0, 1, 2, 3>>
<<0, 1, 2, 3>>
iex> x
<<2, 3>>
```



Given that strings are binaries, we can also pattern match on strings:

```
iex> <<head, rest::binary>> = "banana"
"banana"
iex> head == ?b
true
iex> rest
"anana"
```

However, remember that binary pattern matching works on _bytes_, so matching on the string like “über” with multibyte characters won’t match on the _character_, it will match on the _first byte of that character_:

```
iex> "ü" <> <<0>>
<<195, 188, 0>>
iex> <<x, rest::binary>> = "über"
"über"
iex> x == ?ü
false
iex> rest
<<188, 98, 101, 114>>
```

Above, `x` matched on only the first byte of the multibyte `ü` character.

Therefore, when pattern matching on strings, it is important to use the `utf8` modifier:

```
iex> <<x::utf8, rest::binary>> = "über"
"über"
iex> x == ?ü
true
iex> rest
"ber"
```

### Charlists <a href="charlists" id="charlists"></a>

Our tour of our bitstrings, binaries, and strings is nearly complete, but we have one more data type to explain: the charlist.

{% hint style="info" %}
**A charlist is a list of integers where all the integers are valid code points.** In practice, you will not come across them often, except perhaps when interfacing with Erlang, in particular when using older libraries that do not accept binaries as arguments.
{% endhint %}



Whereas strings (i.e. binaries) are created using double-quotes, charlists are created with single-quoted literals:

```
iex> 'hello'
'hello'
iex> [?h, ?e, ?l, ?l, ?o]
'hello'
```

You can see that instead of containing bytes, a charlist contains integer code points. However, the list is only printed in single-quotes if all code points are within the ASCII range:

```
iex> 'hełło'
[104, 101, 322, 322, 111]
iex> is_list('hełło')
true
```



Interpreting integers as code points may lead to some surprising behavior. For example, if you are storing a list of integers that happen to range between 0 and 127, by default IEx will interpret this as a charlist and it will display the corresponding ASCII characters.

```
iex> heartbeats_per_minute = [99, 97, 116]
'cat'
```

Note that those functions are polymorphic - not only do they convert charlists to strings, they also operate on integers, atoms, and so on.

String (binary) concatenation uses the `<>` operator but charlists, being lists, use the list concatenation operator `++`:

```
iex> 'this ' <> 'fails'
** (ArgumentError) expected binary argument in <> operator but got: 'this '
    (elixir) lib/kernel.ex:1821: Kernel.wrap_concatenation/3
    (elixir) lib/kernel.ex:1808: Kernel.extract_concatenations/2
    (elixir) expanding macro: Kernel.<>/2
    iex:1: (file)
iex> 'this ' ++ 'works'
'this works'
iex> "he" ++ "llo"
** (ArgumentError) argument error
    :erlang.++("he", "llo")
iex> "he" <> "llo"
"hello"
```

### Keyword lists <a href="keyword-lists" id="keyword-lists"></a>

In many functional programming languages, it is common to use a list of 2-item tuples as the representation of a key-value data structure. In Elixir, when we have a list of tuples and the first item of the tuple (i.e. the key) is an atom, we call it a keyword list:

```
iex> list = [{:a, 1}, {:b, 2}]
[a: 1, b: 2]
iex> list == [a: 1, b: 2]
true
```

As you can see above, Elixir supports a special syntax for defining such lists: `[key: value]`. Underneath it maps to the same list of tuples as above. Since keyword lists are lists, we can use all operations available to lists. For example, we can use `++` to add new values to a keyword list:\


```
iex> list ++ [c: 3]
[a: 1, b: 2, c: 3]
iex> [a: 0] ++ list
[a: 0, a: 1, b: 2]
```

Note that values added to the front are the ones fetched on lookup:

```
iex> new_list = [a: 0] ++ list
[a: 0, a: 1, b: 2]
iex> new_list[:a]
0
```



For example, [the Ecto library](https://github.com/elixir-lang/ecto) makes use of these features to provide an elegant DSL for writing database queries:

```
query =
  from w in Weather,
    where: w.prcp > 0,
    where: w.temp < 20,
    select: w
```



These characteristics are what prompted keyword lists to be the default mechanism for passing options to functions in Elixir. In chapter 5, when we discussed the `if/2` macro, we mentioned that the following syntax is supported:

```
iex> if false, do: :this, else: :that
:that
```

The `do:` and `else:` pairs form a keyword list! In fact, the call above is equivalent to:

```
iex> if(false, [do: :this, else: :that])
:that
```

keyword lists are used in Elixir mainly for passing optional values. If you need to store many items or guarantee one-key associates with at maximum one-value, you should use maps instead.

### Maps <a href="maps" id="maps"></a>

Whenever you need a key-value store, maps are the “go to” data structure in Elixir. A map is created using the `%{}` syntax:

```
iex> map = %{:a => 1, 2 => :b}
%{2 => :b, :a => 1}
iex> map[:a]
1
iex> map[2]
:b
iex> map[:c]
nil
```

Compared to keyword lists, we can already see two differences:

* Maps allow any value as a key.
* Maps’ keys do not follow any ordering.

In contrast to keyword lists, maps are very useful with pattern matching. When a map is used in a pattern, it will always match on a subset of the given value:

When a map is used in a pattern, it will always match on a subset of the given value:

(ie: merge)

```
iex> %{} = %{:a => 1, 2 => :b}
%{2 => :b, :a => 1}
iex> %{:a => a} = %{:a => 1, 2 => :b}
%{2 => :b, :a => 1}
iex> a
1
iex> %{:c => c} = %{:a => 1, 2 => :b}
** (MatchError) no match of right hand side value: %{2 => :b, :a => 1}
```

Variables can be used when accessing, matching and adding map keys:

```
iex> n = 1
1
iex> map = %{n => :one}
%{1 => :one}
iex> map[n]
:one
iex> %{^n => :one} = %{1 => :one, 2 => :two, 3 => :three}
%{1 => :one, 2 => :two, 3 => :three}
```

[The `Map` module](https://hexdocs.pm/elixir/Map.html) provides a very similar API to the `Keyword` module with convenience functions to manipulate maps:

```
iex> Map.get(%{:a => 1, 2 => :b}, :a)
1
iex> Map.put(%{:a => 1, 2 => :b}, :c, 3)
%{2 => :b, :a => 1, :c => 3}
iex> Map.to_list(%{:a => 1, 2 => :b})
[{2, :b}, {:a, 1}]
```

Updating a map's key value

```
iex> map = %{:a => 1, 2 => :b}
%{2 => :b, :a => 1}

iex> %{map | 2 => "two"}
%{2 => "two", :a => 1}
iex> %{map | :c => 3}
** (KeyError) key :c not found in: %{2 => :b, :a => 1}
```



When all the keys in a map are atoms, you can use the keyword syntax for convenience:

```
iex> map = %{a: 1, b: 2}
%{a: 1, b: 2}
```



Another interesting property of maps is that they provide their own syntax for accessing atom keys:

```
iex> map = %{:a => 1, 2 => :b}
%{2 => :b, :a => 1}

iex> map.a
1
iex> map.c
** (KeyError) key :c not found in: %{2 => :b, :a => 1}
```

### Nested data structures <a href="nested-data-structures" id="nested-data-structures"></a>

Often we will have maps inside maps, or even keywords lists inside maps, and so forth. Elixir provides conveniences for manipulating nested data structures via the `put_in/2`, `update_in/2` and other macros giving the same conveniences you would find in imperative languages while keeping the immutable properties of the language.

Imagine you have the following structure:

```
iex> users = [
  john: %{name: "John", age: 27, languages: ["Erlang", "Ruby", "Elixir"]},
  mary: %{name: "Mary", age: 29, languages: ["Elixir", "F#", "Clojure"]}
]
[
  john: %{age: 27, languages: ["Erlang", "Ruby", "Elixir"], name: "John"},
  mary: %{age: 29, languages: ["Elixir", "F#", "Clojure"], name: "Mary"}
]
```

We have a keyword list of users where each value is a map containing the name, age and a list of programming languages each user likes. If we wanted to access the age for john, we could write:

```
iex> users[:john].age
27
```

It happens we can also use this same syntax for updating the value:

```
iex> users = put_in users[:john].age, 31
[
  john: %{age: 31, languages: ["Erlang", "Ruby", "Elixir"], name: "John"},
  mary: %{age: 29, languages: ["Elixir", "F#", "Clojure"], name: "Mary"}
]
```

The `update_in/2` macro is similar but allows us to pass a function that controls how the value changes. For example, let’s remove “Clojure” from Mary’s list of languages:

```
iex> users = update_in users[:mary].languages, fn languages -> List.delete(languages, "Clojure") end
[
  john: %{age: 31, languages: ["Erlang", "Ruby", "Elixir"], name: "John"},
  mary: %{age: 29, languages: ["Elixir", "F#"], name: "Mary"}
]
```

There is more to learn about `put_in/2` and `update_in/2`, including the `get_and_update_in/2` that allows us to extract a value and update the data structure at once. There are also `put_in/3`, `update_in/3` and `get_and_update_in/3` which allow dynamic access into the data structure. [Check their respective documentation in the `Kernel` module for more information](https://hexdocs.pm/elixir/Kernel.html).\
