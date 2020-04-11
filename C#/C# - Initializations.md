C# Initializations
====

Array, different way to instantiate an empty array
----

1. T[] array = new T[]{};

    ```csharp
    return new int[]{};
    ```

2. T[] array = new T[0];

    ```csharp
    return new int[0];
    ```

3. T[] array = Array.Empty`<T>`();

    ```csharp
    return Array.Empty<int>();
    ```

4. T[] array = Enumerable.Empty`<T>`().ToArray();

    ```csharp
    return Enumerable.Empty<int>().ToArray();
    ```

5. T[] array = Enumerable.Repeat(0,0).ToArray();

    ```csharp
    return Enumerable.Repeat(0, 0).ToArray();
    ```

[Ref](https://www.techiedelight.com/declare-empty-array-csharp/)

Object, Initializer
----

