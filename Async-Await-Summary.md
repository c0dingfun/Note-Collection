# Async-Await-Summary Tue - 02/11/2020
Asynchronous Programming with “async” and “await”

- !!! Key point:

   Async Task can be viewed as a UNIT of WORK (an async method) Beautiful!

   Here is a simplest example:

```csharp
    class Program
    {
        static void Main(string[] args)
        {
            ExecuteOneUnitOfWorkAsync();   // of course, it MUST be an async method!
            // telling us we are back on the main thread
            Console.WriteLine($"Back to Main Thread id = {Thread.CurrentThread.ManagedThreadId}");   
            Console.ReadLine();     // just to kept the Main Thread (the program) alive
        }
        private static async void ExecuteOneUnitOfWorkAsync()
        {
            Console.WriteLine( $"Thread id in Async Method = {Thread.CurrentThread.ManagedThreadId}"); 
            await Task.Run(new Action(LongRunningTask));
            Console.WriteLine("New Thread - done LongRunningTask");
        }
        private static void LongRunningTask()
        {
            Console.WriteLine( $"Thread id in Long Running = {Thread.CurrentThread.ManagedThreadId}"); 
            Thread.Sleep(5000);     // by sleeping to simulate a running task
        }
    }

   //Output:
   //Thread id in Async Method = 9
   //Thread id in Long Running = 6
   //Back to Main Thread id = 9
   //New Thread - done LongRunningTask ID = 6
```

## Difficulties in traditional asynchronous code

- composition
- loops
- exception handling
- timeout
- cancellation
- synchronization context (WPF, winform)

Thread vs Task:

- Thread low level concept
- Task is a high level abstraction

## [Asynchrony Evolution](https://msdn.microsoft.com/en-us/library/dd997423.aspx)

### Asynchronous Programming Model (APM) - **Obsolete**

- Method-based
- Methods
    * BeginGetData()
    * EndGetData()
- IAsyncResult
- Use TaskFromAsync to convert it to Task

### Event Asynchronous Pattern (EAP) - **Obsolete**

- Method/Event-based
- Method
    * GetDataAsync()
- Event 
    * GEtDataCompleted
    * Results in EventArgs

- Use TaskCompletionSource<TResult> to convert to Task

### Task Asynchronous Pattern (TAP) - **Recommended**

- Task-based
- Method Returns a Task
    * Task<T> GetDataAsync()
- Task
    * Represents a concurrent operation
    * May or may not operate on a separate thread

- ContinueWith(...)
    * to chained and combined Tasks

### Async-Await Task

- They are syntactic wrapper around Task.
    * “await” pauses the current method until Task is complete
    * Looks like a blocking operation
    * Does not block current thread

* *async* is just a HINT to the compiler
    * By itself, it does not make a method run asynchronously
    * Tells just the compiler to treat “await” as noted above

#### Task Exception Handling

- Exception handling for TAP and async-and-await are different.
- TAP not relying on try-catch-finally, but on combination of the following
    * Task.Exception (AggregateException)
    * Different overload of ContinueWith(...)
    * Task.Status (Canceled, Faulted, RanToCompletion)
    * Task.IsFaulted, .IsCancelled, .IsComplete (careful with IsCompleted) ...
- *async/await* can just use the traditional try...catch...finally. [**much simpler**]

#### Task Cancellation:

- CancellationToken is READONLY
    * new CancellationToken(true)
    * new CancellationToken(false)
- CancellationTokenSource to obtain CancellationToken to be passed to Task

    ```csharp
    var cts = new CancellationTokenSource()
    var token = cts.Token
    cts.Cancel()
    ```

- Inside the Task, do “cancellationToken.ThrowIfCancellationRequested(), and .Net will take care of everything internally, by throwing OperationCancelledException.

- TAP handles cancellation, just as its Exception handling
- *async-and-await* handles cancellation just as its Exception handling, (Exception type: OperationCanceledException)

