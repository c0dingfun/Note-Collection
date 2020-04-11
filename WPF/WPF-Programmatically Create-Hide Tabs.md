Programmatically Create Hide specific Tabs (MVVM-way)
====

InkStickView.xaml (example)

= TabControl is a HeaderedItemsControl, with tabs
= Use TabControl.ItemsSource to be bind to a Tabs Collection in ViewModel
= Use TabControl.ContentTemplateSelector to select selected tab's content
(DataTemplate)
= Use TabControl.ItemContainerStyle to
    1) to specify how the tab header is displayed
    2) to DataTrigger to hide/show the TabItem (a Tab)
= To hide the tab content, need to hide (collapse) the tab's
ContentPresenter.

Note: In order to hide a tab, using TabControl.ItemTemplate is not able to
achieve it, because ItemTemplate itself is contained by the Container,
and using ItemTemplate is not able to reach the Container via DataTrigger.
So, in most cases, TabControl.ItemTemplate can be used to specify how the
tab header is displayed, but when there is a need to hide the tab, we have
to use ItemContainerStyle in order to set the TabItem's Visibility
property.

```csharp

<TabControl ItemsSource="{Binding Tabs}"  // Binding to a collection in MVVM

            // specify the DataTemplate selector
            ContentTemplateSelector="{StaticResource inkStickTabControlContentTemplateSelector}" >

    <TabControl.ItemContainerStyle> // ItemContainerStyle is needed for hidding a tab
        <Style TargetType="TabItem">
            <Setter Property="HeaderTemplate" Value="{StaticResource TabHeaderTemplate}"/>  // specify how the header looks

            <Setter Property="Visibility" Value="Visible"/>
            <Setter Property="IsEnabled" Value="True"/>

            <Style.Triggers>    // Style trigger is used to show/hide a tab
                <DataTrigger Binding="{Binding Claim, Converter={StaticResource converterUserAccessEnable}}" Value="False">
                    <Setter Property="Visibility" Value="Collapsed"/>   // hide the tab
                    // <Setter Property="" Value="Collapsed"/>          // disable the tab
                </DataTrigger>
            </Style.Triggers>

        </Style>
    </TabControl.ItemContainerStyle>
</TabControl>
```

```csharp
<DataTemplate x:Key="TabHeaderTemplate">    // specify how the tab header is displayed
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Column="0" Text="{Binding Header}"/>
    </Grid>
</DataTemplate>
```

```csharp

<DataTemplate x:Key="InkEditorTabTemplate">
    <local:InkEditorView Margin="20" DataContext="{Binding ViewModel}"
        Visibility="{Binding DataContext.Claim, 
            // in order to hide the content, we need to hide the ContentPresenter!!!!!
            RelativeSource={RelativeSource AncestorType={x:Type ContentPresenter}},  
            Converter={StaticResource converterUserAccessVisible} }" />
```

