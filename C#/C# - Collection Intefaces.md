Collection Interfaces (Generics)
====

- C# provides many collection interfaces, abstracting collections, and they are for different "purposes".

1. IEnumerable`<T>`
2. ICollection`<T>`
3. IList`<T>`
4. IList, by the way, int[] implements IList!!!!

    ```dotnetcli
    public class List<T> :
        System.Collections.Generic.ICollection<T>,
        System.Collections.Generic.IEnumerable<T>,
        System.Collections.Generic.IList<T>,
        System.Collections.Generic.IReadOnlyCollection<T>, System.Collections.Generic.IReadOnlyList<T>,
        System.Collections.IList // <<<<
    ```

5. there are others like IDicitonary<TKey, TValue>

Summary
----

- IEnumerable`<T>`
  - Readonly, just for Enumeration.
  - No indexer, no (.Count, .Contains,...)

- ICollection`<T>`
  - Resizable (.Add, .Remove, .Clear, .Count...) 
  - No indexer.

- IList`<T>`
  - With Indexer
  - Resizable (.insert, . RemoveAt)

![picture](_images/IEnumIListEtc.png)
![picture](_images/generic-list2.png)
