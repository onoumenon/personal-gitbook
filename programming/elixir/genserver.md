# GenServer

we can register processes in Elixir by giving them atom names:

```
iex> Agent.start_link(fn -> %{} end, name: :shopping)
{:ok, #PID<0.43.0>}
iex> KV.Bucket.put(:shopping, "milk", 1)
:ok
iex> KV.Bucket.get(:shopping, "milk")
1
```

However, naming dynamic processes with atoms is a terrible idea! If we use atoms, we would need to convert the bucket name (often received from an external client) to atoms, and **we should never convert user input to atoms**. This is because atoms are not garbage collected. Once an atom is created, it is never reclaimed. Generating atoms from user input would mean the user can inject enough different names to exhaust our system memory!

\
The registry needs to guarantee that it is always up to date. For example, if one of the bucket processes crashes due to a bug, the registry must notice this change and avoid serving stale entries. In Elixir, we say the registry needs to _monitor_ each bucket. Because our _registry_ needs to be able to receive and handle ad-hoc messages from the system, the `Agent` API is not enough.

We will use a [GenServer](https://hexdocs.pm/elixir/GenServer.html) to create a registry process that can monitor the bucket processes. GenServer provides industrial strength functionality for building servers in both Elixir and OTP.



Create a new file at `lib/kv/registry.ex` with the following contents:

```
defmodule KV.Registry do
  use GenServer

  ## Missing Client API - will add this later

  ## Defining GenServer Callbacks

  @impl true
  def init(:ok) do
    {:ok, %{}}
  end

  @impl true
  def handle_call({:lookup, name}, _from, names) do
    {:reply, Map.fetch(names, name), names}
  end

  @impl true
  def handle_cast({:create, name}, names) do
    if Map.has_key?(names, name) do
      {:noreply, names}
    else
      {:ok, bucket} = KV.Bucket.start_link([])
      {:noreply, Map.put(names, name, bucket)}
    end
  end
end
```
