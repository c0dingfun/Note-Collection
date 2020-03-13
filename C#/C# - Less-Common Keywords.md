# Less Common Keywords in C#

```csharp
// Constant: can not be changed after declaration
public const string YearBorn = "2050";

// ReadOnly: Can not be changed after the class' declaration---
// meaning it can be changed in the class constructor, but not after
private readonly string mYearBorn="3300";

// YearBorn belong to the Class, not the Class instance
public static string YearBorn = "2000";

// Params is used to take an arbitrary or variable number of arguments
public static int AddAll(params int[] numbers) {...}
```
