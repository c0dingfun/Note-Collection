Yield Return v Yield Break vs Break
====

```csharp

/// <summary>
/// Yields numbers from 0 to 9
/// </summary>
/// <returns>{0,1,2,3,4,5,6,7,8,9}</returns>
public static IEnumerable<int> YieldBreak()
{
    for (int i = 0; ; i++)
    {
        if (i < 10)
        {
            yield return i; // Yields a number
        }
        else
        {
            // Indicates that the iteration has ended, everything 
            // from this line onward will be ignored
            yield break;
        }
    }
    yield return 10; // This will never get executed
}

/// <summary>
/// Yields numbers from 0 to 10
/// </summary>
/// <returns>{0,1,2,3,4,5,6,7,8,9,10}</returns>
public static IEnumerable<int> Break()
{
    for (int i = 0; ; i++)
    {
        if (i < 10)
        {
            yield return i; // Yields a number
        }
        else
        {
            // Terminates just the loop
            break;
        }
    }
    // Execution continues
    yield return 10;
}
```

Pasted from <https://riptutorial.com/csharp/example/29811/the-difference-between-break-and-yield-break> 




