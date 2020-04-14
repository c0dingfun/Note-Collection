Arrays, String, Char, and StringBuilder
====

- String is immutable, to manipulate it or modifying it, we can either use Array of Char or StringBuilder.
- That's why understand Arrays, Char and StringBuilder is important, in addition to String.

Arrays
----

- Store multiple variables of the **SAME Type**
- Declaration specify the Type of its elements
- Or type can be determined via **implicit type**, just like var
- If want to store any Type, use "Object" as its type.

- type[] array; // declaration of array

```csharp
// Declare a single-dimensional array.
int[] array1 = new int[5]; // and instantiate it

// Declare and initialize array elements with values.
int[] array2 = new int[] { 1, 3, 5, 7, 9 };
int[] array22 = new [] { 1, 3, 5, 7, 9 }; // inferred int

// Alternative syntax.
var array33 = new []{ 1, 3, 5, 7, 9 }; // inferred array of int
int[] array3 = { 1, 2, 3, 4, 5, 6 };   // inferred int

// Declare a two dimensional array.
int[,] multiDimensionalArray1 = new int[2, 3]; // instantiate

// Declare and set array element values.
int[,] multiDimensionalArray2 = { { 1, 2, 3 }, { 4, 5, 6 } };

// Declare a JAGGED array.
int[][] jaggedArray = new int[6][]; // MUST provide array size of 6

// Set the values of the first array in the jagged array structure.
jaggedArray[0] = new int[4] { 1, 2, 3, 4 };
```

Array Overview
----

- An array can be Single-Dimensional, Multidimensional or Jagged.
- The number of dimensions and the length of each dimension are established when the array instance is created. These values can't be changed during the lifetime of the instance.
- The default values of value type array elements are set to zero, and reference elements are set to null.
- A jagged array is an array of arrays, and therefore its elements are reference types and are initialized to null.
- Arrays are zero indexed: an array with n elements is indexed from 0 to n-1.
- Array elements can be of any type, including an array type.
- Array types are reference types derived from the abstract base type Array.
- Since this type implements IEnumerable and IEnumerable`<T>`, you can use foreach iteration on all arrays in C#

Arrays as Objects
----

- In C#, arrays are actually objects, and not just addressable regions of contiguous memory as in C and C++.
- Array is the abstract base type of all array types.
- You can use the properties and other class members that Array has.
- An example of this is using the **Length** property to get the length of an array.
- The following code assigns the length of the numbers array, which is 5, to a variable called lengthOfNumbers:

```csharp
int[] numbers = { 1, 2, 3, 4, 5 };
int lengthOfNumbers = numbers.Length;
```

- The Array class provides many other useful methods and properties for
- sorting,
- searching, and
- copying arrays.

Single-Dimensional Arrays
----

- You can declare a single-dimensional array of five integers as shown in the following example:

```csharp
int[] array = new int[5]; // produced: int[5] { 0, 0, 0, 0, 0 }
var array21 = new int[5]; // produced: int[5] { 0, 0, 0, 0, 0 }
```

- This array contains the elements from array[0] to array[4].
- The new operator is used to create the array and initialize the array elements to their default values. In this example, all the array elements are initialized to zero.

- An array that stores string elements can be declared in the same way. For example:

`string[] stringArray = new string[6];`

Array Initialization
----

- It is possible to initialize an array upon declaration, in which case, the length specifier is not needed because it is already supplied by the number of elements in the initialization list.
- For example:

```csharp
int[] array1 = new int[] { 1, 3, 5, 7, 9 }; // int[] without specifying length
```

- A string array can be initialized in the same way.
- The following is a declaration of a string array where each array element is initialized by a name of a day:

`string[] weekDays = new string[] { "Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };`

- When you initialize an array upon declaration, you can use the following shortcuts:

```csharp
int[] array2 = { 1, 3, 5, 7, 9 };
string[] weekDays2 = { "Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat" };

// Note: can not use "var" in these cases!
```

- It is possible to declare an array variable without initialization, but you must use the new operator when you assign an array to this variable. For example:

```csharp
int[] array3;
array3 = new int[] { 1, 3, 5, 7, 9 };   // OK
//array3 = {1, 3, 5, 7, 9};   // Error
```

MultiDimensional Arrays
----

- Arrays can have more than one dimension.
- For example, the following declaration creates a two-dimensional array of four rows and two columns.

`int[,] array = new int[4/*row*/, 2/*col*/];`

- The following declaration creates an array of three dimensions, 4, 2, and 3.

`int[, ,] array1 = new int[4, 2, 3];`

Array Initialization (MultiDimensions)
----

