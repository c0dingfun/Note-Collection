Singleton Implementation (generic)
====

> https://csharpindepth.com/Articles/Singleton  https://stackoverflow.com/questions/100081/whats-a-good-threadsafe-singleton-generic-template-pattern-in-c-sharp

> https://social.msdn.microsoft.com/Forums/en-US/b6b8f2ce-b23d-4254-88b0-cf0701c67f29/help-on-creating-a-generic-singleton-class?forum=csharplanguage

A simple, yet effective means of creating a thread-safe singleton is to use a nested class to instantiate it. The following is an example of a lazy instantiation singleton:

```csharp
public sealed class Singleton
{ 
   private Singleton() { }

   public static Singleton Instance
   {
      get { return SingletonCreator.instance; }
   }

   private class SingletonCreator
   {
      static SingletonCreator() { } // static contructor
      internal static readonly Singleton instance = new Singleton();
   }
}
```

Usage:

```csharp

Singleton s1 = Singleton.Instance;
Singleton s2 = Singleton.Instance;

if (s1.Equals(s2))
{
    Console.WriteLine("Thread-Safe Singleton objects are the same");
}
```

Generic Solution:
----

```csharp

public class Singleton<T> where T : class, new()
{
    private Singleton() { }
    public static T Instance 
    { 
        get { return SingletonCreator.instance; } 
    } 

    private class SingletonCreator 
    {
        static SingletonCreator() { }
        internal static readonly T instance = new T();
    }
}
```

Usage:

```csharp

class TestClass { }

Singleton s1 = Singleton<TestClass>.Instance;
Singleton s2 = Singleton<TestClass>.Instance;
if (s1.Equals(s2))
{
    Console.WriteLine("Thread-Safe Generic Singleton objects are the same");
}
```

Lastly, here is a somewhat related and useful suggestion - to help avoid deadlocks that can be caused by using the lock keyword, consider adding the following attribute to help protect code in only public static methods:

```csharp

using System.Runtime.CompilerServices;
[MethodImpl (MethodImplOptions.Synchronized)]
public static void MySynchronizedMethod()
{
}
```

References:

C# Cookbook (O'Reilly), Jay Hilyard & Stephen Teilhet
C# 3.0 Design Patterns (O'Reilly), Judith Bishop
CSharp-Online.Net - Singleton design pattern: Thread-safe Singleton

```csharp

public sealed class Singleton
{
    private static readonly Singleton instance = new Singleton();     

    static Singleton() { }

    public static Singleton Instance
    { 
        get { return instance; } 
    }

    private Singleton() { } // can't be called, only statics here anyway
}


public sealed class Singleton
{
    public static Singleton Instance { get { return Nested.instance; } }

    private class Nested
    {
        static Nested() { }
        internal static readonly Singleton instance = new Singleton();
    }

    private Singleton() { }
}

public sealed class Singleton
{
    private static readonly Lazy<Singleton> lazy = 
                new Lazy<Singleton> (() => new Singleton());

    public static Singleton Instance { get { return lazy.Value; } }

    private Singleton() { }
}
```
