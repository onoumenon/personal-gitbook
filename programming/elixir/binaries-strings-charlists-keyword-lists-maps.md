# Binaries, strings, charlists, keyword lists, maps



{% embed url="https://elixir-lang.org/getting-started/binaries-strings-and-char-lists.html" %}

{% embed url="https://mortoray.com/2013/11/27/the-string-type-is-broken/" %}

## **TLDR**

* double quote strings are utf-8 binary \(preferred\)
* single quote strings are a list of chars



```text
'hello' ++ ' ' ++ 'world'
```

* Double quoted string, is a UTF-8 encoded ‘binary’, aka a series of bytes. Their concatenation is done via ‘&lt;&gt;’.

```text
"hello" <> " " <> "world"
```

functions to convert

```text
to_charlist("hello")
to_string('hello')to_string('hello')
```

{% embed url="https://medium.com/@junwan01/elixir-strings-in-single-quote-vs-double-quote-c0a61d3e5acd" %}

## Single quote

To understand strings in Elixir, we have to educate ourselves about [Unicode](https://en.wikipedia.org/wiki/Unicode) and character encodings, specifically the [UTF-8](https://en.wikipedia.org/wiki/UTF-8) encoding

Unicode organizes all of the characters in its repertoire into code charts, and each character is given a unique numerical index. This numerical index is known as a [Code Point](https://en.wikipedia.org/wiki/Code_point).

In Elixir you can use a `?` in front of a character literal to reveal its code point:

```text
iex> ?a
97
iex> ?ł
322
```

Whereas the code point is **what** we store, an encoding deals with **how** we store it: encoding is an implementation. In other words, we need a mechanism to convert the code point numbers into bytes so they can be stored in memory, written to disk, etc.

Elixir uses UTF-8 to encode its strings, which means that code points are encoded as a series of 8-bit bytes. UTF-8 is a **variable width** character encoding that uses one to four bytes to store each code point; it is capable of encoding all valid Unicode code points.

Besides defining characters, UTF-8 also provides a notion of graphemes. Graphemes may consist of multiple characters that are often perceived as one. For example, `é` can be represented in Unicode as a single character. It can also be represented as the combination of the character `e` and the acute accent character `´` into a single grapheme.

```text
iex> string = "hełło"
"hełło"
iex> String.length(string)
5
iex> byte_size(string)
7
```

see the inner binary representation of a string is to concatenate the null byte `<<0>>` to it:

```text
iex> "hełło" <> <<0>>
<<104, 101, 197, 130, 197, 130, 111, 0>>
```

### Bitstrings <a id="bitstrings"></a>

Although we have covered code points and UTF-8 encoding, we still need to go a bit deeper into how exactly we store the encoded bytes, and this is where we introduce the **bitstring**. A bitstring is a fundamental data type in Elixir, denoted with the `<<>>` syntax. **A bitstring is a contiguous sequence of bits in memory.**

\*\*\*\*

```text
iex> <<42>> === <<42::8>>
true
iex> <<3::4>>
<<3::size(4)>>
```

For example, the decimal number `3` when represented with 4 bits in base 2 would be `0011`, which is equivalent to the values `0`, `0`, `1`, `1`, each stored using 1 bit:

```text
iex> <<0::1, 0::1, 1::1, 1::1>> == <<3::4>>
true
```

Any value that exceeds what can be stored by the number of bits provisioned is truncated:

```text
iex> <<1>> === <<257>>
true
```

Here, 257 in base 2 would be represented as `100000001`, but since we have reserved only 8 bits for its representation \(by default\), the left-most bit is ignored and the value becomes truncated to `00000001`, or simply `1` in decimal.



### Binaries <a id="binaries"></a>

**A binary is a bitstring where the number of bits is divisible by 8.** That means that every binary is a bitstring, but not every bitstring is a binary. We can use the `is_bitstring/1` and `is_binary/1` functions to demonstrate this.

```text
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



Note that unless you explicitly use `::` modifiers, each entry in the binary pattern is expected to match a single byte \(exactly 8 bits\). If we want to match on a binary of unknown size, we can use the `binary` modifier at the end of the pattern:

```text
iex> <<0, 1, x::binary>> = <<0, 1, 2, 3>>
<<0, 1, 2, 3>>
iex> x
<<2, 3>>
```



Given that strings are binaries, we can also pattern match on strings:

```text
iex> <<head, rest::binary>> = "banana"
"banana"
iex> head == ?b
true
iex> rest
"anana"
```

However, remember that binary pattern matching works on _bytes_, so matching on the string like “über” with multibyte characters won’t match on the _character_, it will match on the _first byte of that character_:

```text
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

```text
iex> <<x::utf8, rest::binary>> = "über"
"über"
iex> x == ?ü
true
iex> rest
"ber"
```

### Charlists <a id="charlists"></a>

Our tour of our bitstrings, binaries, and strings is nearly complete, but we have one more data type to explain: the charlist.

{% hint style="info" %}
**A charlist is a list of integers where all the integers are valid code points.** In practice, you will not come across them often, except perhaps when interfacing with Erlang, in particular when using older libraries that do not accept binaries as arguments.
{% endhint %}



Whereas strings \(i.e. binaries\) are created using double-quotes, charlists are created with single-quoted literals:

```text
iex> 'hello'
'hello'
iex> [?h, ?e, ?l, ?l, ?o]
'hello'
```

You can see that instead of containing bytes, a charlist contains integer code points. However, the list is only printed in single-quotes if all code points are within the ASCII range:

```text
iex> 'hełło'
[104, 101, 322, 322, 111]
iex> is_list('hełło')
true
```



Interpreting integers as code points may lead to some surprising behavior. For example, if you are storing a list of integers that happen to range between 0 and 127, by default IEx will interpret this as a charlist and it will display the corresponding ASCII characters.

```text
iex> heartbeats_per_minute = [99, 97, 116]
'cat'
```

Note that those functions are polymorphic - not only do they convert charlists to strings, they also operate on integers, atoms, and so on.

String \(binary\) concatenation uses the `<>` operator but charlists, being lists, use the list concatenation operator `++`:

```text
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



