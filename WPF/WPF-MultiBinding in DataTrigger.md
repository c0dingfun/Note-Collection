MultiBinding in DataTrigger
====

> http://blog.danskingdom.com/dont-write-wpf-converters-write-c-inline-in-your-xaml-instead-using-quickconverter/

```csharp

<con:AnyoneOfNumbersEqualToZeroToBooleanConverter x:Key="converter"/>
<Style x:Key="btnAddStyle" TargetType="{x:Type Button}">
    <Style.Triggers>
        <MultiDataTrigger>

            <MultiDataTrigger.Conditions>

                <Condition Value="True">
                    <Condition.Binding>
                        <MultiBinding Converter= "{StaticResource converter}">
                            <Binding Path="Text.Length" ElementName="txEditDesc"/>
                            <Binding Path="Text.Length" ElementName="txEditName"/>
                            <Binding Path="Text.Length" ElementName="txEdit3"/>
                        </MultiBinding>
                    </Condition.Binding>
                </Condition>

            </MultiDataTrigger.Conditions>

            <Setter Property="IsEnabled" Value="False"/>
        </MultiDataTrigger>
    </Style.Triggers>
</Style>

```


```csharp

<MultiDataTrigger>
    <MultiDataTrigger.Conditions>

        <Condition Binding="{Binding SensorType}" Value="PhotoIonizer"/> 
        <Condition Binding="{Binding State.Value}" Value="{x:Static common:DigitalSensorState.On}" />
        <Condition Binding="{Binding DeviceState.Value}" Value="{x:Static gen:DeviceStates.Ready}"/> 

        // either HF or OT, it is true; none is false
        <Condition Value="True"> // this other one is False
            <Condition.Binding> 
                <MultiBinding Converter={StaticResource converter}>
                <Binding Path="Overtime">
                <Binding Path="Headfail">
            </Condition.Binding>
        </Condition>

    </MultiDataTrigger.Conditions>

    <MultiDataTrigger.Setters>
        <Setter>
            <Setter.Property>Fill</Setter.Property>
            <Setter.Value>
                <RadialGradientBrush GradientOrigin=".35,.35">
                    <GradientStop Offset="0" Color="{StaticResource DeviceOnBackgroundGlare}" />
                    <GradientStop Offset="1" Color="{StaticResource DeviceOnBackground}" />
                </RadialGradientBrush>
            </Setter.Value>
        </Setter>

        <Setter>
            <Setter.Property>StrokeThickness</Setter.Property>
            <Setter.Value>3</Setter.Value>
        </Setter>

    </MultiDataTrigger.Setters>
</MultiDataTrigger>

```