```csharp
// Two-dimensional array. (declare and init'ed)
int[,] array2D = new int[,] { { 1, 2 }, { 3, 4 }, { 5, 6 }, { 7, 8 } };

// The same array with dimensions specified.
int[,] array2Da = new int[4, 2] { { 1, 2 }, { 3, 4 }, { 5, 6 }, { 7, 8 } };

// A similar array with string elements.
string[,] array2Db = new string[3, 2]
{
  { "one", "two" },
  { "three", "four" },
  { "five", "six" }
};

// Three-dimensional array.
int[, ,] array3D = new int[,,] // [2, 2, 3]
{
  {
    { 1, 2, 3 },  // [0, 0, 0-2]
    { 4, 5, 6 }   // [0, 1, 0-2]
  },
  {
    { 7, 8, 9 }, 
    { 10, 11, 12 }
  }
};

// The same array with dimensions specified.
int[, ,] array3Da = new int[2, 2, 3]
{
  {
    { 1, 2, 3 },  // [0, 0, 0-2]
    { 4, 5, 6 }   // [0, 1, 0-2]
  },
  {
    { 7, 8, 9 }, 
    { 10, 11, 12 }
  }
};

// Accessing array elements.
System.Console.WriteLine(array2D[0, 0]);
System.Console.WriteLine(array2D[0, 1]);
System.Console.WriteLine(array2D[1, 0]);
System.Console.WriteLine(array2D[1, 1]);
System.Console.WriteLine(array2D[3, 0]);
System.Console.WriteLine(array2Db[1, 0]);
System.Console.WriteLine(array3Da[1, 0, 1]);
System.Console.WriteLine(array3D[1, 1, 2]);

// Getting the total count of elements in the array
var allLength = array3D.Length;

var total = 1;
for (int i = 0; i < array3D.Rank; i++) {
    total *= array3D.GetLength(i); // total count of elements in "i" dimension
}
System.Console.WriteLine("{0} equals {1}", allLength, total);

// Output:
// 1
// 2
// 3
// 4
// 7
// three
// 8
// 12
// 12 equals 12
```

- You also can initialize the array without specifying the rank.

`int[,] array4 = { { 1, 2 }, { 3, 4 }, { 5, 6 }, { 7, 8 } };`

- If you choose to declare an array variable without initialization, you must use the **new** operator to assign an array to the variable.

- The use of new is shown in the following example.


```csharp
int[,] array5;
array5 = new int[,] { { 1, 2 }, { 3, 4 }, { 5, 6 }, { 7, 8 } };   // OK
//array5 = {{1,2}, {3,4}, {5,6}, {7,8}};   // Error
```

- The following example assigns a value to a particular array element.

`array5[2, 1] = 25;`

- The following code example initializes the array elements to default values (except for jagged arrays, because jagged array is array of arrays, the arrays need to be initialized first; otherwise, they are null).

`int[,] array6 = new int[10, 10];`

Jagged Arrays
----

- A jagged array is an array whose elements are arrays; thus jagged array is called an "array of arrays".
- The elements of a jagged array can be of different dimensions and sizes.
- The following examples show how to declare, initialize, and access jagged arrays.

- The following is a declaration of a single-dimensional array that has three elements, each of which is a single-dimensional array of integers:

```csharp
int[][] jaggedArray = new int[3][]; // declaration and init the 1st dimension
var jaggedArray = new int[3][]; // the jagged array can be inferred.
```

- Before you can use jaggedArray, its elements must be initialized. You can initialize the elements like this:

```csharp
jaggedArray[0] = new int[5];
jaggedArray[1] = new int[4];
jaggedArray[2] = new int[2];
```

- Each of the elements is a single-dimensional array of integers.
- The first element is an array of 5 integers, the second is an array of 4 integers, and the third is an array of 2 integers.

- It is also possible to use initializers to fill the array elements with values, in which case you do not need the array size. For example:

```csharp
jaggedArray[0] = new int[] { 1, 3, 5, 7, 9 };
jaggedArray[1] = new int[] { 0, 2, 4, 6 };
jaggedArray[2] = new int[] { 11, 22 };

// or the "new int[]" can be shorthanded as "new []"
// jaggedArray[0] = new [] { 1, 3, 5, 7, 9 };
// jaggedArray[1] = new [] { 0, 2, 4, 6 };
// jaggedArray[2] = new [] { 11, 22 };

```

- You can also initialize the array upon declaration like this:

```csharp
int[][] jaggedArray2 = new int[][]
{
    new int[] { 1, 3, 5, 7, 9 },
    new int[] { 0, 2, 4, 6 },
    new int[] { 11, 22 }
};
```

- You can use the following shorthand form.
- **Notice** that you cannot omit the new operator from the elements initialization because there is no default initialization for the elements:

```csharp
int[][] jaggedArray3 =          // Short Hand
{
    new int[] { 1, 3, 5, 7, 9 },
    new int[] { 0, 2, 4, 6 },
    new int[] { 11, 22 }
};
```

- A jagged array is an array of arrays, and therefore its elements are reference types and are initialized to null.

- You can access individual array elements like these examples:

```csharp
// Assign 77 to the second element ([1]) of the first array ([0]):
jaggedArray3[0][1] = 77;

// Assign 88 to the second element ([1]) of the third array ([2]):
jaggedArray3[2][1] = 88;
```

- It is possible to mix jagged and multidimensional arrays.
- The following is a declaration and initialization of a single-dimensional jagged array that contains three two-dimensional array elements of different sizes.

```csharp
int[][,] jaggedArray4 = new int[3][,] // new int[3][,] is optional, can be short handed
{
    new int[,] { {1,3}, {5,7} },
    new int[,] { {0,2}, {4,6}, {8,10} },
    new int[,] { {11,22}, {99,88}, {0,9} }
};

// or short handed 
// int[][,] jaggedArray4 = 
// {
//     new int[,] { {1,3}, {5,7} },
//     new int[,] { {0,2}, {4,6}, {8,10} },
//     new int[,] { {11,22}, {99,88}, {0,9} }
// };
```

- You can access individual elements as shown in this example, which displays the value of the element [1,0] of the first array (value 5):

`System.Console.Write("{0}", jaggedArray4[0][1, 0]);`

- The method Length returns the **number of arrays** contained in the jagged array.
- Not **number of elements**
- For example, assuming you have declared the previous array, this line:

```csharp
jaggedArray4.Length; // return 3, NOT 16
```

