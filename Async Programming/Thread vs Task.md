# Thread vs Task

## How are Thread and Task **Different**

| Area | Thread | Task |
|------|------|-------|
| Level|  Low level OS concept  | High Level Concept|
| Cost | Thread is expensive | Use ThreadPool thread|
|Composable| No | Yes, easily|
|Getting Result Back| No Standard Way | Task<T> XyzAysnc() |
|Knowing when finished | No Standard Way | Task.ContinueWith(); or better, await XyzAysnc() |
|Looping| No | Yes|
|Exception Propagation| Non| Easy, either use try-catch or Task.Exception|
|Cancellation | Yes, use CancellationToken| Yes, use CancellationToken|
|Continuation/Resume| No natural way| Yes, .ContinueWith()|
|SynchronizationContext| Manual | Auto|
|Async/Await| Not Applied| Awaitable|
|Attached to Parent| No | Yes|
|Status | - | TaskStatus enum|

Thread: 
    Join
    Polling
    Event
    Callback

## Concurrency vs Parallelism

| Concurrency | Parallelism|
|----|----|
|Non Blocking| Performance|
|Usable Application| - |
|Break down big problem in to piece that can run concurrently| Run things in parallel|
|Can do time slicing on a thread| Parallelism is a special case of Concurrency (eg: on multicore system) |

## Async-Await

Task does back by ThreadPool thread. However, Async-Await does not create an extra Thread. Async-Await is a syntactic sugar which allow the compiler to create a state machine, so that when await is called, the control is return to the caller immediately. When the awaited is finished, control is back to the async method and the rest of the code will be either run in the caller's thread or a ThreadPool thread, depending on whether or not a SynchronizationContext is available or not. If not available ThreadPool thread is used. In addition, ConfigureAwait(bool) is used to specify w

### Returning Result in Async Method

When you need to return a value inside an async method that returns a task, you don't need to return the task explicitly. You can regularly return the value as if it were a simple synchronous method. The compiler makes the transformations behind the scenes:

```csharp
private static async Task<string> GetPageAsync()
{
    var httpClient = new HttpClient();

    var response = await httpClient.GetAsync("http://ReactiveX.io");
    var page = await response.Content.ReadAsStringAsync();
    return page;
}
```

### Await Hiding the Task.ContinueWith()

Await is hiding the callings of Task.ContinueWith(prevTask=>{...});
However, don't think await can completely replace Tas.ContinueWith() because what await replaced is a very simple ContinueWith( ) overload; there are many more overloads of the ContinueWith() that would call for ContinueWith() only when previous task is Faulted, RunToComplete, etc.; there are many options to consider.

Further more when application need to ContinueWith() multiple attached multiple task

```csharp
    private const TaskCreationOptions OperationTaskCreationOptions =
                    TaskCreationOptions.AttachedToParent |
                    TaskCreationOptions.PreferFairness;

    private const TaskContinuationOptions SuccessfulTaskContinuationOptions =
                    TaskContinuationOptions.OnlyOnRanToCompletion |
                    TaskContinuationOptions.AttachedToParent |
                    TaskContinuationOptions.LazyCancellation |
                    TaskContinuationOptions.PreferFairness;

    private const TaskContinuationOptions FaultedTaskContinuationOptions =
                    TaskContinuationOptions.OnlyOnFaulted |
                    TaskContinuationOptions.AttachedToParent |
                    TaskContinuationOptions.LazyCancellation |
                    TaskContinuationOptions.PreferFairness;

    private const TaskContinuationOptions AlwaysTaskContinuationOptions =
                    TaskContinuationOptions.AttachedToParent |
                    TaskContinuationOptions.LazyCancellation |
                    TaskContinuationOptions.PreferFairness
```

## Synchronization Context

SynchronizationContext catches where (which thread) code is needed to run or is running. It is needed because, particularly UI application, such as WinForm, WPF, ASP.Net (legacy), UI controls are created using the UI thread ins STA (Single Thread Apartment). Thus, modification to them must be done by the UI thread itself. The SynchronizationContext is used to capture that information.

