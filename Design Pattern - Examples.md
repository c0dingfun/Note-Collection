
# [Strategy (Provider, Behavior)](https://en.wikipedia.org/wiki/Strategy_pattern)

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