#### TAP vs async-and-await

- Both are based on Task-based
- TAP has finer control
- async-await “simulate” synchronous programming

#### Terminologies:

   Asynchronous methods that you define by using “async” and “await” are referred to as async methods.

#### What would happen when the “await-point” is hit:

The method usually includes at least one await expression, which marks a point where the method can't continue until the awaited asynchronous operation is complete. In the meantime, the method is suspended, and control returns to the method's caller.

#### Threadless/Async

Task can be created in following ways, particularly for converting *obsolete* async patterns to Task-Based (TAP)
- TaskCompletionSource<TResult>
- TaskFromResult<TResult>(TResult result)
- Task.FromCancelled
- Task.FromException

### Tips: 

1. “async void” is only for top-level event handlers. (“async void” is a fire-n-forget mechanism)

- [**VERY IMPORTANT**]  “async void” IS NOT AWAITABLE!!!!
- await for “async void” will finish IMMEDIATELY, because with void, there is nothing to await!
- Also, given that “async void” is not awaitable, many people then “wait for different THING”, creating race condition.

2. Use ThreadPool for CPU-bound code, but not for IO-bound code.

- for CPU-bound work, background thread would help
- for IO -bound work, await/async would help

3. Async tasks can wrap around events with TaskCompletionSource, to make code SIMPLER!!!

- TaskCompletionSource is used to create Task objects that don't execute code!

### Ways to Create Awaitables:

1. Task as Code:

- Task
- Task<T>

```csharp
    public static void MyThreadPoolMethod()
    {
        // Do work (assuming we're running on the thread pool).
    }
    public async Task DoStuffAsync()
    {
        var cpuResult = await Task.Run(() => MyThreadPoolMethod());
        // Use cpuResult...
    }
```

- Task as Event:

    * TaskCompletionSource<T>, without any code
    * task without code can represent any kind of event, eg: I/O completion events.
    * TaskFactory.FromAsync

```csharp
      public static Task<int> MyIntegerEventAsync()
      {
          var tcs = new TaskCompletionSource<int>();
          // Register for the "event".
          //   For example, if this is an I/O operation, start the I/O and register for its completion.
          // When the event fires, it should call:
          //   tcs.TrySetResult(...); // For a successful event.
          // or
          //   tcs.TrySetException(...); // For some error.
          // or
          //   tcs.TrySetCanceled(); // If the event was canceled.
          // TaskCompletionSource is thread-safe, so you can call these methods from whatever thread you want.
          // Return the Task<int>, which will complete when the event triggers.
          return tcs.Task;
      }
```

- Task as Async Method

```csharp
      public async Task<int> DivideAsync(int numerator, int denominator)
      {
          await Task.Delay(100);
          return numerator / denominator;
      }
```

