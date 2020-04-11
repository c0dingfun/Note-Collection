
ListView SelectedItems and ViewModel Binding
====

Turns out the IsSelected is on a ListBoxItem, which DataContext is the T in ObservableCollection`<T>`().

So, we can have the T to "save" the IsSelected value and in the ItemContainerStyle (TargetType=ListViewItem) we can bind the ListViewItem's IsSelected to the T's IsSelected. Then, switching tabs will restore the Selected Items on the ListView.

ListView when SelectionMode=Multiple/Extended is a bit tricky.

The SelectedItems is NOT Bindable!!!

There no SelectedIndices for us to use!!!

[Ref](http://blog.functionalfun.net/2009/02/how-to-databind-to-selecteditems.html)
[Ref](https://www.markwithall.com/programming/2017/05/14/accessing-wpf-listbox-selecteditems-using-mvvm.html)
[Ref](http://blogs.microsoft.co.il/miziel/2014/05/02/wpf-binding-listbox-selecteditems-attached-property-vs-style/)
[Ref](https://tyrrrz.me/blog/wpf-listbox-selecteditems-twoway-binding)
