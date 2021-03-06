# C\#

## Access Modifiers

### volatile

 The `volatile` keyword indicates that a field might be modified by multiple threads that are executing at the same time. The compiler, the runtime system, and even hardware may rearrange reads and writes to memory locations for performance reasons. Fields that are declared `volatile` are not subject to these optimizations. Adding the `volatile` modifier ensures that all threads will observe volatile writes performed by any other thread in the order in which they were performed. There is no guarantee of a single total ordering of volatile writes as seen from all threads of execution.

##  Async

* [https://devblogs.microsoft.com/premier-developer/dissecting-the-async-methods-in-c/](https://devblogs.microsoft.com/premier-developer/dissecting-the-async-methods-in-c/)

The C\# language is great for developer’s productivity and I’m glad for the recent push towards making it more suitable for high-performance applications.

Classes `Task` and `Task<T>` were introduced in .NET 4.0 and, from my perspective, made a huge mental shift in area of asynchronous and parallel programming in .NET. Unlike older asynchronous patterns such as the `BeginXXX`/`EndXXX` pattern from .NET 1.0 \(also known as [“Asynchronous Programming Model”](https://docs.microsoft.com/en-us/dotnet/standard/asynchronous-programming-patterns/asynchronous-programming-model-apm)\) or [Event-based Asynchronous Pattern](https://docs.microsoft.com/en-us/dotnet/standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-overview) like [`BackgroundWorker`](https://docs.microsoft.com/en-us/dotnet/framework/winforms/controls/backgroundworker-component) class from .NET 2.0, tasks are composable.

### Async methods internals

A regular method has just one entry point and one exit point \(it could have more than one `return`statement but at the runtime there is just one exist point for a given call\). But async methods \(\*\) and iterators \(methods with `yield return`\) are different. In the case of an async method, a method caller can get the result \(i.e. `Task` or `Task<T>`\) almost immediately and then “await” the actual result of the method via the resulting task.

## LINQ

### Group query results

Grouping is one of the most powerful capabilities of LINQ

#### Get the list of all available products from orders where each order has product property



```csharp
var orders = (await _coinbaseProApi.ListOrders(_coinbaseSettings.AccessKey,
    _coinbaseSettings.PassPhrase,
    timestamp,
    signature
));

var products = orders
    .GroupBy(o => o.Product_Id)
    .Distinct()
    .OrderBy(o => o.Key)
    .Select(o => o.Key)
    .ToList();
```





