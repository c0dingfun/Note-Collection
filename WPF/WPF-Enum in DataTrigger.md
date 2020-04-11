Using Enum in DataTrigger
====

> Two Steps:


1 - Add an xmlns reference in the root element of your XAML file, to the namespace where your Enum is defined:

```csharp

    <UserControl ...
        xmlns:myNS="clr-namespace:YourEnumNamespace;assembly=YourAssembly"> 
```

2 - in the Value property of the DataTrigger, use the {x:Static} form:

```csharp
    <DataTrigger Binding="{Binding Path=LapCounterPingStatus}" 
                            Value="{x:Static myNS:MyEnum.TheEnumValueToUse}">
```

Notice that the Enum type must be prefixed with the xmlns prefix you defined above.

Note: If your Enum is declared inside a class you need to use the syntax:

```csharp

    {x:Static namespace:ClassName+EnumName.EnumValue}

    eg: {x:Static myNS:TheClassName+MyEnum.TheEnumValueToUse}
```

[credit:](https://stackoverflow.com/questions/13917033/datatrigger-on-enum-to-change-image)
