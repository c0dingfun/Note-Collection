IEnumerble.AsEnumerable() why?
====

>` https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.asenumerable?view=netframework-4.8

AsEnumerable`<TSource>`(IEnumerable`<TSource>`) can be used to choose between query implementations when a sequence implements IEnumerable`<T>` but also has a different set of public query methods available. 

For example, given a generic class Table that implements IEnumerable`<T>` and has its own methods such as Where, Select, and SelectMany, a call to Where would invoke the public Where method of Table. A Table type that represents a database table could have a Where method that takes the predicate argument as an expression tree and converts the tree to SQL for remote execution. If remote execution is not desired, for example because the predicate invokes a local method, the AsEnumerable method can be used to hide the custom methods and instead make the standard query operators available.

```csharp
namespace AsEnumerableExample
{
    class Clump<T> : List<T>
    {
        // custom implementation of Where()

        // Because Enumerable.Where() takes a Func<T, bool>, so we can not
        // use Predicate<T> here! which is equivalent to Func<T, bool> anyway.
        // public IEnumerable<T> Where(Predicate<T> predicate)
        public IEnumerable<T> Where(Func<T, bool> predicate)
        {
            Console.WriteLine($"In Clump's Where() Impl");
            return Enumerable.Where(this, predicate);
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            AsEnumerableExample();
            Console.Read();
        }

        private static void AsEnumerableExample()
        {
            // create a new Clump<T> instance
            Clump<string> fruitClump =
                new Clump<string>
                {
                    "apple", "passionFruit", "banana", "mango", "orange", "blueberry", "grape", "strawberry"
                };

            // First call to Where(): - fruitClump's
            IEnumerable<string> query1 = fruitClump.Where(fruit => fruit.Contains("o"));

            foreach(var q in query1)
            {
                Console.WriteLine(q);
            }

            // Second call to Where(): - Enumerable.Where is called, because
            // AsEnumerable() hides Clump's Where()
            IEnumerable<string> query2 =
                fruitClump.AsEnumerable().Where(fruit => fruit.Contains("o"));

            foreach(var q in query2)
            {
                Console.WriteLine(q);
            }
        }
    }
}
```

Output:

In Clump's Where() Impl
passionFruit
mango
orange
passionFruit
mango
orange
