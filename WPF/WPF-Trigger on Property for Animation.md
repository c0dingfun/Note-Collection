Trigger on Property to start Animation
====

> https://actualrandy.wordpress.com/2014/05/12/use-datatriggers-to-initiate-wpf-animation-from-code/

```csharp

<Window.Resources>

  <Style x:Key="timerTriggeredFlash" TargetType="Label">

    <Style.Triggers>

      <DataTrigger Binding="{Binding RelativeSource={RelativeSource AncestorType=Window}, Path=DataContext.StartFlashing}" Value="True">

          <DataTrigger.EnterActions>
             <BeginStoryboard >
               <Storyboard>
                  <ColorAnimation 
                     Storyboard.TargetProperty="(Background).(SolidColorBrush.Color)" 
                     From="White" To="Red" RepeatBehavior="3x" 
                     AutoReverse="True" />
                   </Storyboard>
             </BeginStoryboard>
          </DataTrigger.EnterActions>

      </DataTrigger>
    </Style.Triggers>
  </Style>

</Window.Resources>
```
- HH

```csharp

<Window.Resources>

  <Style x:Key="flashingTextBlock" TargetType="TextBlock">

    <Style.Triggers>

      <Trigger Property="Visibility" Value="Visible">

          <Trigger.EnterActions>

             <BeginStoryboard Name="xxx">
               <Storyboard>
                  <ColorAnimation Storyboard.TargetProperty="(Background).(SolidColorBrush.Color)" From="White" To="Red" RepeatBehavior="3x" AutoReverse="True" />
               </Storyboard>
             </BeginStoryboard>

          </Trigger.EnterActions>

          <Trigger.ExitActions>
            <StopStoryBoard BeginStoryboardName="xxx"/>
          </Trigger.ExitActions>



      </Trigger>
    </Style.Triggers>
  </Style>

</Window.Resources>
```

> https://stackoverflow.com/questions/39508943/binding-to-self-in-style-with-datatrigger

```csharp

 <Style x:Key="MyButtonStyle" TargetType="Button">
 
    <Style.Triggers>

        <Trigger Property="IsEnabled" Value="False">
            <Setter Property="Background" Value="Purple" />
        </Trigger>

        <Trigger Property="IsEnabled" Value="True">
            <Setter Property="Background" Value="Yellow" />
        </Trigger>

    </Style.Triggers>

</Style>

```