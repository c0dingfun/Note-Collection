How to tie a Control's Visibility to Binding Target's NULL?
====

When null, make it invisible.

> https://stackoverflow.com/questions/26737687/binding-default-value-to-visibility-if-default-value-is-null-in-viewmodel

`TargetNullValue={x:Static Visibility.Collapsed}`

For example:

```csharp
<local:LightTowerView  Grid.RowSpan="2" VerticalAlignment="Stretch"  Width="8" 
    DataContext="{Binding LightTowerViewModel}" 
    Visibility="{Binding TargetNullValue=Collapsed}" />
```


Better Ref: https://www.c-sharpcorner.com/UploadFile/41e70f/fallbackvalue-in-wpf/