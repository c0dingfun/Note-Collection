Debugging in Visual Studio (Debugger.IsAttached)
====

```csharp
if (!Debugger.IsAttached)
    Debugger.Launch();
```

> https://stackoverflow.com/questions/6670415/how-to-set-conditional-breakpoints-in-visual-studio

```csharp
#if DEBUG
if (your_condition)
{
    System.Diagnostics.Break();
}
#endif
```

> https://docs.microsoft.com/en-us/dotnet/api/system.diagnostics.debugger?view=netframework-4.8