- This example builds an array whose elements are themselves arrays. Each one of the array elements has a different size.

```csharp
// Declare the array of two elements.
int[][] arr = new int[2][];

// Initialize the elements.
arr[0] = new int[5] { 1, 3, 5, 7, 9 };
arr[1] = new int[4] { 2, 4, 6, 8 };
// produces:
// int[2][] { int[5] { 1, 3, 5, 7, 9 }, int[4] { 2, 4, 6, 8 } }

// Display the array elements.
for (int i = 0; i < arr.Length; i++)
{
    System.Console.Write("Element({0}): ", i);

    for (int j = 0; j < arr[i].Length; j++)
    {
        System.Console.Write("{0}{1}", arr[i][j], j == (arr[i].Length - 1) ? "" : " ");
    }
    System.Console.WriteLine();
}
// Keep the console window open in debug mode.
System.Console.WriteLine("Press any key to exit.");
System.Console.ReadKey();
/* Output:
    Element(0): 1 3 5 7 9
    Element(1): 2 4 6 8
*/
```

Multi-Dimension vs Jagged Array
----

```csharp
int[,] array2d;         // multi-dimension
int[][] array2jagged;   // jagged
```

- Note: Jagged array can have 3D array too!

```csharp
//int[][][] jagged3d = new int[][][]
int[][][] jagged3d =    //short hand
{
    new int[][] { new int[] { 111, 112 }, 
    new int[] { 121, 122, 123 } },
    new int[][] { new int[] { 211 } }
}

// produced:
// int[2][][] { int[2][] { int[2] { 111, 112 }, int[3] { 121, 122, 123 } }, int[1][] { int[1] { 211 } } }

// to access 
// jagged3d[0][1][2]

```

Using foreach() with Arrays
----

- The foreach statement provides a simple, clean way to iterate through the elements of an array.

- For single-dimensional arrays, the foreach statement processes elements in increasing index order, starting with index 0 and ending with index Length - 1:

```csharp
int[] numbers = { 4, 5, 6, 1, 2, 3, -2, -1, 0 };
foreach (int i in numbers)
{
    System.Console.Write("{0} ", i);
}
// Output: 4 5 6 1 2 3 -2 -1 0

```

- For multi-dimensional arrays, elements are traversed such that the indices of the rightmost dimension are increased first, then the next left dimension, and so on to the left:

```csharp
int[,] numbers2D = new int[3, 2] { { 9, 99 }, { 3, 33 }, { 5, 55 } };
// Or use the short form:
// int[,] numbers2D =
// {
//    { 9, 99 },  // row by row for iteration
//    { 3, 33 },
//    { 5, 55 }
//};

foreach (int i in numbers2D)
{
    System.Console.Write("{0} ", i);
}
// Output: 9 99 3 33 5 55
```

- However, with multidimensional arrays, using a nested for loop gives you more control over the order in which to process the array elements.

- NOTE: Also for jagged array, the iteration is done against the array [of arrays] first

```csharp
int[][] arr = new int[2][];
arr[0] = new int[5] { 1, 3, 5, 7, 9 };
arr[1] = new int[4] { 2, 4, 6, 8 };
// produced
// int[2][] { int[5] { 1, 3, 5, 7, 9 }, int[4] { 2, 4, 6, 8 } }
foreach (var aa in arr)  // the a = array
{
    foreach (var a in aa)
    {
        Console.WriteLine($"{a});
    }
};

// output
// 1
// 3
// 5
// 7
// 9
// 2
// 4
// 6
// 8
```

Passing Arrays as Arguments
----

- Arrays can be passed as arguments to method parameters. Because arrays are reference types, the method can change the value of the elements.

```csharp
void PrintArray(int[] arr)  // passing single dimension array
{
    // Method code.
}

void Print2DArray(int[,] arr) // passing 2-dimension array
{
    // Method code.
}

void PrintJagged(int[][] arr) // passing jagged array
{
  // code
}

```

Implicit Typed Arrays
----

- You can create an implicitly-typed array in which the type of the array instance is inferred from the elements specified in the array **initializer**.
- The rules for any implicitly-typed **variable** also apply to implicitly-typed **arrays**.

- Implicitly-typed arrays are usually used in query expressions together with anonymous types and object and collection initializers.

- The following examples show how to create an implicitly-typed array:

```csharp
// use new[] because var need to know it is a one-dimension array
var a = new[] { 1, 10, 100, 1000 }; // int[]
var b = new[] { "hello", null, "world" }; // string[]

// single-dimension jagged array
var c = new[]  
{ 
    new[]{1,2,3,4},
    new[]{5,6,7,8}
};

// or single-dimension jagged array (**not implicit for the dimenion**)
// int[][] c2 needs it!!!
int[][] c2 =
{ 
    new[]{1,2,3,4},
    new[]{5,6,7,8}
};

// jagged array of strings
var d = new[]  
{
    new[]{"Luca", "Mads", "Luke", "Dinesh"},
    new[]{"Karen", "Suma", "Frances"}
};


// implicit-typed for object, using object initializers
var contacts = new[]
{
    new {
        Name = " Eugene Zabokritski",
        PhoneNumbers = new[] { "206-555-0108", "425-555-0001" }
    },
    new {
        Name = " Hanying Feng",
        PhoneNumbers = new[] { "650-555-0199" }
    }
};
```

- In the previous example, notice that with implicitly-typed arrays, no square brackets are used on the **LEFT side** of the initialization statement.

