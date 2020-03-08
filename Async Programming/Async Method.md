# Async Method

## How to write your own Async Method

[Ref](http://stackoverflow.com/questions/21954799/how-to-create-your-own-asynchronous-methods-with-async-and-await)

```csharp

public function Task<string> GetParsedData(string code){
    Console.WriteLine("Starting parser");

    var taskResult = await Task.Run(() =>
    {
        var result = "";
        foreach(var x in code.Split('\n'))
            result += Encode(x);

        return result;
    });
    return taskResult;
}
```

Now I can use the await keyword on this method within other async methods as needed.

## Simplest Example to turn an Sync method into Async and use it inside of another Async method 

- Simple use Task.Run()!!!!
  
LoadCommercialControlLotNumbersAsync(); // <<< usage

```csharp

// async method as wrapper, required to use "await" inside
private async void LoadCommercialControlLotNumbersAsync() 
{
   SelectableLotNumbers.AddRange(
      await Task.Run(() =>                            // becoming waitable!
         qcMgmtClientSvc.AbbottControlLotNumbers())); // <<< sync method
}
```

## Task.Run() to turn sync to async


[Ref](http://stackoverflow.com/questions/17119075/do-you-have-to-put-task-run-in-a-method-to-make-it-async)

First, let's clear up some terminology: "asynchronous" (async) means that it may yield control back to the calling thread before it finishes. In an async method, those "yield" points are await expressions.

This is very different than the term "asynchronous", as (mis)used by the MSDN documentation for years to mean "executes on a background thread".

To further confuse the issue, async is very different than "awaitable"; there are some async methods whose return types are not awaitable, and many methods returning awaitable types that are not async.

Enough about what they aren't; here's what they are:

The async keyword allows an asynchronous method (that is, it allows await expressions within). async methods may return Task, Task<T>, or (if you must) void.

Any type, except void, that follows a certain pattern can be awaitable. The most common awaitable types are Task and Task<T>.

So, if we reformulate your question to "how can I run an operation on a background thread in a way that it's awaitable", the answer is to use Task.Run:

```csharp
  private Task<int> DoWorkAsync() // No async because the method does not need await
  {
    return Task.Run(() => { return 1 + 2; });
  }
```

(But this pattern is a poor approach; see below).

But if your question is "how do I create an async method that can yield back to its caller instead of blocking", the answer is to declare the method async and use await for its "yielding" points:

```csharp
  private async Task<int> GetWebPageHtmlSizeAsync()
  {
    var client = new HttpClient();
    var html = await client.GetAsync("http://www.example.com/");
    return html.Length;
  }
```

So, the basic pattern of things is to have async code depend on "awaitables" in its await expressions. These "awaitables" can be other async methods or just regular methods returning awaitables. Regular methods returning Task/Task<T> can use Task.Run to execute code on a background thread, or (more commonly) they can use TaskCompletionSource<T> or one of its shortcuts (TaskFactory.FromAsync, Task.FromResult, etc). I don't recommend wrapping an entire method in Task.Run; synchronous methods should have synchronous signatures, and it should be left up to the consumer whether it should be wrapped in a Task.Run:

```csharp
private int DoWork() { return 1 + 2; }

private void MoreSynchronousProcessing()
{
  // Execute it directly (synchronously), since we are also a synchronous method.
  var result = DoWork();
  ...
}

private async Task DoVariousThingsFromTheUIThreadAsync()
{
  // I have a bunch of async work to do, and I am executed on the UI thread.
  var result = await Task.Run(() => DoWork());
  ...
}
```