Async Property
====

> https://stackoverflow.com/questions/6602244/how-to-call-an-async-method-from-a-getter-or-setter

> https://blog.stephencleary.com/2013/01/async-oop-3-properties.html

There is no technical reason that async properties are not allowed in C#. It was a purposeful design decision, because "asynchronous properties" is an oxymoron.

Properties should return current values; they should not be kicking off background operations.

Usually, when someone wants an "asynchronous property", what they really want is one of these:

An asynchronous method that returns a value. In this case, change the property to an async method.

A value that can be used in data-binding but must be calculated/retrieved asynchronously. In this case, either use an async factory method for the containing object or use an async InitAsync() method. The data-bound value will be default(T) until the value is calculated/retrieved.

A value that is expensive to create, but should be cached for future use. In this case, use AsyncLazy from my blog or AsyncEx library. This will give you an awaitable property.