- Note also that **jagged arrays** are initialized by using new [] just like single-dimension arrays.


Array Properties
----

|Property| Meaning|
|---| --- |
|SyncRoot|    Gets an object that can be used to synchronize access to the Array.|
|IsSynchronized    |Gets a value indicating whether access to the Array is synchronized (thread safe).|
|IsFixedSize|    Gets a value indicating whether the Array has a fixed size.|
|IsReadOnly|    Gets a value indicating whether the Array is read-only.|
|Length|    Gets the total number of elements in all the dimensions of the Array.|
|LongLength|    Gets a 64-bit integer that represents the total number of elements in all the dimensions of the Array.|
|Rank|    Gets the rank (number of dimensions) of the Array. For example, a one-dimensional array returns 1, a two-dimensional array returns 2, and so on.|

Array Methods
----
|* (has overloads) Method (Static)|Meaning|
|---|---|
|AsReadOnly`<T>`(T[])    |Returns a read-only wrapper for the specified array.|
|*BinarySearch(Array, ...Value...)| Static Method to do search on **SORTED** one-dimension array|
|Clear(Array, Int32 /*index*/, Int32 /*length*/)|    Sets a range of elements in an array to the default value of each element type.|
|ConstrainedCopy(Array, Int32 /*source index*/, Array, Int32 /* dest index*/, Int32 /*length*/)    |Copies a range of elements from an Array starting at the specified source index and pastes them to another Array starting at the specified destination index. Guarantees that all changes are undone if the copy does not succeed completely.|
|ConvertAll<TInput,TOutput>(TInput[], Converter<TInput,TOutput>)|    Converts an array of one type to an array of another type.|
|*Copy(Array, Array, Int32 /*length*/)   | Copies a range of elements from an Array starting at the first element and pastes them into another Array starting at the first element. The length is specified as a 32-bit integer.|
|CreateInstance(Type, Int32/*length*/)|    Creates a one-dimensional Array of the specified Type and length, with zero-based indexing.|
|Empty`<T>`()|    Returns an empty array.|
|Exists`<T>`(T[], Predicate`<T>` /*match*/)|    Determines whether the specified array contains elements that match the conditions defined by the specified predicate.|
|Find`<T>`(T[], Predicate`<T>`/*match*/)  |  Searches for an element that matches the conditions defined by the specified predicate, and returns the first occurrence within the entire Array.
|FindAll`<T>`(T[], Predicate`<T>`)|    Retrieves all the elements that match the conditions defined by the specified predicate.|
|FindIndex`<T>`(T[], Int32, Int32, Predicate`<T>`)|    Searches for an element that matches the conditions defined by the specified predicate, and returns the zero-based index of the first occurrence within the range of elements in the Array that starts at the specified index and contains the specified number of elements.|
|FindLast`<T>`(T[], Predicate`<T>`)|    Searches for an element that matches the conditions defined by the specified predicate, and returns the last occurrence within the entire Array.|
|FindLastIndex`<T>`(T[], Int32, Int32, Predicate`<T>`)|    Searches for an element that matches the conditions defined by the specified predicate, and returns the zero-based index of the last occurrence within the range of elements in the Array that contains the specified number of elements and ends at the specified index.|
|ForEach`<T>`(T[], Action`<T>`)|    Performs the specified action on each element of the specified array (in-place).|
|IndexOf(Array, Object)|    Searches for the specified object and returns the index of its first occurrence in a one-dimensional array.|
|LastIndexOf(Array, Object)|    Searches for the specified object and returns the index of the last occurrence within the entire one-dimensional Array.|
|Resize`<T>`(T[], Int32/*new size*/)|    Changes the number of elements of a one-dimensional array to the specified new size.|
| Reverse(Array)|    Reverses the sequence of the elements in the entire one-dimensional Array.|
|Reverse(Array, Int32 /*index*/, Int32/*length*/)|    Reverses the sequence of a subset of the elements in the one-dimensional Array.
|*Sort(Array)|    Sorts the elements in an entire one-dimensional Array using the IComparable implementation of each element of the Array.|
|TrueForAll`<T>`(T[], Predicate`<T>`)|    Determines whether every element in the array matches the conditions defined by the specified predicate.|

|Method (Instance)|Meaning|
|---|---|
|Clone()|    Creates a shallow copy of the Array.|
|GetEnumerator()|    Returns an IEnumerator for the Array.|
|GetLength(Int32 /*dimension*/)|    Gets a 32-bit integer that represents the number of elements in the specified dimension of the Array.|
|GetLongLength(Int32 /* dimension */)|    Gets a 64-bit integer that represents the number of elements in the specified dimension of the Array.|
|GetLowerBound(Int32)|    Gets the index of the first element of the specified dimension in the array.|
|GetUpperBound(Int32)|    Gets the index of the last element of the specified dimension in the array.|
|object? GetValue(Int32)|    Gets the value at the specified position in the one-dimensional Array. The index is specified as a 32-bit integer.|
|Initialize()|    Initializes every element of the value-type Array by calling the parameterless constructor of the value type.|
|*SetValue(Object, Int32)|    Sets a value to the element at the specified position in the one-dimensional Array. The index is specified as a 32-bit integer.|


- Explicit Interface Implementations

|Method|Meaning|
|---|---|
|IStructuralComparable.CompareTo(Object, IComparer)|    Determines whether the current collection object precedes, occurs in the same position as, or follows another object in the sort order.|
|IStructuralEquatable.Equals(Object, IEqualityComparer)|    Determines whether an object is equal to the current instance.|
|IStructuralEquatable.GetHashCode(IEqualityComparer)|    Returns a hash code for the current instance.|

