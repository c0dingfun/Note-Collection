Type Casting in Binding Path
====

[Ref](https://stackoverflow.com/questions/16560550/wpf-binding-casting-in-binding-path)

The solution for the problem, finally, is to use following syntax:

Path=Item.(myNameSpace:IEdge.Tag).caption
The previous code cast Item to the type IEdge in order to access the Tag property.

In case of multiple nested casts the global pattern is :

Path=Obj1.(ns1:TypeObj1.Obj2).(ns2:TypeObj2.Obj3)...(nsN:TypeObjN.BindedProp) 

[Ref](https://web.archive.org/web/20120814100526/http://msdn.microsoft.com/en-us/library/ms742451.aspx)

Single Property, Attached or Otherwise Type-Qualified
`<object property="(ownerType.propertyName)" .../>`

The parentheses indicate that this property in a PropertyPath should be constructed using a partial qualification. It can use an XML namespace to find the type with an appropriate mapping. The ownerType searches types that a XAML processor has access to, through the XmlnsDefinitionAttribute declarations in each assembly. Most applications have the default XML namespace mapped to the http://schemas.microsoft.com/winfx/2006/xaml/presentation namespace, so a prefix is usually only necessary for custom types or types otherwise outside that namespace. propertyName must resolve to be the name of a property existing on the ownerType. This syntax is generally used for one of the following cases:

The path is specified in XAML that is in a style or template that does not have a specified Target Type. A qualified usage is generally not valid for cases other than this, because in non-style, non-template cases, the property exists on an instance, not a type.

The property is an attached property.

You are binding to a static property.