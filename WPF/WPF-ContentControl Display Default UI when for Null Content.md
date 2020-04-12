
ContentControl to display Default UI when Content is NULL
====

>   https://stackoverflow.com/questions/1217254/display-a-default-datatemplate-in-a-contentcontrol-when-its-content-is-null-or-e

== use Data Trigger
----

```xaml

<ContentControl>
    <ContentControl.Style>
        <Style TargetType="ContentControl">
            <Setter Property="Content" Value="{Binding HurfView.EditedPart}" />
            <Style.Triggers>
                <DataTrigger Binding="{Binding RelativeSource={x:Static RelativeSource.Self}, Path=Content}" Value="{x:Null}">
                    <Setter Property="ContentControl.Template">
                        <Setter.Value>
                            <ControlTemplate>
                                <Grid HorizontalAlignment="Stretch" VerticalAlignment="Stretch">
                                    <TextBlock>EMPTY!</TextBlock>
                                </Grid>
                            </ControlTemplate>
                        </Setter.Value>
                    </Setter>
                </DataTrigger>
            </Style.Triggers>
        </Style>
    </ContentControl.Style>
</ContentControl>
```

== use TargetNullValue
----

```xaml
<ContentControl>
    <ContentControl.Content>
    <Binding Path="ContentViewModel">
        <Binding.TargetNullValue>
        <Grid HorizontalAlignment="Stretch" VerticalAlignment="Stretch">
            <TextBlock>EMPTY!</TextBlock>
        </Grid>
        </Binding.TargetNullValue>
    </Binding>
    </ContentControl.Content>
</ContentControl>
```

```xaml

<DataTemplate DataType="{x:Type system:DBNull}">
    <!-- The default template -->
</DataTemplate>
```

```xaml
<ContentControl Content="{Binding HurfView.EditedPart, FallbackValue={x:Static system:DBNull.Value}}" />
```