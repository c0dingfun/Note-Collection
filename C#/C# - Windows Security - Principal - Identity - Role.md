Windows Security
====

- ASP.Net

```csharp
HTTPContext.User.Identity.Name;
HTTPContext.User.Identity.Authenticated;
HTTPContext.User.IsInRole(@"Administrators");

```

- Windows

```csharp
WindowsPrincipal wp = 
    new WindowsPrincipal(WindowsIdentity.GetCurrent());
String username = wp.Identity.Name;
bool b = wp.IsInRole(@"Admin");
```