
UI Thread Monitor
====

In UI, when UI is slow to hung, there is most like the UI Thread is occupied by some codes. In order to confirm if that is the case, or monitor such a case, a UI Thread Monitor is a helpful tool.

Codes for UI Thread Monitor
----

```csharp
/// <summary>    
/// Usage:
/// New up this class and set a breakpoint
/// ref:  https://stackoverflow.com/questions/3758838/determine-what-is-blocking-ui-thread
/// </summary>
public class UiThreadBlockerDetector
{
    static Timer _timer;
    public UiThreadBlockerDetector(ILogger logger, int maxFreezeTimeInMilliseconds = 1000) // 1 seconds
    {
        var sw = new Stopwatch();
        new DispatcherTimer(TimeSpan.FromMilliseconds(10),
                            DispatcherPriority.Send,
                            (sender, args) => { lock (sw) { sw.Restart(); } },
                            Application.Current.Dispatcher);

        _timer = new Timer(
            state => {  // callback
                lock (sw)
                {
                    if (sw.ElapsedMilliseconds > maxFreezeTimeInMilliseconds)
                    {
                        // set breakpoint here;
                        // Goto Visual Studio --> Debug --> Windows --> Theads 
                        // and checkup where the MainThread is.

                        logger.Write(LogEntryCategory.Progress,
                            $"HH: - WARNING ... UI thread is being held up for at least ({sw.ElapsedMilliseconds})");

                        // or 
                        //if (!Debugger.IsAttached)
                        //    Debugger.Launch();

                        //Debugger.Break();

                    }
                }
            },
            null, // state, passed to the callback; can be null
            TimeSpan.FromMilliseconds(0),   // due time, 0 = start immediately
            TimeSpan.FromMilliseconds(100)); // period, time interval of callbacks
    }
}


```