[Microsoft Example](https://code.msdn.microsoft.com/Async-Sample-Example-from-9b9f505c)

```csharp
    public partial class MainWindow : Window
    {
        // Mark the event handler with async so you can use await in it.

        // Note: >>>> this is the TOP-LEVEL handler as the STARTING POINT <<<<
        private async void StartButton_Click(object sender, RoutedEventArgs e)
        {
            var t = AccessTheWebAsync();   // get the task, t
            resultsTextBox.Text += "top-level: returning from AccessTheWebAsync()\r\n";
            int contentLength = await t;   // await on the task, t
            // vvvv NOTE NOTE NOTE NOTE NOTE NOTE NOTE NOTE NOTE NOTE 
            // The line above:  
            //     int contentLength = await t;   // await on the task, t
            // can not be replace with the following!!!!
            // WHY?
            //
            // Because the Click handler is executing in the UI THREAD!
            //
            // If the while(...) loop is occupying the UI THREAD, the UI would be frozen.
            // Even worse, if the Task is not on a separate Thread, meaning it uses the UI thread,
            // the Task itself won’t even get a chance to RUN, after returing from it 
            // [AccessTheWebAsync() returns when hitting the await()]
            //while (!t.IsCompleted)
            //{
            //    resultsTextBox.Text += "top-level: ... \r\n";
            //    Thread.Sleep(100);
            //}
            //int contentLength = await t;
            // ^^^^ END NOTE - END NOTE - END NOTE 
            resultsTextBox.Text += String.Format("\r\ntop-level: Length of the downloaded string: {0}.\r\n", contentLength);
        }

        // Three things to note in the signature:
        //  - The method has an async modifier. 
        //  - The return type is Task or Task<T>. (See "Return Types" section.)
        //    Here, it is Task<int> because the return statement returns an integer.
        //  - The method name ends in "Async."
        async Task<int> AccessTheWebAsync()
        {
            // You need to add a reference to System.Net.Http to declare client.
            HttpClient client = new HttpClient();
            // Call and await separately, in two steps:

            // Task<int> getLengthTask = AccessTheWebAsync(); // call
            // You can do independent work here.
            // int contentLength = await getLengthTask;       // await (control return to top-level handler and return here when Task completes)

            // GetStringAsync returns a Task<string>. That means that when you await the task you'll get a string (urlContents).
            Task<string> getStringTask = client.GetStringAsync("http://msdn.microsoft.com");

            // You can do work here that doesn't rely on the string from GetStringAsync.
            DoIndependentWork();

            // The await operator suspends AccessTheWebAsync.
            //  - AccessTheWebAsync can't continue until getStringTask is complete.
            //  - Meanwhile, control returns to the caller of AccessTheWebAsync.
            //  - Control resumes here when getStringTask is complete. 
            //  - The await operator then retrieves the string result from getStringTask.
            string urlContents = await getStringTask;

            // The return statement specifies an integer result.
            // Any methods that are awaiting AccessTheWebAsync retrieve the length value.
            return urlContents.Length;
        }
        void DoIndependentWork()
        {
            resultsTextBox.Text += "Working . . . . . . .\r\n";
        }
    }
```

[Jeremy’s Example]

```csharp
  private void FetchWithTaskButton_Click(object sender, RoutedEventArgs e)
  {
      tokenSource = new CancellationTokenSource();
      FetchWithTaskButton.IsEnabled = false;
      CancelButton.IsEnabled = true;
      ClearListBox();

      Console.WriteLine($"Click Handler: Thread = {Thread.CurrentThread.ManagedThreadId}, Threadpool = {Thread.CurrentThread.IsThreadPoolThread}");

      Task<List<Person>> peopleTask = repository.Get(tokenSource.Token);
      peopleTask.ContinueWith(t =>
          {
//                    var tt = t.Result;
              Console.WriteLine($"Lambda: Thread = {Thread.CurrentThread.ManagedThreadId}, Threadpool = {Thread.CurrentThread.IsThreadPoolThread}");
              switch (t.Status)
              {
                  case TaskStatus.Canceled:
                      MessageBox.Show("Operation Canceled", "Canceled");
                      break;
                  case TaskStatus.Faulted:
                      foreach (var exception in t.Exception.Flatten().InnerExceptions)
                          MessageBox.Show(exception.Message, "Error");
                      break;
                  case TaskStatus.RanToCompletion:
                      List<Person> people = t.Result;
                      foreach (var person in people)
                          PersonListBox.Items.Add(person);
                      break;
                  default:
                      break;
              }
              FetchWithTaskButton.IsEnabled = true;
              CancelButton.IsEnabled = false;
          }
          ,TaskScheduler.FromCurrentSynchronizationContext()
          );
  }

  private async void FetchWithAwaitButton_Click(object sender, RoutedEventArgs e)
  {
      tokenSource = new CancellationTokenSource();
      FetchWithAwaitButton.IsEnabled = false;
      CancelButton.IsEnabled = true;
      try
      {
          ClearListBox();
          List<Person> people = await repository.Get(tokenSource.Token);
          foreach (var person in people)
              PersonListBox.Items.Add(person);
      }
      catch (OperationCanceledException ex)
      {
          MessageBox.Show(ex.Message, "Canceled");
      }
      catch (Exception ex)
      {
          MessageBox.Show(ex.Message, "Error");
      }
      finally
      {
          FetchWithAwaitButton.IsEnabled = true;
          CancelButton.IsEnabled = false;
      }
  }

  private void CancelButton_Click(object sender, RoutedEventArgs e)
  {
      tokenSource.Cancel();
      CancelButton.IsEnabled = false;
  }

  private void ClearButton_Click(object sender, RoutedEventArgs e)
  {
      ClearListBox();
  }

  private void ClearListBox()
  {
      PersonListBox.Items.Clear();
  }

A Real Abbott Examples:

#region InstConfig Request

private void SendInstConfigRequestTo(InstModuleId moduleId)
{
    Message message = new Message {
        OneWay = new OneWayMessage {
            InstMgmtMsg = new InstrumentMgmtMsg {
                GetModuleConfigurationForBackup = new GetModuleConfigurationForBackup()
            }
        }
    };

    instCommSvc.SendOneWay(moduleId, message.ToByteArray(), MessageSig.SharedType);
}


// ###########################
// # >>> this an async Task! #
// ###########################
private async Task<EmbeddedConfigMessageEvent> GetInstConfigAsync(InstModuleId moduleId, int timeoutInSeconds)
{
    EventSubscriptionToken token = null;

    // >>> this TaskCompletionSource!!!!
    var instConfigTcs = new TaskCompletionSource<EmbeddedConfigMessageEvent>();

    try
    {
        // Rx: subscribe to EmbeddedConfigMessageEvent
        token = notificationService.Subscribe<EmbeddedConfigMessageEvent>(e => {
                    if (e.ModuleId == moduleId)             // if for this task
                        instConfigTcs.TrySetResult(e);      // signal task is completed
                });

        // Req: send AIPC message to instrument to request its configuration
        // Note: no exception thrown if connection is not there; we just rely on task's timeout
        SendInstConfigRequestTo(moduleId);   // >> make a request to get instrument config

        // now, we can await (with timeout) Rx to finish or timeout

        if (await Task.WhenAny(instConfigTcs.Task, Task.Delay(TimeSpan.FromSeconds(timeoutInSeconds))) == instConfigTcs.Task) 
            return await instConfigTcs.Task;
        else // timeout'ed
            return new EmbeddedConfigMessageEvent {ModuleId = moduleId, MessageContent = null};
    }
    finally
    {
        notificationService.Unsubscribe(token); // unsubscribe to EmbeddedConfigMessageEvent
    }
}

// #########################
// # >>>>> Entry Point<<<< #
// #########################

/// <inheritdoc/>
public AbbottLinkMessage GetInstrumentConfigFileResponseMessage(AbbottLinkMessage requestMessage, IInstMgmtSvc instMgmtSvc)
{
    var ic = AbbottLinkConfig.InstrumentConfig;         // Use short name ic = InstrumentConfig

    var tasks = new List<Task<EmbeddedConfigMessageEvent>>();   // <<< list of Tasks<T>

    // 1. create and fired off task for each instrument <<< there could be multiple instruments, each need to get its configuration files

    instMgmtSvc.GetAllModuleInfo().ForEach(mi => {

        // NOTE: right now, we get all instruments' configurations; however, because
        // NOTE: requestMessage has a componnetId (aka moduleId or module position), 
        // NOTE: in the future, we can use it (requestMessage.InstrumentLogRequest.ComponentId) 
        // NOTE: to get instrument configuration on per module basis, 

        if (instMgmtSvc.TryMakeInstConfigRequest(mi.SerialNumber)) 
            tasks.Add(GetInstConfigAsync(mi.ModuleId, ic.InstConfigResponseTimeoutInSecond));
        else
            // <<< if can’t make “get” Instrument Configure Request, just skip it, no task is created.
            LogDebugMessage(
                string.Format(
                    "AbbottLinkCommProcessor - GetInstrumentConfigFileResponseMessage(), " +
                    "skipping module ({0}) due to inst config request already made for it.", mi.ModuleId));
        });

    Task.WaitAll(tasks.ToArray()); // wait for all tasks to finish (or to timeout)

    AbbottLinkMessage response = null;
    var tmpDir = "";

    try
    {
        tmpDir = ioService.DirectoryCreateTempDirectory(ic.DestDirDefault).FullName;

        // Write each successfully received instrument config into the disk
        tasks.ForEach(async t =>  { // note: it's an aysnc lambda <<< needed because we want to use “await” to get the task.Result

                var result = await t; // >>> this will return the Task result

                var mi = instMgmtSvc.GetModuleInfo(result.ModuleId);

                if (result.MessageContent != null)
                {
                    // write instrument config into disk file
                    ioService.FileWriteAllBytes(Path.Combine(tmpDir, result.MessageContent.ConfigFileName), result.MessageContent.ConfigData);
                }
                else // time out <<< if the result.MessageContent == null
                {
                    LogDebugMessage(
                        string.Format("AbbottLinkCommProcessor - GetInstrumentConfigFileResponseMessage(), " +
                            "a module ({0}) task timeout", result.ModuleId));
                }

                instMgmtSvc.DoneInstConfigRequest(mi.SerialNumber);

            });

        var pathToInstConfig = Path.Combine(ic.DestDirDefault, ic.FileNameInstConfig);

        if (tmpDir.HasFiles(ioService))
        {
            // zip everything up and remove the tmpDir
            FileZipHelper.CreateFromDirectory(tmpDir, pathToInstConfig);
            response = pathToInstConfig.ToAbbottLinkMessageLogFileResponse();
        }
        else
        {
            var errMsg = "Obtained No Instrument Config Files.";
            abbottLinkCommLog.Warn("AbbottlinkCommProcessor - GetIntrumentConfigFileResponseMessage:" + errMsg);
            response = errMsg.ToAbbottLinkMessageLogFileErrorResponse();
        }
    }
    catch (Exception ex)
    {
        var errMsg = "Exception in requesting Instrument Config Files: " + ex.Message;
        abbottLinkCommLog.Warn("AbbottlinkCommProcessor - GetIntrumentConfigFileResponseMessage:" + errMsg);
        response = errMsg.ToAbbottLinkMessageLogFileErrorResponse();
    }
    finally
    {
        if (ioService.DirectoryExists(tmpDir))
            ioService.DirectoryDelete(tmpDir, recursive:true);
    }

    return response;
}
#endregion

```

Refs: 

   https://msdn.microsoft.com/en-us/library/mt674882.aspx

   Jeremy Bytes
   http://www.jeremybytes.com/Downloads.aspx
   https://www.youtube.com/watch?v=0qiB3oW_nd8&list=PLdbkZkVDyKZWWjJ5yUdd_ooLORZgOSHSP

   Lucian Wischik
   https://channel9.msdn.com/Series/Three-Essential-Tips-for-Async/Three-Essential-Tips-For-Async-Introduction
   https://channel9.msdn.com/Series/Three-Essential-Tips-for-Async/Tip-1-Async-void-is-for-top-level-event-handlers-only

   http://blog.stephencleary.com/2012/02/async-and-await.html
   http://blog.stephencleary.com/2012/02/creating-tasks.html
   http://blog.stephencleary.com/2012/02/reporting-progress-from-async-tasks.html
   https://blogs.msdn.microsoft.com/pfxteam/2012/04/12/asyncawait-faq/
