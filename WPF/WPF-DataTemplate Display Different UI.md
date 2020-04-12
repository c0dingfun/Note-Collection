WPF Use DataTemplate to display different UI on ContentControl based on Data
====

```xaml
<ContentControl Content="{Binding SelectedItem.SmartStick, ElementName=ListView}">
    <ContentControl.Resources>

        <DataTemplate DataType="{x:Type generated:StorageInkstickSubsystem}" >
            <StackPanel>
                <StackPanel Orientation="Horizontal" Margin="0,10,0,0">
                    <TextBlock>Previous Ink Stick Serial Number</TextBlock>
                    <TextBlock Margin="10,0,0,0" Text="{Binding LastInkstickSerialNumber.Value}" />
                </StackPanel>
                <Button Margin="0,3,3,0" HorizontalAlignment="Right" 
                        Command="{Binding DataContext.UpdateSerialNumberCmd, ElementName=ListView}" 
                        Content="Update Previous Ink Stick Serial Number"
                        Visibility="{Binding IsEnabled, RelativeSource={RelativeSource Self}, Converter={StaticResource BoolToVisibilityConverter}}" />
            </StackPanel>
        </DataTemplate>

        <DataTemplate DataType="{x:Type generated:PrintInkstickSubsystem}">
            <StackPanel></StackPanel>
        </DataTemplate>

    </ContentControl.Resources>
</ContentControl>

```