Extension Methods

|Method|Meaning|
|---|---|
|Cast<TResult>(IEnumerable)|    Casts the elements of an IEnumerable to the specified type.|
|OfType<TResult>(IEnumerable)|    Filters the elements of an IEnumerable based on a specified type.|
|AsParallel(IEnumerable)|    Enables parallelization of a query.|
|AsQueryable(IEnumerable)|    Converts an IEnumerable to an IQueryable.|

String
----

- A string is an object of type String whose value is text.
- Internally, the text is stored as a sequential read-only collection of Char objects.
- There is no null-terminating character at the end of a C# string; therefore a C# string can contain any number of embedded null characters ('\0').
- The Length property of a string represents the number of Char objects it contains, not the number of Unicode characters.
- To access the individual Unicode code points in a string, use the StringInfo object.

string vs System.String
----

- In C#, the string keyword is an alias for String.  Therefore, String and string are equivalent, and you can use whichever naming convention you prefer.
- The String class provides many methods for safely creating, manipulating, and comparing strings.
- In addition, the C# language overloads some operators to simplify common string operations.

Declaring and Initializing Strings
----

```csharp
string message1; // Declare without initializing.
string message2 = null; // Initialize to null.

// Initialize as an empty string.
// Use the Empty constant instead of the literal "".
string message3 = System.String.Empty;

// Initialize with a regular string literal.
string oldPath = "c:\\Program Files\\Microsoft Visual Studio 8.0";

// Initialize with a verbatim string literal.
string newPath = @"c:\Program Files\Microsoft Visual Studio 9.0";

// Use System.String if you prefer. string == System.String
System.String greeting = "Hello World!";

// In local variables (i.e. within a method body) you can use implicit typing.
var temp = "I'm still a strongly-typed System.String!";

// Use a const string to prevent 'message4' from
// being used to store another string value.
const string message4 = "You can't get rid of me!";

// Use the String constructor only when creating
// a string from a char*, char[], or sbyte*. See
// System.String documentation for details.
char[] letters = { 'A', 'B', 'C' };
string alphabet = new string(letters);
```

- Note that you do not use the new operator to create a string object except when initializing the string with an array of chars.

- Initialize a string with the Empty constant value to create a new String object whose string is of zero length.
- The string literal representation of a zero-length string is "".
- By initializing strings with the Empty value instead of null, you can reduce the chances of a NullReferenceException occurring.
- Use the static IsNullOrEmpty(String) method to verify the value of a string before you try to access it.

Immutability of String Objects
----

- String objects are **immutable**:
- they cannot be changed after they have been created.
- All of the String methods and C# operators that appear to modify a string actually return the results in a **new** string object.
- In the following example, when the contents of s1 and s2 are concatenated to form a single string, the two original strings are unmodified.
- The += operator creates a **new** string that contains the combined contents.
- That new object is assigned to the variable s1, and the original object that was assigned to s1 is released for garbage collection because no other variable holds a reference to it.

```csharp
string s1 = "A string is more ";
string s2 = "than the sum of its chars.";

// Concatenate s1 and s2. This actually creates a new
// string object and stores it in s1, releasing the
// reference to the original object.
s1 += s2;

System.Console.WriteLine(s1);
// Output: A string is more than the sum of its chars.
```

- Because a string "modification" is actually a new string creation, you must use caution when you create references to strings.
- If you create a reference to a string, and then "modify" the original string, the reference will continue to point to the original object instead of the new object that was created when the string was modified. The following code illustrates this behavior:

```csharp
string s1 = "Hello ";
string s2 = s1;
s1 += "World";

System.Console.WriteLine(s2);
//Output: Hello
```

Regular and Verbatim String Literals
----

- Use regular string literals when you must embed escape characters provided by C#, as shown in the following example:

```csharp
string columns = "Column 1\tColumn 2\tColumn 3";
//Output: Column 1        Column 2        Column 3

string rows = "Row 1\r\nRow 2\r\nRow 3";
/* Output:
  Row 1
  Row 2
  Row 3
*/

string title = "\"The \u00C6olean Harp\", by Samuel Taylor Coleridge";
//Output: "The Æolean Harp", by Samuel Taylor Coleridge
```csharp

- Use verbatim strings for convenience and better readability when the string text contains backslash characters, for example in file paths.
- Because verbatim strings preserve new line characters as part of the string text, they can be used to initialize multiline strings.
- Use double quotation marks to embed a quotation mark inside a verbatim string.
- The following example shows some common uses for verbatim strings:

```csharp
string filePath = @"C:\Users\scoleridge\Documents\";
//Output: C:\Users\scoleridge\Documents\

string text = @"My pensive SARA ! thy soft cheek reclined
    Thus on mine arm, most soothing sweet it is
    To sit beside our Cot,...";
/* Output:
My pensive SARA ! thy soft cheek reclined
   Thus on mine arm, most soothing sweet it is
   To sit beside our Cot,...
*/

string quote = @"Her name was ""Sara.""";
//Output: Her name was "Sara."
```

Format Strings
----

- A format string is a string whose contents are determined dynamically at runtime.
- Format strings are created by embedding interpolated expressions or placeholders inside of braces within a string.
- Everything inside the braces ({...}) will be resolved to a value and output as a formatted string at runtime.
- There are two methods to create format strings: string interpolation and composite formatting.

String Interpolation
----

- Available in C# 6.0 and later, interpolated strings are identified by the $ special character and include interpolated expressions in braces.

