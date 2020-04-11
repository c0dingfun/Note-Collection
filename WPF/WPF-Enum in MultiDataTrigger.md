Using Enum value in MultiDataTrigger's Condition Binding
====

> https://stackoverflow.com/questions/14279602/how-can-i-use-enum-types-in-xaml

> https://stackoverflow.com/questions/13917033/datatrigger-on-enum-to-change-image

You need 2 things to get this working:

1 - Add an xmlns reference in the root element of your XAML file, to the namespace where your Enum is defined:

```csharp

<UserControl ...
xmlns:my="clr-namespace:YourEnumNamespace;assembly=YourAssembly"> 
```

2 - in the Value property of the DataTrigger, use the {x:Static} form:

```csharp

    <DataTrigger Binding="{Binding Path=LapCounterPingStatus}" Value="{x:Static my:PingStatus.PING_UNKNOWN}">
```

Notice that the Enum type must be prefixed with the xmlns prefix you defined above.

Edit:

If your Enum is declared inside a class you need to use the syntax:

`{x:Static namespace:ClassName+EnumName.EnumValue}`

for example:

`{x:Static my:ConfigurationViewModel+PingStatus.PING_UNKNOWN}`