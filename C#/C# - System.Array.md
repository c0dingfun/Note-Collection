C# - System Array Class
====

Implemented Interfaces
----

```csharp

[System.Runtime.InteropServices.ComVisible(true)]
[System.Serializable]
public abstract class Array :
        ICloneable,
        System.Collections.IList,
        System.Collections.IStructuralComparable,
        System.Collections.IStructuralEquatable

    //
    // Summary:
    //     Provides methods for creating, manipulating, searching, and sorting arrays, thereby
    //     serving as the base class for all arrays in the common language runtime.
    public abstract class Array : 
                ICollection, 
                IEnumerable, 
                IList, 
                IStructuralComparable, 
                IStructuralEquatable, 
                ICloneable
    {
        ...

    public interface ICloneable { object Clone(); }

    public interface IStructuralEquatable
    {
        bool Equals(object other, IEqualityComparer comparer);
        int GetHashCode(IEqualityComparer comparer);
    }

    public interface IStructuralComparable
    {
        int CompareTo(object other, IComparer comparer);
    }

    [DefaultMember("Item")]
    public interface IList : ICollection, IEnumerable
    {
        object this[int index] { get; set; }
        bool IsFixedSize { get; }
        bool IsReadOnly { get; }
        int Add(object value);
        void Clear();
        bool Contains(object value);
        int IndexOf(object value);
        void Insert(int index, object value);
        void Remove(object value);
        void RemoveAt(int index);
    }

    public interface IEnumerable { IEnumerator GetEnumerator(); }

    public interface ICollection : IEnumerable
    {
        int Count { get; }
        bool IsSynchronized { get; }
        object SyncRoot { get; }
        void CopyTo(Array array, int index);
    }

```

```csharp
    [DefaultMember("Item")]
    public class ReadOnlyCollection<T> : 
                    ICollection<T>, 
                    IEnumerable<T>, 
                    IEnumerable, 
                    IList<T>, 
                    IReadOnlyCollection<T>, 
                    IReadOnlyList<T>, 
                    ICollection, 
                    IList
    {
        public ReadOnlyCollection(IList<T> list);
        public T this[int index] { get; }
        public int Count { get; }
        protected IList<T> Items { get; }
        public bool Contains(T value);
        public void CopyTo(T[] array, int index);
        public IEnumerator<T> GetEnumerator();
        public int IndexOf(T value);
    }
```

Properties
----

IsFixedSize
IsReadOnly
IsSynchronized
Length
LongLength
Rank
SyncRoot
ICollection.Count
IList.Item[Int32]

Methods
----


AsReadOnly
BinarySearch
Clear
Clone
ConstrainedCopy
ConvertAll
Copy
CopyTo
CreateInstance
Empty
Exists
Find
FindAll
FindIndex
FindLast
FindLastIndex
ForEach
GetEnumerator
GetLength
GetLongLength
GetLowerBound
GetUpperBound
GetValue
IndexOf
Initialize
LastIndexOf
Resize
Reverse
SetValue
Sort
TrueForAll
