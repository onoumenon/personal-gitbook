# Comprehensions

## TLDR:

* Comprehensions are syntactic sugar for enum loops using `for`

```
for n <- [1, 2, 3, 4], rem(n, 3) == 0, into: "", do: n * n
```

* &#x20;`n <- [1, 2, 3, 4]` is the **generator**.

supports pattern matching, can have multiple:&#x20;

```
for i <- [:a, :b, :c], j <- [1, 2], do:  {i, j}
```

* `rem(n, 3) == 0 ` is the filter

can have multiple:

```
for dir <- dirs,
    file <- File.ls!(dir),
    path = Path.join(dir, file),
    File.regular?(path) do
  File.stat!(path).size
end
```

* `into: ` is the collectable

the result of a comprehension can be inserted into different data structures by passing the `:into` option to the comprehension.

## Comprehensions

Comprehensions are syntactic sugar for such constructs: they group those common tasks into the `for` special form.

For example, we can map a list of integers into their squared values:

```
iex> for n <- [1, 2, 3, 4], do: n * n
[1, 4, 9, 16]
```

A comprehension is made of three parts: generators, filters, and collectables.



### Generators and filters <a href="generators-and-filters" id="generators-and-filters"></a>

In the expression above, `n <- [1, 2, 3, 4]` is the **generator**. It is literally generating values to be used in the comprehension. Any enumerable can be passed on the right-hand side of the generator expression:

```
iex> for n <- 1..4, do: n * n
[1, 4, 9, 16]
```

supports pattern matching:

```
iex> values = [good: 1, good: 2, bad: 3, good: 4]
iex> for {:good, n} <- values, do: n * n
[1, 4, 16]
```

Alternatively to pattern matching, filters can be used to select some particular elements. For example, we can select the multiples of 3 and discard all others:

```
iex> for n <- 0..5, rem(n, 3) == 0, do: n * n
[0, 9]
```

Comprehensions discard all elements for which the filter expression returns `false` or `nil`; all other values are selected.\


Furthermore, comprehensions also allow multiple generators and filters to be given. Here is an example that receives a list of directories and gets the size of each file in those directories:

```
dirs = ['/home/mikey', '/home/james']

for dir <- dirs,
    file <- File.ls!(dir),
    path = Path.join(dir, file),
    File.regular?(path) do
  File.stat!(path).size
end
```

Multiple generators can also be used to calculate the cartesian product of two lists:

```
iex> for i <- [:a, :b, :c], j <- [1, 2], do:  {i, j}
[a: 1, a: 2, b: 1, b: 2, c: 1, c: 2]
```

### Bitstring generators <a href="bitstring-generators" id="bitstring-generators"></a>

Bitstring generators are also supported and are very useful when you need to comprehend over bitstring streams. The example below receives a list of pixels from a binary with their respective red, green and blue values and converts them into tuples of three elements each:

```
iex> pixels = <<213, 45, 132, 64, 76, 32, 76, 0, 0, 234, 32, 15>>
iex> for <<r::8, g::8, b::8 <- pixels>>, do: {r, g, b}
[{213, 45, 132}, {64, 76, 32}, {76, 0, 0}, {234, 32, 15}]
```

A bitstring generator can be mixed with “regular” enumerable generators, and supports filters as well.



### The `:into` option <a href="the-into-option" id="the-into-option"></a>

In the examples above, all the comprehensions returned lists as their result. However, the result of a comprehension can be inserted into different data structures by passing the `:into` option to the comprehension.

For example, a bitstring generator can be used with the `:into` option in order to easily remove all spaces in a string:

```
iex> for <<c <- " hello world ">>, c != ?\s, into: "", do: <<c>>
"helloworld"
```
