Background Task (ABT)
====

Proj Q’s “Background Task” is meant to replace .Net’s BackgroundWorker pattern, on the basis that .Net’s can cause contention of accessing shared data.

Project Q’s “Background Task” requires following:

- .../Architecture.UI.Interfaces:
 
   public delegate BackgroundThreadArgs BackgroundWorkerArgsDelegate(BackgroundThreadArgs args)
   public delegate void BackgroundWorkerDoWork (object sender, DoWorkEventArgs e)
   public delegate void BackgroundWorkerTaskComplete (object sender, RunWorkerCompletedEventArgs e)

- IViewModelBase

   +AutoResetEvent InvokeActionDoneEvent
   +Boolean InvokeActionSynchronous

- ViewModelBase

   - BackgroundWorker backgroundWorker
   - ConcurrentQueue<string> workerThreadQueue
 
> BackgroundWorkerTaskComplete BackgroundWorkerTaskCompleteDelegate (run on UI thread)
> BackgroundWorkerDoWork  BackgroundWorkerDoWorkDelegate (point to the BackgroundWorkerTask instance) (output)
> BackgroundWorkerArgsDelegate BackgroundWorkerArgsDelegate (input)
 
+ void RunBackgroundCommand(String command)

- IBackgroundWorkerTask (interface) / BackgroundWorkerTask (class)

   void BackgroundWorkerTask(object sender, DoWorkEventArgs e) (output)

- BackgroundThreadArgs (class)