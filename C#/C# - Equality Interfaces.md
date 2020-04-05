C# - Equality Interfaces
====

- Object has Equality and Comparisons

```csharp

    static bool Equals(object objA, object objB)
    bool virtual Equals(object obj)

    static bool ReferenceEquals(object objA, object objB)
    virtual int GetHashCode()
```

- Equality and Comparison Interfaces

1. IEquatable<T> ( Interface for natural Equality)
2. IComparable; IComparable<T>
3. IComparer; IComparer<T>
4. IEqualityComparer; IEqualityComparer<T> ( Interface for Plugged-in Equality)
5. IStructuralEquatable
6. IStructuralComparable

- String is special, == performs value equality

```C#
    string str1 = "Value Checked";
    string str2 = string.copy(str1);
    str1==str2 // returns true.

    // But
    int i=10;
    int i1 = 10;

    i == i1 // returns true
    (object)i == (object) i1 // returns false
    (IComparable<int>) i == (IComparable<int>) i1 // returns false

    // But
    string str1 = "string"
    string str2 = "STRING"

    str1 ==str2 returns false
    str1.Equals(str2,StringComparison.OrdinalIgnoreCase) // returns true.
```

- Equality

  - Natural Equality - IEquatable<T> (String Implements)

    ```csharp
        IEquatable<T>
            bool Equals(T other)
    ```

  - Plugged-in Equality - IEqualityComparer<T>

    ```csharp
        IEqualityComparer<T>
            bool Equals(T x, T y)
            int GetHashCode(T obj)
    ```

- Comparison

  - Natural Comparison - IComparable<T>

    ```csharp
        IComparable<T>
            int CompareTo(T other)
    ```

  - Plugged-in Comparison
    
    ```csharp
        IComparer<T>
            int Compare(T x, T y)
    ```

- Object Reference Equality Comparison

    ```csharp
        objectA.Equals(objectB)
    ```

Equality Operator "==" (C#)
---

|Type | == |
|-----| ---|
|Ref.Type| Reference Equality|
|String| Value Equality|
|Value Type| Value Equality|
|Delegate| Value Equality|
|Tuple| Value Equality|

- Casting Value to Interface becomes Reference Type

- Equality "=="
- Comparison "<", "==", ">"

Virtual Object.Equals (.Net Framework)
----


- Null is always equal to another Null


The Interfaces

----
|  | Equality | Comparison|
|---|--- | ---|
|"Natural"|**IEquatable`<T>`** <br> bool Equals(T other)|**IComparable`<T>`**<br>int CompareTo(T other)<br> **IComparable**<br> int CompareTo(object obj) |
|"Plugged-in"|**IEqualityComparer`<T>`**<br>bool Equals(T x, T y)<br>int GetHashCode(T obj) <br> **IEqualityComparer**<br>bool Equals(object x, object y)<br>int GetHashCode(object obj)|**IComparer`<T>`**<br>int Compare(T x, T y) <br> **IComparer**<br>Compare(object x, object y) |
|"Structural"|**IStructuralEquatable**<br>bool Equals(object ohter, IEqualityComparer comparer)<br>int GetHashCode(IEqualityComparer comparer)|**IStructuralComparable**<br>int CompareTo(object other, IComparer comparer)|



[Flash Cards](https://quizlet.com/156000898/c-equality-and-comparisons-flash-cards/)
[Equality Story](https://www.c-sharpcorner.com/article/story-of-equality-in-net-part-one/)