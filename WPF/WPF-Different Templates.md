Control.Template vs ContentControl.ControlTemplate vs ItemsControl.ItemTemplate
====

|Template|Type of Template|
|---|---|
|Control.Template|ControlTemplate|
|ContentControl.ContentTemplate|DataTemplate|
|ItemsControl.ItemTemplate|DataTemplate|

[Diff](https://stackoverflow.com/questions/1340108/difference-between-control-template-and-datatemplate-in-wpf)

- A Control is rendered for itself; it doesn't reflect underlying data.

For example, a Button would NOT be bound to business object; it's just purely something can be click on.

- A ContentControl/ItemsControl (ListBox), however is to present data for the user.

Therefore, a DataTemplate is used to provide visual structure for underlying data, while ControlTemplate has nothing to do with underlying data and simply provides visual layout for the Control itself.

- A ControlTemplate will generally only contain TemplateBinding expressions, binding back to the properties of the Control itself. 

While a DataTemplate will contain standard Binding expressions, binding to the properties of its DataContext (the business/domain object or ViewModel)