SynchronizationContext is the base class for following derived classes for different frameworks:

- WinForm: WindowsFormSynchronizationContext
- WPF: DispatcherSynchronizationContext
- ASP.Net: AspNetSynchronizationContext

Not all apps has SynchronizationContext, such as the Console app and Window service. And Asp.Net Core does not have SynchronizationContext.

## BlockingCollection

For used in Producer and Consumer scenarios

### Bounding and Blocking Support

Able to set the capacity of the Collection, so that the producers could not run too far ahead of the consumer.

When the collection is full, producer tries to Add() will block, until consumer Take one() from the collection.

When the collection is empty, consumer tries to Take() will block.

Also, Producer can call CompleteAdding() to let the consumer know there is no more to be taken. To know that by the consumer, it read the IsCompleted property of the collection.

### Time Blocking Operations

BlockingCollection has two methods, TryAdd() and TryTake(), that allow us to specify a timeout (in ms).

They will return *true* if operation is completed before timeout; otherwise, they will return false.

### Cancelling TryAdd() and Take() operations

In addition, these methods has overloads that take the CancellationToken. Isn't it nice!!!

Normally, the TryAdd() and TryTake() are typically performed in a loop. You can cancel a loop by passing a CancellationToken to the TryAdd() or TryTake(), and then checking the value of the token's IsCancellationRequested property on each iteration. If the value is true, then it is up to you to respond the cancellation request by cleaning up any resources and existing the loop.

Or do it in Try-Catch which will get OCE exception if the operation is cancelled.

```csharp
do
{
    // Cancellation causes OCE. We know how to handle it.
    try
    {
        success = bc.TryAdd(itemToAdd, 2, ct);
    }
    catch (OperationCanceledException)
    {
        bc.CompleteAdding();
        break;
    }
    //...
} while (moreItems == true);

```

### Specifying the Collection Type as it Data Store

When you create the BlockingCollection<T>, you can specify not only the bounded capacity but also the type of the Collection to use for Data Store.

| Behavior | Collection Type that implements IProducerConsumerCollection<T> |
|-------------|------------|
| FIFO | ConcurrentQueue<T>|
| LIFO | ConcurrentStack<T>|
| Bag | ConcurrentBag<T>|
|Default| ConcurrentQueue<T>|

```csharp
BlockingCollection<string> bc = 
    new BlockingCollection<string>(new ConcurrentBag<string>(), 1000 );  
```

### IEnumerable Support

BlockingCollection<T> provides a GetConsumingEnumerable method that enables consumers to use **foreach** to remove items until the collection is completed, which means it is empty and no more items will be added.

### Using Many BlockingCollections as One

In situations where a consumer needs to take items from multiple collections simultaneously, you can create arrays of BlockingCollection<T> and use the static methods such as TakeFromAny() and AddToAny() that will add to or take from any of the collections in the array. If one collection is blocking, the method immediately tries another until it finds one that can perform the operation.

## Managed Thread

### Managed Threading Basics

