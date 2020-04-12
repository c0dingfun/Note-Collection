Reflection: Setting Property by its string Name
====

Example:
----

```csharp

Ship ship = new Ship();
string value = "5.5";
PropertyInfo propertyInfo = ship.GetType().GetProperty("Latitude"); // Latitude is a double

// Latitude is a double, but the value is a string; thus, Convert is need
propertyInfo.SetValue(ship, Convert.ChangeType(value, propertyInfo.PropertyType), null);

----

using System.Reflection;

...

string prop = "name";
PropertyInfo pi = myObject.GetType().GetProperty(prop);
pi.SetValue(myObject, "Bob", null);

```