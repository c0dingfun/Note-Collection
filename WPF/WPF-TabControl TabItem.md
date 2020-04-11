TabControl and TabItem
====

> https://wpf.2000things.com/2013/09/16/907-binding-a-tabcontrol-to-a-list-of-objects-part-i/  https://wpf.2000things.com/2013/09/17/908-binding-a-tabcontrol-to-a-list-of-objects-part-ii/

> https://wpf.2000things.com/2013/09/18/909-binding-a-tabcontrol-to-a-list-of-objects-part-iii/

> https://wpf.2000things.com/tag/tabcontrol/
  https://stackoverflow.com/questions/7498110/how-to-wpf-tabitem-style-headertemplate-binding/8227618

> https://www.essentialobjects.com/doc/wpf/controls/tabcontrol/custom_item.aspx

> https://books.google.com/books?id=LyR1myauBPEC&pg=PT218&lpg=PT218&dq=TabControl+headertemplate&source=bl&ots=wKV5ESE4s2&sig=ACfU3U3JuytCGD7GPkFcsk1S-mYOJnAQmw&hl=en&sa=X&ved=2ahUKEwisrIjW6JLiAhWCFjQIHYwXA8E4FBDoATAAegQICRAB#v=onepage&q=TabControl%20headertemplate&f=false


> https://stackoverflow.com/questions/32199403/wpf-tabitem-headertemplate

```csharp

<TabControl ItemsSource="{Binding Tabs}">

    <TabControl.ItemContainerStyle>
        <Style TargetType="TabItem">
            <Setter Property="HeaderTemplate">
                <Setter.Value>
                    <DataTemplate DataType="local:TabViewModel">

                        <StackPanel Orientation="Horizontal" Margin="5">
                            <Image x:Name="Icon" Source="{Binding Icon, Converter={StaticResource UriToBitmapSourceConverter}}" />
                            <Rectangle x:Name="ColorRect" Height="16" Width="16" Fill="{Binding Color}" Visibility="Collapsed" />
                            <TextBlock Text="{Binding Title}" Foreground="{Binding Color}"/>
                        </StackPanel>

                        <DataTemplate.Triggers>
                            <DataTrigger Binding="{Binding Icon}" Value="{x:Null}">
                                <Setter TargetName="Icon" Property="Visibility" Value="Collapsed" />
                                <Setter TargetName="ColorRect" Property="Visibility" Value="Visible" />
                            </DataTrigger>
                        </DataTemplate.Triggers>
                    </DataTemplate>
                </Setter.Value>
            </Setter>
        </Style>
    </TabControl.ItemContainerStyle>
</TabControl>

```