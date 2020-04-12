Property--Auto-Implemented and Expression
====

Summary
----

1) Default Constructor is automatically provided, if no parameterized constructor is provided.

2) We only can use object-initializer to create instances when either

    a. We provide the default constructor
    b. No constructor in the class

3) Properties can be:

    a. Full Property with backing fields explicitly spell out
    b. Property can use => expression body for ReadONLY access in C# 6
    c. Property's get and set can use => expression together in C#7

A) Full Property with Backing Fields

```csharp
class TimePeriod
{
    private double _seconds; // backing field
    public double Hours
    {
        get { return _seconds / 3600; }
        set { 
            if (value < 0 || value > 24)
                throw new ArgumentOutOfRangeException(
                    $"{nameof(value)} must be between 0 and 24.");

            _seconds = value * 3600; 
        }
    }
}
```

B) Expression - for read-only property
    
```csharp

public class Person
{
    private string _firstName;
    private string _lastName;
    
    public Person(string first, string last)
    {
        _firstName = first;
        _lastName = last;
    }

    // Property can do "=>" expression
    public string Name => $"{_firstName} {_lastName}";    // C# 6
}

```

```csharp

public class SaleItem
{
    string _name;
    decimal _cost;

    public SaleItem(string name, decimal cost)
    {
        _name = name;
        _cost = cost;
    }

    public string Name 
    {
        get => _name;
        set => _name = value;
    }

    public decimal Price
    {
        // getter and setter can do "=>" expression together (must)
        get => _cost;
        set => _cost = value;   // C# 7
    }
}
```

C) Auto-Implemented Properties

```csharp
// for Simple straight-forward properties,
// we can use auto-implemented properties:
public class SaleItem
{
    // 2) here there is no constructor, so object-initializer must be used.
   public string Name { get; set; }
   public decimal Price { get; set; }
}

class Program
{
   static void Main(string[] args)
   {
     // we can only do this when SaleItem has no Constructor
      var item = new SaleItem{ Name = "Shoes", Price = 19.95m };
      Console.WriteLine($"{item.Name}: sells for {item.Price:C2}");
   }
}

```
