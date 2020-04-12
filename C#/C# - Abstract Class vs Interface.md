Abstract Class and Interface
====

Abstract Class
----

Abstract Class is not instantiable. It is meant to provide default implementations for all concrete subclasses and specify methods, properties that concrete subclasses need to implement (this is more like an interface).

Abstract class can not be static or private.

Abstract class can have both non-abstract and abstract properties, methods, etc.

Due to C# does not support multiple inheritance, a subclass can not inherit multiple different abstract classes. However an abstract class can inherit another abstract class.

Interface
----

Interface defines a contract that classes implement  it has to implement.

Interface has no implementations, unlike abstract class.

Interface can not define static and virtual methods. (Interface methods are not overridden)

A class can inherit multiple interfaces

Interfaces can be used in method's parameter type, to specify what kind of object (that implements the interface) can be passed into.

Summary
----

Abstract - is vertical reusability within the class hierarchy
Abstract - can have methods, properties, indexers, and events

Interface - is horizontal contract among different classes
Interface - can have methods, properties, delegate, and events, but no implementation

||Abstract Class|Interface|
|---|---|---|
|Acced Specifier|allowed|no access specifier, by default, public|
|Implementation|can have implementation|no implementation|
|Fields|Can define fields and constants|can not have fields|
|Methods|Can have abstract nad non-abstract methods|onl abstract methods|


> https://www.c-sharpcorner.com/UploadFile/d0e913/abstract-class-interface-two-villains-of-every-interview/

> https://www.c-sharpcorner.com/UploadFile/d0e913/abstract-class-interface-two-villains-of-every-interview756/