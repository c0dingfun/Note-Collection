
ObservableCollection - INotifyCollectionChanged
====

> When writing an MVVM application, you want to separate ViewModel from the UI View. However you also need to make sure that UI updates happen on the UI thread. Changes made through INotifyPropertyChanged get automatically marshaled to the UI thread, so in most cases you'll be fine. However, when using INotifyCollectionChanged (such as with an ObservableCollection), these changes are **Not** marshaled to the UI thread.

> Using BindingOperations.EnableCollectionSynchronization(...) is needed.

> Example:

```csharp

    Application.Current.Dispatcher.BeginInvoke(new Action(() => { 
BindingOperations.EnableCollectionSynchronization(Slots, mSlotCollectionSyncObject); }));

  Another way is use the Dispatcher to update the ObservableCollection, like:

    Application.Current.Dispatcher.Invoke(() =>
    {
        ScrappedSubstrateItems.Clear();
        NormalRemovedSubstrateItems.Clear();
    });

```