DataContext (Interesting Read)
====

[Interesting Read:](https://siderite.blogspot.com/2011/06/contentcontrol-and-contentpresenter-do.html)

A quick post here about using a ContentPresenter (or a ContentControl which uses a 
ContentPresenter in its template) with its Content property. 

The intended usage of ContentPresenter is to set the Content to some binding to a data object, then control the element tree via the ContentTemplate property. That may lead to a counterintuitive situation when you want to specify some UI element content and then use bindings in that content. Let's take an example:

```csharp


<!-- 
This ContentControl has a MainViewModel class as a DataContext.
The MainViewModel class exposes a MyButtonCommand property.
-->
<ContentControl>
  <ContentControl.Content>
    <Button Command="{Binding MyButtonCommand}">Press me!</Button>
  </ContentControl.Content>
</ContentControl>
```

You may expect to press the button and execute the command, but it doesn't work. 
In fact, the binding on the Command property will fail.

Here is a working example:

```csharp

<!-- 
This ContentControl has a MainViewModel class as a DataContext.
The MainViewModel class exposes a MyButtonCommand property.
-->
<ContentControl Content="{Binding MyButtonCommand}">
  <ContentControl.ContentTemplate>
    <DataTemplate>
      <Button Command="{Binding}">Press me!</Button>
    </DataTemplate>
  </ContentControl.ContentTemplate>
</ContentControl>
```

I realize this is not what most of you have in mind when using a ContentControl. 
Another solution is to use the Content as in the first example, but add an explicit 
DataContext property to it before using any binding, something like this:

```C#

<!-- 
This ContentControl has a MainViewModel class as a DataContext.
The MainViewModel class exposes a MyButtonCommand property.
-->
<ContentControl>
  <ContentControl.Content>
    <DataTemplate>
      <Button 
         DataContext="{Binding DataContext,
                        RelativeSource={RelativeSource AncestorType={x:Type ContentControl}}}"
         Command="{Binding MyButtonCommand}">Press me!</Button>
    </DataTemplate>
  </ContentControl.Content>
</ContentControl>
```

In this case, though, you specify the DataContext as an ugly binding and, worst of all, 
you cannot set it via the ContentControl, but you need to access the actual content.

Perhaps another solution, one that would involve a custom DataTemplateSelector on the 
ContentControl would work, but right now I have no perfectly satisfactory solution.


Better: (Commented by Annoymous)

```csharp

    <ContentControl Content="{Binding}">    // binding to MainViewModel
        <ContentControl.ContentTemplate>
            <DataTemplate>  // DataContext of the DataTemplate is the Control.Conent!!!
                // Since the DataTemplate Context is the Content = MainViewModel
                // Inside the DataTemplate, we can bind to MainViewModel's
                // MyButtonCommand
                <Button Command="{Binding MyButtonCommand}">Press me!</Button>
            </DataTemplate>
        </ContentControl.ContentTemplate>
    </ContentControl>
```

    or

```csharp

    <ContentControl 
        Content="{Binding}"
        ContentTemplate={StaticResource MyDataTemplate}" />
```
