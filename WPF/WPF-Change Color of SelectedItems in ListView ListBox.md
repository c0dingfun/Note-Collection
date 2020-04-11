Change Color of Selected Items in ListView, ListBox
====

[Ref](https://www.wpftutorial.net/ListBoxSelectionBackground.html)

If you select an item in a listbox it gets the default selection color (usually blue) as background. Even if you specify a custom data template. The reason is that the blue background (or gray if the control is not focussed) is drawn outside of the data template. So you have no chance to override it from within the data template.

The color used to draw the blue and gray background are system colors. So the easiest way to get rid of these backgrounds is to locally override the highlight and control brushes of the system colors.

The best way to do this is to create a style for the listbox. Place the style in the resources of a parent element. For e.g. Window.Resources

```csharp

<Style x:Key="myListboxStyle">
    <Style.Resources>
        <!-- Background of selected item when focussed -->
        <SolidColorBrush x:Key="{x:Static SystemColors.HighlightBrushKey}" Color="Red" />
        <!-- Background of selected item when not focussed -->
        <SolidColorBrush x:Key="{x:Static SystemColors.ControlBrushKey}" Color="Green" />
    </Style.Resources>
</Style>
```

Usage:
----

```csharp
<Grid x:Name="LayoutRoot">
  <ListBox Style="{StaticResource myListboxStyle}" />
</Grid>

```