- Use string interpolation to improve the readability and maintainability of your code.
- String interpolation achieves the same results as the String.Format method, but improves ease of use and inline clarity.


```csharp
// a tuple
var jh = (firstName: "Jupiter", lastName: "Hammon", born: 1711, published: 1761);

Console.WriteLine($"{jh.firstName} {jh.lastName} was an African American poet born in {jh.born}.");
Console.WriteLine($"He was first published in {jh.published} at the age of {jh.published - jh.born}.");
Console.WriteLine($"He'd be over {Math.Round((2018d - jh.born) / 100d) * 100d} years old today.");

// Output:
// Jupiter Hammon was an African American poet born in 1711.
// He was first published in 1761 at the age of 50.
// He'd be over 300 years old today.
```

Formatting String in String Interpolation
----

- Example

`Console.WriteLine($"On {date:d}, the price of {item} was {price:C2} per {unit}.");`

- d: standard date and time formatting string
- t: display the short time format
- y: display the year and month
- yyyy: display the year as four digit number

- C2: standard numeric formatting string
- e: for exponential notation
- F3: for numeric value with 3 digits after the decimal point

Ref: 
> https://docs.microsoft.com/en-us/dotnet/standard/base-types/formatting-types

Field Width and Alignment of String Interpolation Expressions
----

- **Alignment** and **Field Width** : adding a comma (",") after an interpolation expression and designating the minimum field width. 
- Positive number: the field is Right-Aligned
- Negative number: the field is Left-Aligned

- Can combine an alignment specifier and a format string for a single interpolation expression. To do that, specify the alignment first, followed by a colon and the format string. 

`Console.WriteLine($"[{DateTime.Now,-20:d}] Hour [{DateTime.Now,-10:HH}] [{1063.342,15:N2}] feet");`

- Output:

```csharp
[04/14/2018          ] Hour [16        ] [       1,063.34] feet
```

Composite Formatting
----

- The String.Format utilizes placeholders in braces to create a format string.
- This example results in similar output to the string interpolation method used above.

```csharp
var pw = (firstName: "Phillis", lastName: "Wheatley", born: 1753, published: 1773);
Console.WriteLine("{0} {1} was an African American poet born in {2}.", pw.firstName, pw.lastName, pw.born);
Console.WriteLine("She was first published in {0} at the age of {1}.", pw.published, pw.published - pw.born);
Console.WriteLine("She'd be over {0} years old today.", Math.Round((2018d - pw.born) / 100d) * 100d);

// Output:
// Phillis Wheatley was an African American poet born in 1753.
// She was first published in 1773 at the age of 20.
// She'd be over 300 years old today.
```

Substrings
----

- A substring is any sequence of characters that is contained in a string.
- Use the Substring method to create a new string from a part of the original string.
- You can search for one or more occurrences of a substring by using the IndexOf method.
- Use the Replace method to replace all occurrences of a specified substring with a new string.
- Like the Substring method, Replace actually returns a new string and does not modify the original string.

```csharp
string s3 = "Visual C# Express";
System.Console.WriteLine(s3.Substring(7, 2));
// Output: "C#"

System.Console.WriteLine(s3.Replace("C#", "Basic"));
// Output: "Visual Basic Express"

// Index values are zero-based
int index = s3.IndexOf("C");
// index = 7
```

Accessing Individual Characters
----

- You can use array notation with an index value to acquire read-only access to individual characters, as in the following example:

```csharp
string s5 = "Printing backwards";

for (int i = 0; i < s5.Length; i++)
{
    System.Console.Write(s5[s5.Length - i - 1]);
}
// Output: "sdrawkcab gnitnirP"
```

- If the String methods do not provide the functionality that you must have to modify individual characters in a string, you can use a StringBuilder object to modify the individual chars "in-place", and then create a new string to store the results by using the StringBuilder methods.
- In the following example, assume that you must modify the original string in a particular way and then store the results for future use:

```csharp
string question = "hOW DOES mICROSOFT wORD DEAL WITH THE cAPS lOCK KEY?";
System.Text.StringBuilder sb = new System.Text.StringBuilder(question);

for (int j = 0; j < sb.Length; j++)
{
    if (System.Char.IsLower(sb[j]) == true)
        sb[j] = System.Char.ToUpper(sb[j]);

    else if (System.Char.IsUpper(sb[j]) == true)
        sb[j] = System.Char.ToLower(sb[j]);
}
// Store the new string.
string corrected = sb.ToString();
System.Console.WriteLine(corrected);
// Output: How does Microsoft Word deal with the Caps Lock key?   
```

Null Strings and Empty Strings
----

- An empty string is an instance of a System.String object that contains zero characters.
- Empty strings are used often in various programming scenarios to represent a blank text field.
-- You can call methods on empty strings because they are valid System.String objects.
- Empty strings are initialized as follows:


`string s = String.Empty;  `

- By contrast, a null string does not refer to an instance of a System.String object and any attempt to call a method on a null string causes a NullReferenceException.
- However, you can use null strings in concatenation and comparison operations with other strings.
- The following examples illustrate some cases in which a reference to a null string does and does not cause an exception to be thrown:

