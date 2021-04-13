# Processes

## TLDR:

* fail fast, no need for try/ catch with supervisor
* tasks wraps around spawn, better error reports
* store state in processes
* agents are abstractions around state

Elixir’s processes should not be confused with operating system processes. Processes in Elixir are extremely lightweight in terms of memory and CPU \(even compared to threads as used in many other programming languages\). Because of this, it is not uncommon to have tens or even hundreds of thousands of processes running simultaneously.



### `spawn` <a id="spawn"></a>

The basic mechanism for spawning new processes is the auto-imported `spawn/1` function:

```text
iex> spawn(fn -> 1 + 2 end)
#PID<0.43.0>
```



`spawn/1` takes a function which it will execute in another process.

Notice `spawn/1` returns a PID \(process identifier\). At this point, the process you spawned is very likely dead. The spawned process will execute the given function and exit after the function is done:

```text
iex> pid = spawn(fn -> 1 + 2 end)
#PID<0.44.0>
iex> Process.alive?(pid)
false
```

retrieve the PID of the current process by calling `self/0`:

```text
iex> self()
#PID<0.41.0>
iex> Process.alive?(self())
true
```



### `send` and `receive` <a id="send-and-receive"></a>

We can send messages to a process with `send/2` and receive them with `receive/1`:

```text
iex> send(self(), {:hello, "world"})
{:hello, "world"}
iex> receive do
...>   {:hello, msg} -> msg
...>   {:world, _msg} -> "won't match"
...> end
"world"
```



When a message is sent to a process, the message is stored in the process mailbox. The `receive/1` block goes through the current process mailbox searching for a message that matches any of the given patterns. `receive/1` supports guards and many clauses, such as `case/2`.

The process that sends the message does not block on `send/2`, it puts the message in the recipient’s mailbox and continues. In particular, a process can send messages to itself.

If there is no message in the mailbox matching any of the patterns, the current process will wait until a matching message arrives. A timeout can also be specified:

```text
iex> receive do
...>   {:hello, msg}  -> msg
...> after
...>   1_000 -> "nothing after 1s"
...> end
"nothing after 1s"
```



A timeout of 0 can be given when you already expect the message to be in the mailbox.

Let’s put it all together and send messages between processes:

```text
iex> parent = self()
#PID<0.41.0>
iex> spawn(fn -> send(parent, {:hello, self()}) end)
#PID<0.48.0>
iex> receive do
...>   {:hello, pid} -> "Got hello from #{inspect pid}"
...> end
"Got hello from #PID<0.48.0>"
```



The `inspect/1` function is used to convert a data structure’s internal representation into a string, typically for printing. Notice that when the `receive` block gets executed the sender process we have spawned may already be dead, as its only instruction was to send a message.

While in the shell, you may find the helper `flush/0` quite useful. It flushes and prints all the messages in the mailbox.

```text
iex> send(self(), :hello)
:hello
iex> flush()
:hello
:ok
```

### Links <a id="links"></a>

```text
iex> spawn(fn -> raise "oops" end)
#PID<0.58.0>

[error] Process #PID<0.58.00> raised an exception
** (RuntimeError) oops
    (stdlib) erl_eval.erl:668: :erl_eval.do_apply/6
```

It merely logged an error but the parent process is still running. That’s because processes are isolated. If we want the failure in one process to propagate to another one, we should link them. This can be done with `spawn_link/1`:

```text
iex> self()
#PID<0.41.0>
iex> spawn_link(fn -> raise "oops" end)

** (EXIT from #PID<0.41.0>) evaluator process exited with reason: an exception was raised:
    ** (RuntimeError) oops
        (stdlib) erl_eval.erl:668: :erl_eval.do_apply/6

[error] Process #PID<0.289.0> raised an exception
** (RuntimeError) oops
    (stdlib) erl_eval.erl:668: :erl_eval.do_apply/
```

Links, however, allow processes to establish a relationship in case of failure. We often link our processes to supervisors which will detect when a process dies and start a new process in its place.

`spawn/1` and `spawn_link/1` are the basic primitives for creating processes in Elixir. Although we have used them exclusively so far, most of the time we are going to use abstractions that build on top of them. Let’s see the most common one, called tasks.

### Tasks <a id="tasks"></a>

```text
iex(1)> Task.start(fn -> raise "oops" end)
{:ok, #PID<0.55.0>}

15:22:33.046 [error] Task #PID<0.55.0> started from #PID<0.53.0> terminating
** (RuntimeError) oops
    (stdlib) erl_eval.erl:668: :erl_eval.do_apply/6
    (elixir) lib/task/supervised.ex:85: Task.Supervised.do_apply/2
    (stdlib) proc_lib.erl:247: :proc_lib.init_p_do_apply/3
Function: #Function<20.99386804/0 in :erl_eval.expr/5>
    Args: []
```

Instead of `spawn/1` and `spawn_link/1`, we use `Task.start/1` and `Task.start_link/1` which return `{:ok, pid}` rather than just the PID. This is what enables tasks to be used in supervision trees. Furthermore, `Task` provides convenience functions, like `Task.async/1` and `Task.await/1`, and functionality to ease distribution.

### State <a id="state"></a>

We haven’t talked about state so far in this guide. If you are building an application that requires state, for example, to keep your application configuration, or you need to parse a file and keep it in memory, where would you store it?

Processes are the most common answer to this question. We can write processes that loop infinitely, maintain state, and send and receive messages. As an example, let’s write a module that starts new processes that work as a key-value store in a file named `kv.exs`:

```text
defmodule KV do
  def start_link do
    Task.start_link(fn -> loop(%{}) end)
  end

  defp loop(map) do
    receive do
      {:get, key, caller} ->
        send caller, Map.get(map, key)
        loop(map)
      {:put, key, value} ->
        loop(Map.put(map, key, value))
    end
  end
end
```

Note that the `start_link` function starts a new process that runs the `loop/1` function, starting with an empty map. The `loop/1` \(private\) function then waits for messages and performs the appropriate action for each message. We made `loop/1` private by using `defp` instead of `def`. In the case of a `:get` message, it sends a message back to the caller and calls `loop/1` again, to wait for a new message. While the `:put` message actually invokes `loop/1` with a new version of the map, with the given `key` and `value` stored.

Let’s give it a try by running `iex kv.exs`:

```text
iex> {:ok, pid} = KV.start_link()
{:ok, #PID<0.62.0>}
iex> send(pid, {:get, :hello, self()})
{:get, :hello, #PID<0.41.0>}
iex> flush()
nil
:ok
```

name the process \(pid\):

```text
iex> Process.register(pid, :kv)
true
iex> send(:kv, {:get, :hello, self()})
{:get, :hello, #PID<0.41.0>}
iex> flush()
:world
:ok
```

 Elixir provides [agents](https://hexdocs.pm/elixir/Agent.html), which are simple abstractions around state:

```text
iex> {:ok, pid} = Agent.start_link(fn -> %{} end)
{:ok, #PID<0.72.0>}
iex> Agent.update(pid, fn map -> Map.put(map, :hello, :world) end)
:ok
iex> Agent.get(pid, fn map -> Map.get(map, :hello) end)
:world
```

