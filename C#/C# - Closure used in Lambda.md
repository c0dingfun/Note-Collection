Closure used in Lambda
====


`Func<TN, bool> predicate = (tn) => tn.Parent.Identity.Equals(identity);`

Example:
----

```csharp

Func<SmartStickSubsystem, bool> smartStickPredicate = 
    (smartStick) => smartStick.SlotId.Equals(mStationSlot.SlotId);
```