```csharp
string str = "hello";
string nullStr = null;
string emptyStr = String.Empty;

string tempStr = str + nullStr;
// Output of the following line: hello
Console.WriteLine(tempStr);

bool b = (emptyStr == nullStr);
// Output of the following line: False
Console.WriteLine(b);

// The following line creates a new empty string.
string newStr = emptyStr + nullStr;

// Null strings and empty strings behave differently. The following
// two lines display 0.
Console.WriteLine(emptyStr.Length);
Console.WriteLine(newStr.Length);
// The following line raises a NullReferenceException.
//Console.WriteLine(nullStr.Length);

// The null character can be displayed and counted, like other chars.
string s1 = "\x0" + "abc";
string s2 = "abc" + "\x0";
// Output of the following line: * abc*
Console.WriteLine("*" + s1 + "*");
// Output of the following line: *abc *
Console.WriteLine("*" + s2 + "*");
// Output of the following line: 4
Console.WriteLine(s2.Length);
```

Using StringBuilder for Fast String Creation
----

- String operations in .NET are highly optimized and in most cases do not significantly impact performance.
- However, in some scenarios such as tight loops that are executing many hundreds or thousands of times, string operations can affect performance.
- The StringBuilder class creates a string buffer that offers better performance if your program performs many string manipulations.
- The StringBuilder string also enables you to reassign individual characters, something the built-in string data type does not support.
- This code, for example, changes the content of a string without creating a new string:

```csharp
System.Text.StringBuilder sb = new System.Text.StringBuilder("Rat: the ideal pet");
sb[0] = 'C';    // StringBuilder implements the indexer
System.Console.WriteLine(sb.ToString());
System.Console.ReadLine();

//Outputs Cat: the ideal pet
```

- In this example, a StringBuilder object is used to create a string from a set of numeric types:

```csharp
var sb = new StringBuilder();

// Create a string composed of numbers 0 - 9
for (int i = 0; i < 10; i++)
{
    sb.Append(i.ToString());
}
Console.WriteLine(sb);  // displays 0123456789

// Copy one character of the string (not possible with a System.String)
sb[0] = sb[9];

Console.WriteLine(sb);  // displays 9123456789
Console.WriteLine();
```

Strings, Extension Methods and LINQ
----

- Because the String type implements IEnumerable`<T>`, you can use the extension methods defined in the Enumerable class on strings.
- To avoid visual clutter, these methods are excluded from IntelliSense for the String type, but they are available nevertheless.
- You can also use LINQ query expressions on strings.

Char
---

- The .NET Framework uses the Char structure to represent a Unicode character.
- The Unicode Standard identifies each Unicode character with a unique 21-bit scalar number called a code point, and defines the UTF-16 encoding form that specifies how a code point is encoded into a sequence of one or more 16-bit values.
- Each 16-bit value ranges from hexadecimal 0x0000 through 0xFFFF and is stored in a Char structure. - The value of a Char object is its 16-bit numeric (ordinal) value.

- The following sections examine the relationship between a Char object and a character and discuss some common tasks performed with Char instances.

Char objects, Unicode characters and strings
----

- A String object is a sequential collection of Char structures that represents a string of text.
- Most Unicode characters can be represented by a single Char object, but a character that is encoded as a base character, surrogate pair, and/or combining character sequence is represented by multiple Char objects.
- For this reason, a Char structure in a String object is not necessarily equivalent to a single Unicode character.

- Multiple 16-bit code units are used to represent single Unicode characters in the following cases:

- Glyphs, which may consist of a single character or of a base character followed by one or more combining characters.
- For example, the character ä is represented by a Char object whose code unit is U+0061 followed by a Char object whose code unit is U+0308. (The character ä can also be defined by a single Char object that has a code unit of U+00E4.)
- The following example illustrates that the character ä consists of two Char objects.

```csharp
StreamWriter sw = new StreamWriter("chars1.txt");
char[] chars = { '\u0061', '\u0308' };
string strng = new String(chars);
sw.WriteLine(strng);
sw.Close();
// The example produces the following output:
//       ä
```

- Characters outside the Unicode Basic Multilingual Plane (BMP).
- Unicode supports sixteen planes in addition to the BMP, which represents plane 0.
- A Unicode code point is represented in UTF-32 by a 21-bit value that includes the plane.
- For example, U+1D160 represents the MUSICAL SYMBOL EIGHTH NOTE character.
- Because UTF-16 encoding has only 16 bits, characters outside the BMP are represented by surrogate pairs in UTF-16.
- The following example illustrates that the UTF-32 equivalent of U+1D160, the MUSICAL SYMBOL EIGHTH NOTE character, is U+D834 U+DD60.
- U+D834 is the high surrogate; high surrogates range from U+D800 through U+DBFF.
- U+DD60 is the low surrogate; low surrogates range from U+DC00 through U+DFFF.

```csharp
public class Example
{
   public static void Main()
   {
      StreamWriter sw = new StreamWriter(@".\chars2.txt");
      int utf32 = 0x1D160;
      string surrogate = Char.ConvertFromUtf32(utf32);
      sw.WriteLine("U+{0:X6} UTF-32 = {1} ({2}) UTF-16",
                       utf32,        surrogate, ShowCodePoints(surrogate));
      sw.Close();                   
   }

   private static string ShowCodePoints(string value)
   {
      string retval = null;
      foreach (var ch in value)
         retval += String.Format("U+{0:X4} ", Convert.ToUInt16(ch));

      return retval.Trim();
   }
}
// The example produces the following output:
//       U+01D160 UTF-32 = ð (U+D834 U+DD60) UTF-16
```

Characters and character categories
----

- Each Unicode character or valid surrogate pair belongs to a Unicode category.
- In the .NET Framework, Unicode categories are represented by members of the UnicodeCategory enumeration and include values such as
- a) UnicodeCategory.CurrencySymbol,
- b) UnicodeCategory.LowercaseLetter, and
- c) UnicodeCategory.SpaceSeparator

