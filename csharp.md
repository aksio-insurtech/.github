# C# coding standards

## Async

* If your API supports both synchronous and asynchronous operations you **MUST** postfix the async version with `Async`.
* If your API supports only synchronous or only asynchronous operations you don't need to postfix with `Async`.
* If your system is calling the consumer either by convention or by the means of a registered callback, the system must
  be clear on its support of sync/async. If it does not support one or the other, it should throw an exception at runtime.
