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

## Immutable

We favor immutability when we can. The reason behind that is that immutability leads to less side-effects. Think about multi core processors
and asynchronous code as an example, having multiple threads modify state can be dangerous and lead to unwanted side-effects. It is also
very hard to reason about what happened. Keeping things immutable leads to designs of creating new instances for each of these with the
state at the time and enabling composition. With the threading example it also helps us from a performance perspective as we wouldn't
need to lock and use expensive semaphores to assure anyone else is not modifying state.

Immutability also extends into the design of APIs. APIs should not return mutatable objects. The source owns its invariance and it should
be the one mutating it. An example of this is returning a `List` or `Dictionary`. These are by design mutable. Instead you should have on
your public contract `IEnumerable` and `IReadOnlyDictionary`. The implementation could be using mutable enumerables and return these directly
as they implement `IEnumerable`.

When returning mutatable objects you're at the mercy of the consumer and having to rely on it not altering the state in which it does not
own. This can lead to unwanted side-effects and very hard to debug and reason about.

## Concepts

To articulate the domain we're working on, we prefer using specific types for everything rather than technical building blocks such as primitives.
For instance, lets say you have a domain where you have a model that includes person. On the person model you have a property holding the persons social security number.
In its basic form this is a string.

```csharp
public class Person
{
    public string SocialSecurityNumber { get; set; }
}
```

Instead of using `string`, we want this to actually be a specific type of `SocialSecurityNumber`. We call these domain concepts and can be created
in the following way:

```csharp
public record SocialSecurityNumber(string value) : ConceptAs<string>(value);
```

Then our class would change as follows:

```csharp
public class Person
{
    public SocialSecurityNumber SocialSecurityNumber { get; set; }
}
```

When I'm using this class I will quickly see the type it is expecting.
This becomes even more clear when used in APIs, such as a repository:

```csharp
public interface IPersons
{
    Person GetBy(SocialSecurityNumber socialSecurityNumber);
}
```
