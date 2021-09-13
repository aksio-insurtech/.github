# C# coding standards

## Async

* If your API supports both synchronous and asynchronous operations you **MUST** postfix the async version with `Async`.
* If your API supports only synchronous or only asynchronous operations you don't need to postfix with `Async`.
* If your system is calling the consumer either by convention or by the means of a registered callback, the system must
  be clear on its support of sync/async. If it does not support one or the other, it should throw an exception at runtime.

## Fluent APIs

The goal of a fluent API is that it should provide a highly readable fluent way of describing something.

* If you're describing something, Fluent APIs can be very helpful. e.g. configuration, mapping files, projection descriptors etc.
* If you're prescribing something, Fluent APIs can be a very bad choice due to it being harder to debug and follow its flow - its no longer naturally flowing.

## Logging

Logging can actually impact the running performance. You therefor have to use the
concept of [LoggerMessage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging/loggermessage?view=aspnetcore-5.0).
It will generate optimized actions that can just be called.

We encapsulate log messages for a type in a class called the same as the type suffixed with `LogMessages`.
It holds partial methods that represent specific log messages leveraging the [compile-time logging source generation](https://docs.microsoft.com/en-us/dotnet/core/extensions/logger-message-generator)
or .NET 6. The **EventId** must be unique within every class, it should be a sequential number starting with 0.

Below is an example:

```csharp
    public static class EventLogLoggerMessages
    {
        [LoggerMessage(0, LogLevel.Information, "Committing event with '{SequenceNumber}' as sequence number")]
        internal static partial void Committing(this ILogger logger, EventType eventType, EventSourceId eventSource, uint sequenceNumber, EventLogId eventLog);

        [LoggerMessage(1, LogLevel.Error, "Problem committing event to storage")]
        internal static partial void CommitFailure(this ILogger logger, Exception exception);
    }
```
