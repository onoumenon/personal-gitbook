# Enumerables, streams

## TLDR:

focus on the `Enum` module first and only move to `Stream` for the particular scenarios where laziness is required, to either deal with slow resources or large, possibly infinite, collections.

## Enums

Elixir also provides ranges:

```
iex> Enum.map(1..3, fn x -> x * 2 end)
[2, 4, 6]
iex> Enum.reduce(1..3, 0, &+/2)
6
```

The functions in the Enum module are limited to, as the name says, enumerating values in data structures. For specific operations, like inserting and updating particular elements, you may need to reach for modules specific to the data type. For example, if you want to insert an element at a given position in a list, you should use the `List.insert_at/3` function from [the `List` module](https://hexdocs.pm/elixir/List.html), as it would make little sense to insert a value into, for example, a range.

We say the functions in the `Enum` module are polymorphic because they can work with diverse data types. In particular, the functions in the `Enum` module can work with any data type that implements [the `Enumerable` protocol](https://hexdocs.pm/elixir/Enumerable.html).

### Eager vs Lazy <a href="eager-vs-lazy" id="eager-vs-lazy"></a>

All the functions in the `Enum` module are eager. This means that when performing multiple operations with `Enum`, each operation is going to generate an intermediate list until we reach the result:

```
iex> 1..100_000 |> Enum.map(&(&1 * 3)) |> Enum.filter(odd?) |> Enum.sum()
7500000000
```

### The pipe operator <a href="the-pipe-operator" id="the-pipe-operator"></a>

The `|>` symbol used in the snippet above is the **pipe operator**: it takes the output from the expression on its left side and passes it as the first argument to the function call on its right side. It’s similar to the Unix `|` operator. Its purpose is to highlight the data being transformed by a series of functions. To see how it can make the code cleaner, have a look at the example above rewritten without using the `|>` operator:

```
iex> Enum.sum(Enum.filter(Enum.map(1..100_000, &(&1 * 3)), odd?))
7500000000
```



&#x20;[the `Stream` module](https://hexdocs.pm/elixir/Stream.html) which supports lazy operations:

```
iex> 1..100_000 |> Stream.map(&(&1 * 3)) |> Stream.filter(odd?) |> Enum.sum
7500000000
```

Streams are lazy, composable enumerables.

In the example above, `1..100_000 |> Stream.map(&(&1 * 3))` returns a data type, an actual stream, that represents the `map` computation over the range `1..100_000`:

```
iex> 1..100_000 |> Stream.map(&(&1 * 3))
#Stream<[enum: 1..100000, funs: [#Function<34.16982430/1 in Stream.map/2>]]>
```

Furthermore, they are composable because we can pipe many stream operations:

```
iex> 1..100_000 |> Stream.map(&(&1 * 3)) |> Stream.filter(odd?)
#Stream<[enum: 1..100000, funs: [...]]>
```

Instead of generating intermediate lists, streams build a series of computations that are invoked only when we pass the underlying stream to the `Enum` module. Streams are useful when working with large, _possibly infinite_, collections.\


Many functions in the `Stream` module accept any enumerable as an argument and return a stream as a result. It also provides functions for creating streams. For example, `Stream.cycle/1` can be used to create a stream that cycles a given enumerable infinitely. Be careful to not call a function like `Enum.map/2` on such streams, as they would cycle forever:

```
iex> stream = Stream.cycle([1, 2, 3])
#Function<15.16982430/2 in Stream.unfold/2>
iex> Enum.take(stream, 10)
[1, 2, 3, 1, 2, 3, 1, 2, 3, 1]
```



Another interesting function is `Stream.resource/3` which can be used to wrap around resources, guaranteeing they are opened right before enumeration and closed afterwards, even in the case of failures. For example, `File.stream!/1` builds on top of `Stream.resource/3` to stream files:

```
iex> stream = File.stream!("path/to/file")
%File.Stream{
  line_or_bytes: :line,
  modes: [:raw, :read_ahead, :binary],
  path: "path/to/file",
  raw: true
}
iex> Enum.take(stream, 10)
```
