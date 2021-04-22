# Sage \(Saga Pattern\)

Rollback/ recover changes with saga pattern:

{% embed url="https://github.com/Nebo15/sage" %}

Sage is a dependency-free implementation of the [Sagas](http://www.cs.cornell.edu/andru/cs711/2002fa/reading/sagas.pdf) pattern in pure Elixir and provides a set of additional built-in features.

It is a go-to way for dealing with distributed transactions, especially with an error recovery/cleanup. Sage does it's best to guarantee that either all of the transactions in a saga are successfully completed, or compensating that all of the transactions did run to amend a partial execution.

> It’s like `Ecto.Multi` but across business logic and third-party APIs.
>
> -- @jayjun

This is done by defining two way flow with transaction and compensation functions. When one of the transactions fails, Sage will ensure that the transaction's and all of its predecessors' compensations are executed. However, it's important to note that Sage can not protect you from a node failure that executes given Sage.

To visualize it, let's imagine we have a 4-step transaction. Successful execution flow would look like:

```text
[T1] -> [T2] -> [T3] -> [T4]
```

and if we get a failure on 3-d step, Sage would cleanup side effects by running compensation functions:

```text
[T1] -> [T2] -> [T3 has an error]
                ↓
[C1] <- [C2] <- [C3]
```