- To determine the Unicode category of a character, you call the **GetUnicodeCategory** method.
- For example, the following example calls the GetUnicodeCategory to display the Unicode category of each character in a string.

```csharp
// Define a string with a variety of character categories.
String s = "The red car drove down the long, narrow, secluded road.";
// Determine the category of each character.
foreach (var ch in s)
   Console.WriteLine("'{0}': {1}", ch, Char.GetUnicodeCategory(ch));
```

- Internally, for characters outside the ASCII range (U+0000 through U+00FF), the GetUnicodeCategory method depends on Unicode categories reported by the CharUnicodeInfo class.
- Starting with the .NET Framework 4.6.2, Unicode characters are classified based on The Unicode Standard, Version 8.0.0.
- In versions of the .NET Framework from the .NET Framework 4 to the .NET Framework 4.6.1, they are classified based on The Unicode Standard, Version 6.3.0.

Characters and text elements
----

- Because a single character can be represented by multiple Char objects, it is not always meaningful to work with individual Char objects.
- For instance, the following example converts the Unicode code points that represent the Aegean numbers zero through 9 to UTF-16 encoded code units.
- Because it erroneously equates Char objects with characters, it inaccurately reports that the resulting string has 20 characters.

```csharp
string result = String.Empty;
for (int ctr = 0x10107; ctr <= 0x10110; ctr++)  // Range of Aegean numbers.
   result += Char.ConvertFromUtf32(ctr);

Console.WriteLine("The string contains {0} characters.", result.Length);
// The example displays the following output:
//     The string contains 20 characters.
```

- You can do the following to avoid the assumption that a Char object represents a single character.

- You can work with a String object in its entirety instead of working with its individual characters to represent and analyze linguistic content.

- You can use the **StringInfo** class to work with text elements instead of individual Char objects.
- The following example uses the **StringInfo** object to count the number of text elements in a string that consists of the Aegean numbers zero through nine.
- Because it considers a surrogate pair a single character, it correctly reports that the string contains ten characters.

```csharp
string result = String.Empty;
for (int ctr = 0x10107; ctr <= 0x10110; ctr++)  // Range of Aegean numbers.
   result += Char.ConvertFromUtf32(ctr);

StringInfo si = new StringInfo(result);
Console.WriteLine("The string contains {0} characters.",
                  si.LengthInTextElements);
// The example displays the following output:
//       The string contains 10 characters.
```

- If a string contains a base character that has one or more combining characters, you can call the String.Normalize method to convert the substring to a single UTF-16 encoded code unit.
- The following example calls the String.Normalize method to convert the base character U+0061 (LATIN SMALL LETTER A) and combining character U+0308 (COMBINING DIAERESIS) to U+00E4 (LATIN SMALL LETTER A WITH DIAERESIS).

```csharp
 public static void Main()
 {
    string combining = "\u0061\u0308";
    ShowString(combining);
   
    string normalized = combining.Normalize();
    ShowString(normalized);
 }

 private static void ShowString(string s)
 {
    Console.Write("Length of string: {0} (", s.Length);
    for (int ctr = 0; ctr < s.Length; ctr++) {
       Console.Write("U+{0:X4}", Convert.ToUInt16(s[ctr]));
       if (ctr != s.Length - 1) Console.Write(" ");
    }
    Console.WriteLine(")\n");
 }
// The example displays the following output:
//       Length of string: 2 (U+0061 U+0308)
//      
//       Length of string: 1 (U+00E4)
```

Common Char operations
----

- The Char structure provides methods to
- a) compare Char objects,
- b) convert the value of the current Char object to an object of another type, and
- c) determine the Unicode category of a Char object:

|What to do |Use these System.Char methods|
|---|---|
|Compare Char objects    |CompareTo and Equals|
|Convert a code point to a string    |ConvertFromUtf32|
|Convert a Char object or a surrogate pair of Char objects to a code point|For a single character: Convert.ToInt32(Char)<br> For a surrogate pair or a character in a string: Char.ConvertToUtf32|
|Get the Unicode category of a character|    GetUnicodeCategory|
|Determine whether a character is in a particular Unicode category such as digit, letter, punctuation, control character, and so on|    IsControl,<br> IsDigit,<br> IsHighSurrogate,<br> IsLetter,<br> IsLetterOrDigit,<br> IsLower,<br> IsLowSurrogate,<br> IsNumber,<br> IsPunctuation,<br> IsSeparator,<br> IsSurrogate,<br> IsSurrogatePair,<br> IsSymbol,<br> IsUpper,<br> and IsWhiteSpace|
|Convert a Char object that represents a number to a numeric value type    |GetNumericValue|
|Convert a character in a string into a Char object    |Parse and TryParse|
|Convert a Char object to a String object    |ToString|
|Change the case of a Char object|    ToLower,<br> ToLowerInvariant,<br> ToUpper,<br> and ToUpperInvariant|

|Property|Meaning|
|---|---|
|MaxValue| Largest possible value of a Char|
|MinValue| Smallest possible value of a Char|

|Method (Static)|Meaning|
|---|---|

Refs:

> https://docs.microsoft.com/en-us/dotnet/api/system.array?view=netcore-3.1
> https://docs.microsoft.com/en-us/dotnet/api/system.char?view=netcore-3.1
> https://docs.microsoft.com/en-us/dotnet/api/system.string?view=netcore-3.1
