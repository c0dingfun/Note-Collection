WPF - Startup
====

App.xaml
----

```csharp

<Application x:Class="SimpleMVVMwpf.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:SimpleMVVMwpf"
             StartupUri="MainWindow.xaml">
```

App.xaml.cs 
----

```csharp
    public partial class App : Application { } 
```

App.xaml.cs (Generated) App.g.i.cs)
----

```csharp

/// <summary>
/// App
/// </summary>
public partial class App : System.Windows.Application {
    
    /// <summary>
    /// InitializeComponent
    /// </summary>
    [System.Diagnostics.DebuggerNonUserCodeAttribute()]
    [System.CodeDom.Compiler.GeneratedCodeAttribute("PresentationBuildTasks", "4.0.0.0")]
    public void InitializeComponent() {
        
        #line 5 "..\..\App.xaml"
        this.StartupUri = new System.Uri("MainWindow.xaml", System.UriKind.Relative);
        
        #line default
        #line hidden
    }
```

Application Entry Point
----

```csharp

    /// <summary>
    /// Application Entry Point.
    /// </summary>
    [System.STAThreadAttribute()]
    [System.Diagnostics.DebuggerNonUserCodeAttribute()]
    [System.CodeDom.Compiler.GeneratedCodeAttribute("PresentationBuildTasks", "4.0.0.0")]
    public static void Main() {
        SimpleMVVMwpf.App app = new SimpleMVVMwpf.App();
        app.InitializeComponent();
        app.Run();
    }
}

```