[Credit:Microsoft](https://docs.microsoft.com/en-us/dotnet/standard/threading/managed-threading-basics)

A **process** is an executing program. An OS uses **process** to separate the applications that are being executed.

A **thread** is the basic unit to which an OS allocates processor time. Each **thread** has a scheduling priority and maintains a set of structures the system uses to save the thread context when the thread's execution is paused. The thread context includes all the information the thread needs to seamlessly resume execution, including the thread's set of CPU registers and stack.

Multiple **threads** can run in the context of a process. All threads of a process share its virtual address space. A thread can execute any part of the program code, include parts currently being executed by another thread.

Actually, the process and thread hierarchy has an additional entity called AppDomain.

Process--> (n) AppDomain ---> (m) Threads

By default, Process has a default AppDomain, which by default threads are belong to.

Additional AppDomains can be created. Normally, we want to use AppDomain as a sandbox for third-party DLLs, which we don't have source code for them.

Note: .Net Core does not have AppDomain

### Exceptions in Managed Thread

***Catch*** Exceptions happened in a thread within the thread code. Because the exception won't be propagated back to the calling thread (the thread that spawn the thread).

### How to Catch All C# Exceptions

[Credit](https://dzone.com/articles/what-is-an-unhandled-exception-and-how-to-catch-al)

```csharp
static void Main(string[] args)
{
  Application.ThreadException += 
    new ThreadExceptionEventHandler(Application_ThreadException);

  AppDomain.CurrentDomain.UnhandledException += 
    new UnhandledExceptionEventHandler(CurrentDomain_UnhandledException);

  string fileContents = File.ReadAllText(args[0]);
  //do something with the file contents
}
static void Application_ThreadException(object sender, ThreadExceptionEventArgs e)
{
  // Log the exception, display it, etc
  Debug.WriteLine(e.Exception.Message);
}
static void CurrentDomain_UnhandledException(object sender, UnhandledExceptionEventArgs e)
{
  // Log the exception, display it, etc
  Debug.WriteLine((e.ExceptionObject as Exception).Message);
}

```

In addition, we can use Windows Event Viewer to look at the Unhandled Exceptions.

### First Chance Exception

[What is first Chance Exception](https://docs.microsoft.com/en-us/archive/blogs/davidklinems/what-is-a-first-chance-exception)

When an application is being debugged, the debugger gets notified whenever an exception is encountered  At this point, the application is suspended and the debugger decides how to handle the exception. The first pass through this mechanism is called a "first chance" exception. Depending on the debugger's configuration, it will either resume the application and pass the exception on or it will leave the application suspended and enter debug mode. If the application handles the exception, it continues to run normally.

Does a first chance exception mean there is a problem in my code?

First chance exception messages most often do not mean there is a problem in the code. For applications / components which handle exceptions gracefully, first chance exception messages let the developer know that an exceptional situation was encountered and was handled.

For code without exception handling, the debugger will receive a second chance exception notification and will stop with a unhandled exception.

[Credit](https://stackoverflow.com/questions/564681/what-is-a-first-chance-exception)

It's a debugging concept. Basically exceptions are thrown to the debugger first and then to the actual program where if it isn't handled it gets thrown to the debugger a second time, giving you a chance to do something with it in your IDE before and after the application itself. This appears to be a Microsoft Visual Studio invention.

#### Receiving First Chance Exception Notifications in the Default Application Domain

Define an event handler for the FirstChanceException event, using a lambda function, and attach it to the event.

```csharp
using System;
using System.Runtime.ExceptionServices;

class Example
{
    static void Main()
    {
        AppDomain.CurrentDomain.FirstChanceException 
            += (object source, FirstChanceExceptionEventArgs e) => 
            {
                Console.WriteLine(
                    "FirstChanceException event raised in {0}: {1}",
                    AppDomain.CurrentDomain.FriendlyName, e.Exception.Message);
            };

            // ...
    }
```

## ThreadPool Thread

[Credit](https://docs.microsoft.com/en-us/dotnet/standard/threading/the-managed-thread-pool)

The System.Threading.ThreadPool class provides your application with a pool of worker threads that are managed by the system, allowing you to concentrate on application tasks rather than thread management. If you have short tasks that require background processing, the managed thread pool is an easy way to take advantage of multiple threads. Use of the thread pool is significantly easier in Framework 4 and later, since you can create Task and Task<TResult> objects that perform asynchronous tasks on thread pool threads.

.NET uses thread pool threads for many purposes, including Task Parallel Library (TPL) operations, asynchronous I/O completion, timer callbacks, registered wait operations, asynchronous method calls using delegates, and System.Net socket connections.

### Thread Pool Characteristics

Thread pool threads are background threads. 

Each thread uses the default stack size, runs at the default priority, and is in the ***multithreaded apartment***. Once a thread in the thread pool completes its task, it's returned to a queue of waiting threads. From this moment it can be reused. This reuse enables applications to avoid the cost of creating a new thread for each task.

There is only one thread pool per process.

### Exceptions in thread pool threads

Unhandled exceptions in thread pool threads terminate the process.

There are three exceptions to this rule:

- A System.Threading.ThreadAbortException is thrown in a thread pool thread because Thread.Abort was called.
- A System.AppDomainUnloadedException is thrown in a thread pool thread because the application domain is being unloaded.
- The common language runtime or a host process terminates the thread.

You can control the maximum number of threads by using the ThreadPool.GetMaxThreads and ThreadPool.SetMaxThreads methods.

When GetMaxThreads returns, the variable specified by **workerThreads** contains the maximum number of worker threads allowed in the thread pool, and the variable specified by **completionPortThreads** contains the maximum number of asynchronous I/O threads allowed in the thread pool.

### [Foreground vs Background Thread](https://docs.microsoft.com/en-us/dotnet/standard/threading/foreground-and-background-threads)

A managed thread is either a background thread or a foreground thread. Background threads are identical to foreground threads with one exception: a background thread does not keep the managed execution environment running. Once all foreground threads have been stopped in a managed process (where the .exe file is a managed assembly), the system stops all background threads and shuts down.

Use the Thread.IsBackground property to determine whether a thread is a background or a foreground thread, or to change its status. A thread can be changed to a background thread at any time by setting its IsBackground property to true.

## [Cooperative Cancellation](https://docs.microsoft.com/en-us/dotnet/standard/threading/cancellation-in-managed-threads)

This is **Applicable to Both Threads and Tasks**

Starting with the .NET Framework 4, the .NET Framework uses a unified model for cooperative cancellation of asynchronous or long-running synchronous operations. This model is based on a lightweight object called a cancellation token.

The general pattern for implementing the cooperative cancellation model is:

- Instantiate a CancellationTokenSource object, which manages and sends cancellation notification to the individual cancellation tokens.

```csharp
    var cts = new CancellationTokenSource();
```

- Pass the token returned by the **CancellationTokenSource.Token** property to each TASK or THREAD that listens for cancellation.

- Provide a mechanism for each TASK or THREAD to respond to cancellation.

- Call the **CancellationTokenSource.Cancel()** method to provide notification of cancellation.

### Types needed to implement **Cooperative Cancellation***

|Type |Description|
|-----|------|
|CancellationTokenSource|Creates a cancellation token, and also issues the cancellation request for all copies of that token|
|CancellationToken|Value type passed to one or more listeners, typically a method parameter. Listeners monitor the value of the **IsCancellationRequested** property of the token by|
| - | 1. Polling|
| - | 2. Callback|
| - | 3. WaitHandle|
|OperationCanceledException|Listeners can optionally throw this exception to verify the source of the cancellation and notify others that it has responded to a cancellation request.|

#### Polling for Cancellation

```csharp
static void NestedLoops(Rectangle rect, CancellationToken token)
{
   for (int x = 0; x < rect.columns && !token.IsCancellationRequested; x++) {
      for (int y = 0; y < rect.rows; y++) {
         // Simulating work.
         Thread.SpinWait(5000);
         Console.Write("{0},{1} ", x, y);
      }

      // Assume that we know that the inner loop is very fast.
      // Therefore, checking once per row is sufficient.
      if (token.IsCancellationRequested) {
         // Cleanup or undo here if necessary...
         Console.WriteLine("\r\nCancelling after row {0}.", x);
         Console.WriteLine("Press any key to exit.");
         // then...
         break;
         // ...or, if using Task:
         // token.ThrowIfCancellationRequested();
      }
   }
}
```

#### Registering Callback for Cancellation

Some operations can become blocked in such a way that they cannot check the value of the cancellation token in a timely manner. For these cases, you can register a callback method that unblocks the method when a cancellation request is received.

```csharp

class Example
{
    static void Main()
    {
        CancellationTokenSource cts = new CancellationTokenSource();

        StartWebRequest(cts.Token);

        // cancellation will cause the web 
        // request to be cancelled
        cts.Cancel();
    }

    static void StartWebRequest(CancellationToken token)
    {
        WebClient wc = new WebClient();
        wc.DownloadStringCompleted += (s, e) => Console.WriteLine("Request completed.");

        // Cancellation on the token will 
        // call CancelAsync on the WebClient.
        token.Register(() =>
        {
            wc.CancelAsync();
            Console.WriteLine("Request cancelled!");
        });

        Console.WriteLine("Starting request.");
        wc.DownloadStringAsync(new Uri("http://www.contoso.com"));
    }
}
```

***Further Reading***, there are guidelines to avoid ***deadlock***


#### Using WaitHandle for Cancellation

When a cancelable operation can block while it waits on a synchronization primitive such as a System.Threading.ManualResetEvent or System.Threading.Semaphore, you can use the CancellationToken.WaitHandle property to enable the operation to wait on both the event and the cancellation request.

**Note:** .NET Framework 4, System.Threading.ManualResetEventSlim and System.Threading.SemaphoreSlim both support the new cancellation framework in their Wait methods. You can pass the CancellationToken to the method, and when the cancellation is requested, the event wakes up and throws an OperationCanceledException.

```csharp
try
{
    // mres is a ManualResetEventSlim
    mres.Wait(token);
}
catch (OperationCanceledException)
{
    // Throw immediately to be responsive. The
    // alternative is to do one more item of work,
    // and throw on next iteration, because
    // IsCancellationRequested will be true.
    Console.WriteLine("The wait operation was canceled.");
    throw;
}

Console.Write("Working...");
// Simulating work.
Thread.SpinWait(500000);
```

#### Listening to Multiple Cancellation Tokens Simultaneously

```csharp
public void DoWork(CancellationToken externalToken)
{
   // Create a new token that combines the internal and external tokens.
   this.internalToken = internalTokenSource.Token;
   this.externalToken = externalToken;

   using (CancellationTokenSource linkedCts =
           CancellationTokenSource.CreateLinkedTokenSource(internalToken, externalToken))
   {
       try {
           DoWorkInternal(linkedCts.Token);
       }
       catch (OperationCanceledException) {
           if (internalToken.IsCancellationRequested) {
               Console.WriteLine("Operation timed out.");
           }
           else if (externalToken.IsCancellationRequested) {
               Console.WriteLine("Cancelling per user request.");
               externalToken.ThrowIfCancellationRequested();
           }
       }
   }
}
```

## Synchronization Mechanisms

Thread Synchronization Deals with the following conditions:

1. Deadlock
2. Starvation
3. Priority Inversion
4. Busy Waiting

The Following are some classic problems of Synchronization:

1. The Producer-Consumer Problem
2. The Readers-Writers Problem
3. The Dining Philosopher Problem


| Mechanism | Category | Description|
|-----------|------------|----|
|[Synchronized] | Blocking Method | method  attribute (**obsolete**?)|
| Interlocked | Atomic Ops | (static class) for **atomic ops** [Add(), Compare(), Decrement(), CompareExchange()], ... |
| lock() | Locking |  lock(priv_ro_obj) {...}, simplified Monitor, for **critical section** |
|Monitor| Locking | (static class) Enter()/TryEnter()/Exit(), Pulse/PulseAll/Wait(), for **critical section** |
|Semaphore| Blocking Wait() | new Semaphore(init_val, max_val), .Release(), Release(int), for **thread coordination**, permitting # of threads to access shared resources concurrently |
|Mutex| Locking | Mutex(bool initOwned), .ReleaseMutex(), for **thread coordination**, exclusive access to shared resources |
|ManualResetEventSlim()| Signal |  for **thread coordination**, permitting # of threads doing work, like a door, once open it lets everyone through until you close it yourself|
|AutoResetEvent() | Signal | for **thread coordination**, permitting only on thread to do work, like a turnstile |

### AutoResetEvent vs ManualResetEventSlim

AutoResetEvent is for exclusivity.
ManualResetEventSlim is for a task which one thread must complete before other threads can proceed.

