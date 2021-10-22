# Mix, Agent

Mix is a build tool shipped with Elixir for creating, compiling and testing your application.

mix.exs is a config file for the project

you can define project details in `project` function, and dependencies in `dep`

{% code title="mix.exs" %}
```
defmodule KV.MixProject do
  use Mix.Project

  def project do
    [
      app: :kv,
      version: "0.1.0",
      elixir: "~> 1.11",
      start_permanent: Mix.env() == :prod,
      deps: deps()
    ]
  end

  # Run "mix help compile.app" to learn about applications
  def application do
    [
      extra_applications: [:logger]
    ]
  end

  # Run "mix help deps" to learn about dependencies
  defp deps do
    [
      # {:dep_from_hexpm, "~> 0.3.0"},
      # {:dep_from_git, git: "https://github.com/elixir-lang/my_dep.git", tag: "0.1.0"},
    ]
  end
end
```
{% endcode %}

`mix compile` to compile the project

you can start an `iex` session inside the project by running:

```
$ iex -S mix
```



```
defmodule KVTest do
  use ExUnit.Case
  doctest KV

  test "greets the world" do
    assert KV.hello() == :world
  end
end
```

* saved as .exs (elixir script files, doesn't need compilation before running)
* use ExUnit.Case to inject testing API

to run tests:

```
$ mix test
```

to format codebase:

```
mix format
```



### Environments <a href="environments" id="environments"></a>

* `:dev` - the one in which Mix tasks (like `compile`) run by default
* `:test` - used by `mix test`
* `:prod` - the one you will use to run your project in production

{% hint style="warning" %}
Mix is a build tool and, as such, it is not expected to be available in production. Therefore, it is recommended to access `Mix.env` only in configuration files and inside `mix.exs`, never in your application code (`lib`).
{% endhint %}

## Agent

How to keep and share state between multiple entities. If you have previous programming experience, you may think of globally shared variables, but the model we will learn here is quite different. The next chapters will generalize the concepts introduced here.



Elixir is an immutable language where nothing is shared by default. If we want to share information, which can be read and modified from multiple places, we have two main options in Elixir:

* Using Processes and message passing
* [ETS (Erlang Term Storage)](http://www.erlang.org/doc/man/ets.html)





* [Agent](https://hexdocs.pm/elixir/Agent.html) - Simple wrappers around state.
* [GenServer](https://hexdocs.pm/elixir/GenServer.html) - “Generic servers” (processes) that encapsulate state, provide sync and async calls, support code reloading, and more.
* [Task](https://hexdocs.pm/elixir/Task.html) - Asynchronous units of computation that allow spawning a process and potentially retrieving its result at a later time.



```
iex> {:ok, agent} = Agent.start_link fn -> [] end
{:ok, #PID<0.57.0>}
iex> Agent.update(agent, fn list -> ["eggs" | list] end)
:ok
iex> Agent.get(agent, fn list -> list end)
["eggs"]
iex> Agent.stop(agent)
:ok
```



```
iex> {:ok, agent} = Agent.start_link fn -> [] end
{:ok, #PID<0.338.0>}
iex> Agent.update(agent, fn _list -> 123 end)
:ok
iex> Agent.update(agent, fn content -> %{a: content} end)
:ok
iex> Agent.update(agent, fn content -> [12 | [content]] end)
:ok
iex> Agent.update(agent, fn list -> [:nop | list] end)
:ok
iex> Agent.get(agent, fn content -> content end)
[:nop, 12, %{a: 123}]
iex>
```

we can modify the agent state in any way we want. Therefore, we most likely don’t want to access the Agent API throughout many different places in our code. Instead, we want to encapsulate all Agent-related functionality in a single module, which we will call `KV.Bucket`

```
defmodule KV.BucketTest do
  use ExUnit.Case, async: true

  test "stores values by key" do
    {:ok, bucket} = KV.Bucket.start_link([])
    assert KV.Bucket.get(bucket, "milk") == nil

    KV.Bucket.put(bucket, "milk", 3)
    assert KV.Bucket.get(bucket, "milk") == 3
  end
end
```

`:async` must _only_ be set if the test case does not rely on or change any global values. For example, if the test requires writing to the filesystem or access a database, keep it synchronous (omit the `:async` option) to avoid race conditions between tests.

{% code title="lib/kv/bucket.ex" %}
```
defmodule KV.Bucket do
  use Agent

  @doc """
  Starts a new bucket.
  """
  def start_link(_opts) do
    Agent.start_link(fn -> %{} end)
  end

  @doc """
  Gets a value from the `bucket` by `key`.
  """
  def get(bucket, key) do
    Agent.get(bucket, &Map.get(&1, key))
  end

  @doc """
  Puts the `value` for the given `key` in the `bucket`.
  """
  def put(bucket, key, value) do
    Agent.update(bucket, &Map.put(&1, key, value))
  end
end
```
{% endcode %}

We are keeping a map inside the agent to store our keys and values. Getting and putting values on the map is done with the Agent API and the capture operator `&`, introduced in [the Getting Started guide](https://elixir-lang.org/getting-started/modules-and-functions.html#function-capturing). The agent passes its state to the anonymous function via the `&1` argument when `Agent.get/2` and `Agent.update/2` are called.



### Test setup with ExUnit callbacks <a href="test-setup-with-exunit-callbacks" id="test-setup-with-exunit-callbacks"></a>

```
defmodule KV.BucketTest do
  use ExUnit.Case, async: true

  setup do
    {:ok, bucket} = KV.Bucket.start_link([])
    %{bucket: bucket}
  end

  test "stores values by key", %{bucket: bucket} do
    assert KV.Bucket.get(bucket, "milk") == nil

    KV.Bucket.put(bucket, "milk", 3)
    assert KV.Bucket.get(bucket, "milk") == 3
  end
end
```
