# Patterns in OO (Object Oriented Programming)

Factory (Creational - to create objects, provides more flexiblity and control over object creation)
Strategy (Behavioral - Define communication betwee jobject to facilitate the flow of a more complex system)
Observer
Decorator (Structural - compose objects into larger structures to meet complex requirements)
Template Method

Promotes: OO, Loosely-Coupled, faster development, flexibility, maintainability adn reusability.

[credit](http://www.dofactory.com/net/visitor-design-pattern)

[patterns]]
* Creational 
 - Abstract Factory::
 - Builder: Director to create many parts
 - Factory Method::an object that needs many different objects, but instead of crating those objects as concrete objects, which tightly coupling them together, it can use a factory object (with a defined interface with a factory method) to do the creation of the objects. That way, the object do have a tight coupling with the factory object, but as to the objects it needs to use, they are not tightly coupled. (similar to the visitor pattern, except this pattern initiate the object creation, while visitor pattern does not.) 
 - Prototype::
 - Singleton::To ensure that a class has only one instance.

* Structural
 - Adapter
 - Bridge
 - Composite
 - Decorator: Base Feature + Special Feature by building on the Base; kind of a Class wrapping another class to provide different behavior
 - Facade:: grouping lot of low level operations into an object with a few high level methods. For example, instead of calling individual objects to accomplish a series of operations, provide a high level object that will execute methods calling, so that the user would just call that high level method to accomplish thoe tidious operations, everytime.
 - Flyweight
 - Proxy:: WCF is an example of using proxy pattern

* Behavioral
 - Chain of Responsibility
 - Command: ICommand in WPF, separating the invoker and the execution
 - Interpreter
 - Iterator::Iterable (get iterator), Iterator (next(), isDone()) 
 - Mediator: objects don't directly interact with each other but via a Mediator object who do the interaction on everyone's behalf. Chat Application is a good application of it.
 - Memento
 - Observer:: The Hollywood Principle (Don't call us, we will call you), aka Pub-Sub. 
 - State: Main object contains a State (of type IState) assigned with different StateObjects. Main methods invoke the State object's corresponding method to provide different behavior based on the current State (the state object). And each State Object contains the reference to the main object, so that it can set the State property of the Main object.
 - Strategy:: very similar to visitor pattern
 - Template Method
 - Visitor:: a generic object that accepts different visitor objects which implement specific functionalities.
   - eg: a generic Text object that needs to export to HTML, LaTex, PlainText, etc. formats. Instead of Text object implements all these different exports (exportHTML, exportLaTex, etc.) which makes the Text object prone to future changes, we can have Text object to accept an interface based Visitor object that provide its specific export functionality; for example, exportHTML(). In this case, the interface can be call IExport, with Export() as its method. Then, we can have HTMLExporter object (as the visitor) and LatexExporter object that are passed to the Text object.

## [Strategy (Provider, Behavior)](https://en.wikipedia.org/wiki/Strategy_pattern)

```csharp
public class StrategyPatternWiki
{
    public static void Main(String[] args)
    {
        // Prepare strategies
        var normalStrategy    = new NormalStrategy();
        var happyHourStrategy = new HappyHourStrategy();

        var firstCustomer = new Customer(normalStrategy);
        firstCustomer.Add(1.0, 1); // Normal billing

        firstCustomer.Strategy = happyHourStrategy; // Start Happy Hour
        firstCustomer.Add(1.0, 2);

        Customer secondCustomer = new Customer(happyHourStrategy); // New Customer
        secondCustomer.Add(0.8, 1);

        firstCustomer.PrintBill(); // The Customer pays

        secondCustomer.Strategy = normalStrategy; // End Happy Hour
        secondCustomer.Add(1.3, 2);
        secondCustomer.Add(2.5, 1);
        secondCustomer.PrintBill();
    }
}

class Customer
{
    private IList<double> drinks;
    public IBillingStrategy Strategy { get; set; } // Get/Set Strategy

    public Customer(IBillingStrategy strategy)
    {
        this.drinks = new List<double>();
        this.Strategy = strategy;
    }

    public void Add(double price, int quantity)
    {
        this.drinks.Add(this.Strategy.GetActPrice(price * quantity));
    }

    public void PrintBill() // Payment of bill
    {
        double sum = 0;
        foreach (var drinkCost in this.drinks)
            sum += drinkCost;

        Console.WriteLine($"Total due: {sum}.");
        this.drinks.Clear();
    }
}

interface IBillingStrategy
{
    double GetActPrice(double rawPrice);
}

// Normal billing strategy (unchanged price)
class NormalStrategy : IBillingStrategy
{
    public double GetActPrice(double rawPrice) => rawPrice;
}

// Strategy for Happy hour (50% discount)
class HappyHourStrategy : IBillingStrategy
{
    public double GetActPrice(double rawPrice) => rawPrice * 0.5;
}

```
