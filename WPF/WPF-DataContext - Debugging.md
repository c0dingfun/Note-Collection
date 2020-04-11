
DataContext (Debugging)
====

> https://spin.atomicobject.com/2013/12/11/wpf-data-binding-debug/

=> use diagnostics
----

```csharp
<Window x:Class="WpfApplication1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:diag="clr-namespace:System.Diagnostics;assembly=WindowsBase"
        Title="MainWindow" Height="350" Width="525">
    <Grid>
        <TextBlock Text="{Binding ThereIsNoDataContext, 
            diag:PresentationTraceSources.TraceLevel=High}"/>
    </Grid>
</Window>
```
    
=> use Converter to break into it during binding
----

```csharp
    <Window x:Class="WpfApplication1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:WpfApplication1"
        Title="MainWindow" Height="350" Width="525">
        <Grid Name="root">
            <Grid.Resources>
                <local:DebugDataBindingConverter x:Key="DebugBinding"/>
            </Grid.Resources>
            <TextBlock Text="{Binding ActualWidth, ElementName=root, 
                Converter={StaticResource DebugBinding}}"/>
        </Grid>
    </Window>

```

=> Use Listener:
----

```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        PresentationTraceSources.Refresh();
        PresentationTraceSources.DataBindingSource.Listeners.Add(new ConsoleTraceListener());
        PresentationTraceSources.DataBindingSource.Listeners.Add(new DebugTraceListener());
        PresentationTraceSources.DataBindingSource.Switch.Level = SourceLevels.Warning | SourceLevels.Error;
        base.OnStartup(e);
    }
}

public class DebugTraceListener : TraceListener
{
    public override void Write(string message)
    {
    }

    public override void WriteLine(string message)
    {
        Debugger.Break();
    }
}
```

How To Debug Data Binding Issues in WPF (Details)
----

Data binding establishes a connection between the application UI and business logic. When it works, it's a wonderful thing. You no longer have to write code that updates your UI or pass values down to your business logic. When it breaks, it can be frustrating to figure out what went wrong. In this post, I will give you some tips on how you can debug your data bindings in WPF.

1. Add Tracing to the Output Window
Here is a sample TextBlock that has a missing data context. In this situation, you will not get any errors in the Visual Studio output window.

```csharp
<Window x:Class="WpfApplication1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="MainWindow" Height="350" Width="525">
    <Grid>
        <TextBlock Text="{Binding ThereIsNoDataContext}"/>
    </Grid>
</Window>
```

To enable tracing, I added a new xml namespace to include the System.Diagnostics namespace. You can also set the level of tracing to High, Medium, Low, or None. Now let's add some tracing to the output window to see what is wrong with the data binding.

```csharp

<Window x:Class="WpfApplication1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:diag="clr-namespace:System.Diagnostics;assembly=WindowsBase"
        Title="MainWindow" Height="350" Width="525">
    <Grid>
        <TextBlock Text="{Binding ThereIsNoDataContext, 
            diag:PresentationTraceSources.TraceLevel=High}"/>
    </Grid>
</Window>
```

Now the output window will contain the following helpful information:

System.Windows.Data Warning: 71 : BindingExpression (hash=38600745): DataContext is null

2. Attach a Value Converter to Break into the Debugger

When you don't see anything displayed in your UI, it is hard to tell whether it's data binding causing your issue or a problem with the visual layout of the control. You can eliminate the data binding as the problem by adding a value converter and break into the debugger.  If the value is what you expected, then data binding is not your issue.

Here is a simple value converter that breaks into the debugger.

```csharp

using System;
using System.Diagnostics;
using System.Globalization;
using System.Windows.Data;
 
namespace WpfApplication1
{
    public class DebugDataBindingConverter : IValueConverter
    {
        public object Convert(object value, Type targetType,
            object parameter, CultureInfo culture)
        {
            Debugger.Break();
            return value;
        }
 
        public object ConvertBack(object value, Type targetType,
            object parameter, CultureInfo culture)
        {
            Debugger.Break();
            return value;
        }
    }
}
```

To use the value converter, reference the namespace of the assembly that contains the converter and add an instance of it to the resources of your window. Now add the converter to your problematic data binding.

```csharp

<Window x:Class="WpfApplication1.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:WpfApplication1"
        Title="MainWindow" Height="350" Width="525">
    <Grid Name="root">
        <Grid.Resources>
            <local:DebugDataBindingConverter x:Key="DebugBinding"/>
        </Grid.Resources>
        <TextBlock Text="{Binding ActualWidth, ElementName=root, 
            Converter={StaticResource DebugBinding}}"/>
    </Grid>
</Window>
```

3. Know the Instant You Have a Data Binding Problem

Unless you are constantly checking every UI element and monitoring the output window for binding errors, you will not always catch that you have a data binding problem. An exception is not thrown when data binding breaks, so global exception handlers are of no use.

Wouldn't it be nice if you break into the debugger the instant you have a data binding error? By adding our own implementation of a  TraceListener that breaks into the debugger, we will get notified the next time we get a data binding error. I also added the default  ConsoleTraceListener alongside our new DebugTraceListener, so that our previous examples of tracing output would not be broken.

```csharp

public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        PresentationTraceSources.Refresh();
        PresentationTraceSources.DataBindingSource.Listeners.Add(new ConsoleTraceListener());
        PresentationTraceSources.DataBindingSource.Listeners.Add(new DebugTraceListener());
        PresentationTraceSources.DataBindingSource.Switch.Level = SourceLevels.Warning | SourceLevels.Error;
        base.OnStartup(e);
    }
}
 
public class DebugTraceListener : TraceListener
{
    public override void Write(string message)
    {
    }
 
    public override void WriteLine(string message)
    {
        Debugger.Break();
    }
}
```

With these tips, you should have a more pleasant data binding experience. Please share any additional debugging tips you may have in the comments.
