C# - Enum Attribute
====

Enum Attribute
----

• Retrieve Enum value’s Attribute value
• http://stackoverflow.com/questions/1799370/getting-attributes-of-enums-value

1)

This piece of code should give you a nice little extension method on any enum that lets you retrieve a generic attribute. I believe it's different to the lambda function above because it's simpler to use and slightly - you only need to pass in the generic type.

```csharp
public static class EnumHelper
{
    /// <summary>
    /// Gets an attribute on an enum field value
    /// </summary>
    /// <typeparam name="T">The type of the attribute you want to retrieve</typeparam>
    /// <param name="enumVal">The enum value</param>
    /// <returns>The attribute of type T that exists on the enum value</returns>
    /// <example>string desc = myEnumVariable.GetAttributeOfType<DescriptionAttribute>().Description;</example>
    public static T GetAttributeOfType<T>(this Enum enumVal) where T:System.Attribute
    {
        var type = enumVal.GetType();
        var memInfo = type.GetMember(enumVal.ToString());
        var attributes = memInfo[0].GetCustomAttributes(typeof(T), false);
        return (attributes.Length > 0) ? (T)attributes[0] : null;
    }
}

```

2)

In addition to AdamCrawford response, I've further created a more specialized extension methods that feed of it to get the description.

```csharp
public static string GetAttributeDescription(this Enum enumValue)
{
    var attribute = enumValue.GetAttributeOfType<DescriptionAttribute>();
    return attribute == null ? String.Empty : attribute.Description;
} 
```

Hence, to get the description, you could either use the original extension method as

string desc = myEnumVariable.GetAttributeOfType<DescriptionAttribute>().Description
or you could simply call the the extension method here as:

string desc = myEnumVariable.GetAttributeDescription();
Which should hopefully make your code a bit more readable..