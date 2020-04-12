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

IEnumerable`<T>`
----

IEnumerator`<T>`:
  T Current
  bool MoveNext( )
  void Reset ( )

IEnumerable`<T>`:
  IEnumerator`<T>` GetEnumerator ( )

ICollection`<T>`: IEnumerable`<T>`
  bool Contains(T element)
  void Add(T element)
  bool Remove(T element)
  void Clear()
  void CopyTo(T[] targetArray, int targetArrayStartIndex)
  int Count
  bool IsReadOnly

IList`<T>`: ICollection`<T>`, IEnumerable`<T>`
  T this[int index]
  int IndexOf(T element)
  void Insert(int index, T element)
  void RemoveAt(int index)

OLD Notes
----

> https://medium.com/@kunaltandon.kt/ienumerable-vs-icollection-vs-ilist-vs-iqueryable-in-c-2101351453dbIEnumerable

Namespace: System.Collections

An IEnumerable is a list or a container which can hold some items. You can iterate through each element in the IEnumerable. You can not edit the items like adding, deleting, updating, etc. instead you just use a container to contain a list of items. It is the most basic type of list container.
All you get in an IEnumerable is an enumerator that helps in iterating over the elements. An IEnumerable does not hold even the count of the items in the list, instead, you have to iterate over the elements to get the count of items.

An IEnumerable supports filtering elements using where clause.

ICollection
----
Namespace: System.Collections

ICollection is another type of collection, which derives from IEnumerable and extends it's functionality to add, remove, update element in the list. ICollection also holds the count of elements in it and we does not need to iterate over all elements to get total number of elements.

The count of total items can be retrieved in O(1) time.
ICollection supports enumerating over the elements, filtering elements, adding new elements, deleting existing elements, updating existing elements and getting the count of available items in the list.

IList
----
Namespace: System.Collections

IList extends ICollection. An IList can perform all operations combined from IEnumerable and ICollection, and some more operations like inserting or removing an element in the middle of a list.
You can use a foreach loop or a for loop to iterate over the elements.

IQueryable
----

Namespace: System.Linq

IQueryable extends IEnumerable. An IQueryable generates a LINQ to SQL expression that is executed over the database layer. Instead of the generating a Func<T, bool> like the ones above, IQueryable generates an expression tree and gives Expression<Func<T, bool>> that is executed over the database layer to get data set.