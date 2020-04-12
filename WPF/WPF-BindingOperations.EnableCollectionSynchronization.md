BindingOperations.EnableCollectionSynchronization
====

EC editor is empty in Quin (Ritter and Kuna has simular problem when SmartSticks are connected, just that Quin's SmartStick connected during initialization)

> https://blogs.msdn.microsoft.com/nathannesbit/2009/04/20/addrange-and-observablecollection/

Upshot is that in the OnConnChanged() routine which run in worker thread, the ObservableCollection must be created in UI thread (the Main thread) and have synchronization mechanism established.

So added:

```csharp


mModuleEcsDisplay = new Dictionary<string, ObservableCollection<EcGroupViewModel>>();
TotalEcCount = 0;
foreach (var module in mModuleEcs)
{
    // Added:
    Application.Current.Dispatcher.BeginInvoke(new Action(()=> 
    {
        var ecCollection = new ObservableCollection<EcGroupViewModel>();
        BindingOperations.EnableCollectionSynchronization(ecCollection, mEcCollectionLock);
        mModuleEcsDisplay.Add(module.Key, ecCollection);
    }));
    // Added --- end:

    TotalEcCount += module.Value.Values.Count();
}

```