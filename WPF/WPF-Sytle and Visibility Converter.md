
Using Style rather than visibility converter
====

```csharp
<TextBlock>
    <TextBlock.Style>
        <Style TargetType="TextBlock">
            <Setter Property="Text" Value="" />
            <Setter Property="Visibility" Value="Collapsed" />

            <Style.Triggers>
                <DataTrigger Binding="{Binding NoSmartStick}" Value="True">
                    <Setter Property="Text" Value="No Smart Stick Connected"/>
                    <Setter Property="Visibility" Value="Visible" />
                </DataTrigger>

                <DataTrigger Binding="{Binding InitRequired}" Value="True">
                    <Setter Property="Text" Value="System needs initialization"/>
                    <Setter Property="Visibility" Value="Visible" />
                </DataTrigger>

            </Style.Triggers>
        </Style> 
    </TextBlock.Style>
</TextBlock>

```

- Note:  InitRequired has higher precedence than NoSmartStick