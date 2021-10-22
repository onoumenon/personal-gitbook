# Sigils

## TLDR

`~r` for regular expressions

`~r(regex)` delimiter for double/ single quotes without escaping

### Regular expressions <a href="regular-expressions" id="regular-expressions"></a>

The most common sigil in Elixir is `~r`, which is used to create [regular expressions](https://en.wikipedia.org/wiki/Regular\_Expressions):

```
# A regular expression that matches strings which contain "foo" or "bar":
iex> regex = ~r/foo|bar/
~r/foo|bar/
iex> "foo" =~ regex
true
iex> "bat" =~ regex
false
```



### Strings, char lists, and word lists sigils <a href="strings-char-lists-and-word-lists-sigils" id="strings-char-lists-and-word-lists-sigils"></a>

Besides regular expressions, Elixir ships with three other sigils.

#### Strings <a href="strings" id="strings"></a>

The `~s` sigil is used to generate strings, like double quotes are. The `~s` sigil is useful when a string contains double quotes:

```
iex> ~s(this is a string with "double" quotes, not 'single' ones)
"this is a string with \"double\" quotes, not 'single' ones"
```

#### Char lists <a href="char-lists" id="char-lists"></a>

The `~c` sigil is useful for generating char lists that contain single quotes:

```
iex> ~c(this is a char list containing 'single quotes')
'this is a char list containing \'single quotes\''
```

#### Word lists <a href="word-lists" id="word-lists"></a>

The `~w` sigil is used to generate lists of words (_words_ are just regular strings). Inside the `~w` sigil, words are separated by whitespace.

```
iex> ~w(foo bar bat)
["foo", "bar", "bat"]
```



### Interpolation and escaping in string sigils <a href="interpolation-and-escaping-in-string-sigils" id="interpolation-and-escaping-in-string-sigils"></a>

Elixir supports some sigil variants to deal with escaping characters and interpolation. In particular, uppercase letters sigils do not perform interpolation nor escaping. For example, although both `~s` and `~S` will return strings, the former allows escape codes and interpolation while the latter does not:

```
iex> ~s(String with escape codes \x26 #{"inter" <> "polation"})
"String with escape codes & interpolation"
iex> ~S(String without escape codes \x26 without #{interpolation})
"String without escape codes \\x26 without \#{interpolation}"
```



By using `~S`, this problem can be avoided altogether:

```
@doc ~S"""
Converts double-quotes to single-quotes.

## Examples

    iex> convert("\"foo\"")
    "'foo'"

"""
def convert(...)
```



### Calendar sigils <a href="calendar-sigils" id="calendar-sigils"></a>

Elixir offers several sigils to deal with various flavors of times and dates.

#### Date <a href="date" id="date"></a>

A [%Date{}](https://hexdocs.pm/elixir/Date.html) struct contains the fields `year`, `month`, `day`, and `calendar`. You can create one using the `~D` sigil:

```
iex> d = ~D[2019-10-31]
~D[2019-10-31]
iex> d.day
31
```



#### Time <a href="time" id="time"></a>

The [%Time{}](https://hexdocs.pm/elixir/Time.html) struct contains the fields `hour`, `minute`, `second`, `microsecond`, and `calendar`. You can create one using the `~T` sigil:

```
iex> t = ~T[23:00:07.0]
~T[23:00:07.0]
iex> t.second
7
```

#### NaiveDateTime <a href="naivedatetime" id="naivedatetime"></a>

The [%NaiveDateTime{}](https://hexdocs.pm/elixir/NaiveDateTime.html) struct contains fields from both `Date` and `Time`. You can create one using the `~N` sigil:

```
iex> ndt = ~N[2019-10-31 23:00:07]
~N[2019-10-31 23:00:07]
```

Why is it called naive? Because it does not contain timezone information. Therefore, the given datetime may not exist at all or it may exist twice in certain timezones - for example, when we move the clock back and forward for daylight saving time.



#### UTC DateTime <a href="utc-datetime" id="utc-datetime"></a>

A [%DateTime{}](https://hexdocs.pm/elixir/DateTime.html) struct contains the same fields as a `NaiveDateTime` with the addition of fields to track timezones. The `~U` sigil allows developers to create a DateTime in the UTC timezone:

```
iex> dt = ~U[2019-10-31 19:59:03Z]
~U[2019-10-31 19:59:03Z]
iex> %DateTime{minute: minute, time_zone: time_zone} = dt
~U[2019-10-31 19:59:03Z]
iex> minute
59
iex> time_zone
"Etc/UTC"
```

### Custom sigils <a href="custom-sigils" id="custom-sigils"></a>

As hinted at the beginning of this chapter, sigils in Elixir are extensible. In fact, using the sigil `~r/foo/i` is equivalent to calling `sigil_r` with a binary and a char list as the argument:

```
iex> sigil_r(<<"foo">>, 'i')
~r"foo"i
```

We can access the documentation for the `~r` sigil via `sigil_r`:

```
iex> h sigil_r
...
```

We can also provide our own sigils by implementing functions that follow the `sigil_{identifier}` pattern. For example, let’s implement the `~i` sigil that returns an integer (with the optional `n` modifier to make it negative):

```
iex> defmodule MySigils do
...>   def sigil_i(string, []), do: String.to_integer(string)
...>   def sigil_i(string, [?n]), do: -String.to_integer(string)
...> end
iex> import MySigils
iex> ~i(13)
13
iex> ~i(42)n
-42
```



### Custom sigils <a href="custom-sigils" id="custom-sigils"></a>

As hinted at the beginning of this chapter, sigils in Elixir are extensible. In fact, using the sigil `~r/foo/i` is equivalent to calling `sigil_r` with a binary and a char list as the argument:

```
iex> sigil_r(<<"foo">>, 'i')
~r"foo"i
```

We can access the documentation for the `~r` sigil via `sigil_r`:

```
iex> h sigil_r
...
```

We can also provide our own sigils by implementing functions that follow the `sigil_{identifier}` pattern. For example, let’s implement the `~i` sigil that returns an integer (with the optional `n` modifier to make it negative):

```
iex> defmodule MySigils do
...>   def sigil_i(string, []), do: String.to_integer(string)
...>   def sigil_i(string, [?n]), do: -String.to_integer(string)
...> end
iex> import MySigils
iex> ~i(13)
13
iex> ~i(42)n
-42
```
