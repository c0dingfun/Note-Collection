# LINQ Cheatsheet (Originally from Internet)

//Linq Extension Methods Cheat Sheet
//January 17, 2013 LW C#, Cheat Sheet.NET, C#, Linq, Mono, MonoDevelop
//A cheat sheet for Linq Extension Methods in C#

**** AGGREGATE ****
----

```csharp

data.Count()                            // The count of items
data.Count(x => x.Condition == true)     // Count items which pass criteria
data.Sum (x => x.Value);                // Sum of all items
data.Min(x => a.Value);                 // Min value
data.Max(x => a.Value);                 // Max value
data.Average(x => a.Value);             // Average
data.Select(x => new { A = x.Name, B = x.Min(x.Value))   // Nested Aggregate
data.GroupBy( x => x.Type)
    .Select( y => new { 
        Key = y.Key,
        Average = y.Average ( z => z.Value ) 
    }); // Grouped Aggregate
```

**** CONVERSIONS ****
----

```csharp
data.ToArray();                     // Convert to Array
data.ToList();                      // Convert to List
data.ToDictionary( x=> x.Name );    // Convert to Dictionary keyed on Name
data.OfType<decimal> ().ToList ()   // Filters out all elements not of provided type
data.Cast<decimal> ().ToList ()     // Cast can only cast to Implemented Interfaces and classes 
                                    // within the hierarchy
                                    // Convert all items with provided conversion method
data.ConvertAll<double> (x => Convert.ToDouble (x));
```

**** ELEMENT ****
----

```csharp

data.First()                        // Returns the first element
data.First( x=> x.Type == Types.A ) // Returns the first element passing the condition
data.FirstOrDefault( )              // Returns the first element or default*
                                    // Returns the first element passing the condition or default*
data.FirstOrDefault( x => x.Type == Types.B )
data.Last()                         // Returns the last element
data.Last( x=> x.Type == Types.A )  // Returns the last element passing the condition
data.LastOrDefault( )               // Returns the last element or default*
                                    // Returns the last element passing the condition or default*
data.LastOrDefault( x => x.Type == Types.B )
data.ElementAt(0)                   // Returns the element at position 0
*default                            // Default is defined as default(typeOf(T)) or new 
                                    // Constructor() without any constructor parameters
```

**** FILTERS ****
----

```csharp

data.Where(x => x.Type == Types.A)  // Returns all elements passing the condition
                                    // The elements index can be passed into the delegate
data.Where(( x, index) => index <= 4 && x.Type == Types.A)
```

**** GENERATION ****
----

```csharp
Enumerable.Range(1, 10);            // Creates collection of 10 items between 1 and 10
Enumerable.Repeat(1, 10);           // Creates a collection of 10 1s.
```

**** GROUPING ****
----

```csharp
// Grouping is not like SQL where columns have to be aggregates or
// within the group list; you group the elements into a list for each group

data.GroupBy (x => x.Type)
    .Select (grp => new
                    {
                        Key = grp.Key,
                        Value = grp.Count ()
                    }); // Group with count of data

                    // Create a collection of elements ordered by name for each 
                    // group of Type

data.GroupBy (x => x.Type)
    .Select (grp => new
                    {
                        Key = grp.Key,  
                        Value = grp.OrderBy (x => x.Name)
                    });
```

**** ORDERING ****
----

```csharp
data.OrderBy (x => x.Name);                         // Order by Name ASC
data.OrderBy (x => x.Name).ThenBy (x => x.Age);     // Order by Name ASC the Age ASC
data.OrderBy (x => x.Name).ThenByDescending (x => x.Age); // Order by Name ASC then Age DESC
data.OrderByDescending (x => x.Name);               // Order by Name DESC
data.OrderBy (x => x.Name,                          // Order by Name ASC Case insensative
    StringComparer.CurrentCultureIgnoreCase);
data.OrderBy (x => x.Name).Reverse ();              // Reverse elements
```

**** PARTITIONING ****
----

```csharp

data.Take (3);                          // Take 3 items
data.Skip (3);                          // Skip 3 items
data.TakeWhile (x=>x.Type ==Types.A);   // Take all the items while the condition is met
data.SkipWhile (x=>x.Type ==Types.A);   // Skip all the items while the condition is met
```

**** PROJECTION ****
----

```csharp
data.Select (x => x.Name);                              // Select collection of a column
data.Select (x => new
                    {
                        Name = x.Name, 
                        Age = x.Age 
                    });  // Select a collection of columns through an anonymous type
data.SelectMany (x => x.Children) // SelectMany flattens collections into one collection
```

**** QUANTIFIERS ****
----

```csharp

// Returns True/False if any element meets the condition
data.Any (x => x.Type == Types.TypeA);

// Returns True/False if all the elements meet the condition
data.All (x => x.Type == Types.TypeA);

```
 
**** SET ****
----

```csharp

data.Distinct ();           // Returns a collection of distinct elements
data.Intersect(dataTwo);    // Returns the union / intersection of data; elements in both collections
data.Except(dataTwo);       // Returns elements in data which are not in dataTwo
data.Concat(dataTwo);       // Concatenates both collections; appends dataTwo to data
data.SequenceEqual(dataTwo));  // Returns True if data has the same data and sequence of dataTwo
data.Join(dataTwo,             // Joins the data together like SQL join
    x => x.Type,               // Join field on data
    y => y.Type,               // Join field on dataTwo
    ( d1, d2 ) => new { Name = d1.Name, Type = d1.Type, Desc = d2.Description} ); // Selection criteria
 
data.Distinct (new AComparer ()).               // Distinct with providing an equality provider
public class AComparer : IEqualityComparer<A>
{
    public bool Equals (A x, A y) { return a == a;}
    public int GetHashCode (A obj) { return obj.GetHashCode(); }
}
```
