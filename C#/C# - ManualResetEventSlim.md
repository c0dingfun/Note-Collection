ManualResetEventSlim
====

> Reset() Sets the state of the event to non-signaled, which causes threads to block. 

> Set() Sets the state of the event to signaled, which allows one or more threads waiting on the event to proceed.

```csharp
    // following is just a way to block thread for specific period of time
    var complete = new ManualResetEventSlim(false);
    try {

        complete.Wait(TimeSpan.FromSeconds(5));
```