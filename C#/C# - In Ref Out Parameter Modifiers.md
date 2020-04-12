in, ref, out parameter modifiers
====

> https://www.pluralsight.com/guides/csharp-in-out-ref-parameters

ref is used to state that the parameter passed may be modified by the method.
in is used to state that the parameter passed cannot be modified by the method.
out is used to state that the parameter passed must be modified by the method.

Modifiers Are Not Allowed on Async Methods and Iterator Methods
It's important to note that in, out, and ref cannot be used in methods with the async modifier. You can use them in synchronous methods that return a Task, though.

You cannot use them in iterator methods that have a yield return or yield break either.

Overloading Methods is ALLOWED with Modifiers

When overloading a method in C#, using a modifier will be considered a different method signature than not using a modifier at all. You cannot overload a method if the only difference between methods is the type of modifier used. This will result in a compile error.