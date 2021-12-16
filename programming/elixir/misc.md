# misc

### Guard clause

Note that `[]` in the below function wouldn't match:

```
  defp has_minimum_quantity(%PriceRule{entitled_item_ids: []} do
    ...
  end
```

Use a guard clause instead:

```
  defp has_minimum_quantity(%PriceRule{entitled_item_ids: entitled_item_ids}, _cart_items)
       when length(entitled_item_ids) == 0,
       do: {:error, "no item entitled for promo"}
```



### List implementation

Similarly, we could write the list `[1, 2, 3]` using only such pairs (called cons cells):

```
iex> [1 | [2 | [3 | []]]]
[1, 2, 3]
```

Most of the functions in this module work in linear time. This means that, that the time it takes to perform an operation grows at the same rate as the length of the list. For example [**`length/1`**](https://hexdocs.pm/elixir/Kernel.html#length/1) **and** [**`last/1`**](https://hexdocs.pm/elixir/List.html#last/1) **will run in linear time because they need to iterate through every element of the list, but** [**`first/1`**](https://hexdocs.pm/elixir/List.html#first/1) **will run in constant time because it only needs the first element.**
