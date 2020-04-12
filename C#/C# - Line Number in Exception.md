Line Number in Exception
====

> https://stackoverflow.com/questions/3328990/c-sharp-get-line-number-which-threw-exception

Line number where exception is raised

```csharp

try
{
  //your code;
}
catch(Exception ex)
{
  MessageBox.Show(ex.StackTrace + " ---This is your line number, bro' :)", ex.Message);
}
```

If you need the line number for more than just the formatted stack trace you get from Exception.StackTrace, you can use the StackTrace class:

```csharp

try
{
    throw new Exception();
}
catch (Exception ex)
{
    // Get stack trace for the exception with source file information
    var st = new StackTrace(ex, true);
    // Get the top stack frame
    var frame = st.GetFrame(0);
    // Get the line number from the stack frame
    var line = frame.GetFileLineNumber();
}
```

Note that this will only work if there is a pdb file available for the assembly.
