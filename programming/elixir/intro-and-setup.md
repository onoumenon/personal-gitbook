# Intro & setup

{% embed url="https://elixir-lang.org/" %}

## Platform features

### Scalability

Elixir runs in lightweight execution threads \(aka processes\) that are isolated and exchange messages. 

Isolation means:

*  Can run thousands of processes concurrently as they are lightweight
* Garbage collection per process, so no system-wide pauses
* Uses machine resource effectively \(vertical scaling\)

Communication between processes means:

* can communicate with other processes on different machine on same network
* can coordinate work across multiple nodes \(horizontal scaling\)

### Fault tolerance

`Supervisors` can restart processes when things go awry

```text
children = [
  TCP.Pool,
  {TCP.Acceptor, port: 4040}
]

Supervisor.start_link(children, strategy: :one_for_one)
```

## Language features

### Functional programming

* promotes a coding style that is concise and manageable

Eg: pattern matching for destructuring

```text
%User{name: name, age: age} = User.get("John Doe")
name #=> "John Doe"
```

### Extensible

Eg: test case using [Elixir's test framework called ExUnit](https://hexdocs.pm/ex_unit/):

```text
defmodule MathTest do
  use ExUnit.Case, async: true

  test "can add two numbers" do
    assert 1 + 1 == 2
  end
end
```

`async` and `assert` are built using Elixir macros, making it possible to add new constructs as if they were part of the language itself.

## Setup

on mac:

`brew install elixir`

set up path, check version



{% embed url="https://wandbox.org/\#" %}

Alternative: Online elixir compiler to skip setup

## Create first elixir module

```text
defmodule Wandbox do
    def hello do
        IO.puts "Hello"
    end
end

Wandbox.hello()
```

save as `filename.exs`

to run:

```text
$ elixir filename.exs
```



 

