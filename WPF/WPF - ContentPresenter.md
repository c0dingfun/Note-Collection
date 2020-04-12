ContentPresenter
====

Used in ControlTemplate to specify where the content is shown.
If the Templated Parent's content property is not Content, the specify it by ContentSource

`<ContentPresenter ContentSource="MyProperty"/>`

If the Templated Parent's content property is Content, `<ContentPresenter/>` is good enough.

x=WPF TemplateBinding

Use in ControlTemplate to Bind Templated Parent's Property

```csharp
<ControlTemplate>
    <Button Padding={TemplateBinding Padding}/>
</ControlTemplate>
```
