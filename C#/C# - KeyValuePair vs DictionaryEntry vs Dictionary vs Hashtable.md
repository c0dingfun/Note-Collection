KeyValuePair<T1, T2> vs DictionaryEntry; Dictionary vs HashTable
====

> https://stackoverflow.com/questions/905424/keyvaluepair-vs-dictionaryentry

KeyValuePair<T,T> is for iterating through Dictionary<T,T>. 
This is the .Net 2 (and onwards) way of doing things.

DictionaryEntry is for iterating through HashTables. 
This is the .Net 1 way of doing things.

Here's an example:

```csharp

Dictionary<string, int> MyDictionary = new Dictionary<string, int>();
foreach (KeyValuePair<string, int> item in MyDictionary)
{
  // ...
}

Hashtable MyHashtable = new Hashtable();
foreach (DictionaryEntry item in MyHashtable)
{
  // ...

}
```