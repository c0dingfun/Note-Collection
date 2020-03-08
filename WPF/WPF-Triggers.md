# WPF Triggers - in examples

* Data Trigger is used to select different DataTemplates

    <Style x:Key="selectableContentStyle" TargetType="{x:Type ContentControl}">
      <Style.Triggers>
        <DataTrigger Binding="{Binding Path=CurrentStatus}" Value="Required">
          <Setter Property="ContentTemplate" Value="{StaticResource requiredTemplate}" />
        </DataTrigger>
        <DataTrigger Binding="{Binding Path=CurrentStatus}" Value="Completed">
          <Setter Property="ContentTemplate" Value="{StaticResource completedTemplate}" />
        </DataTrigger>
        <!--  your other Status' here -->
      </Style.Triggers>
    </Style>

* Property Trigger

        <TextBlock Text="Hello, styled world!" FontSize="28" HorizontalAlignment="Center" VerticalAlignment="Center">
            <TextBlock.Style>
                <Style TargetType="TextBlock">
                    <Setter Property="Foreground" Value="Blue"></Setter>
                    <Style.Triggers>
                        <Trigger Property="IsMouseOver" Value="True">
                            <Setter Property="Foreground" Value="Red" />
                            <Setter Property="TextDecorations" Value="Underline" />
                        </Trigger>
                    </Style.Triggers>
                </Style>
            </TextBlock.Style>
        </TextBlock>

* Data Trigger

        <TextBlock HorizontalAlignment="Center" Margin="0,20,0,0" FontSize="48">
            <TextBlock.Style>
                <Style TargetType="TextBlock">
                    <Setter Property="Text" Value="No" />
                    <Setter Property="Foreground" Value="Red" />
                    <Style.Triggers>
                        <DataTrigger Binding="{Binding ElementName=cbSample, Path=IsChecked}" Value="True">
                            <Setter Property="Text" Value="Yes!" />
                            <Setter Property="Foreground" Value="Green" />
                        </DataTrigger>
                    </Style.Triggers>
                </Style>
            </TextBlock.Style>
        </TextBlock>

* Event Trigger

        <TextBlock Name="lblStyled" Text="Hello, styled world!" FontSize="18" HorizontalAlignment="Center" VerticalAlignment="Center">
            <TextBlock.Style>
                <Style TargetType="TextBlock">
                    <Style.Triggers>
                        <EventTrigger RoutedEvent="MouseEnter">
                            <EventTrigger.Actions>
                                <BeginStoryboard>
                                    <Storyboard>
                                        <DoubleAnimation Duration="0:0:0.300" Storyboard.TargetProperty="FontSize" To="28" />
                                    </Storyboard>
                                </BeginStoryboard>
                            </EventTrigger.Actions>
                        </EventTrigger>
                        <EventTrigger RoutedEvent="MouseLeave">
                            <EventTrigger.Actions>
                                <BeginStoryboard>
                                    <Storyboard>
                                        <DoubleAnimation Duration="0:0:0.800" Storyboard.TargetProperty="FontSize" To="18" />
                                    </Storyboard>
                                </BeginStoryboard>
                            </EventTrigger.Actions>
                        </EventTrigger>
                    </Style.Triggers>
                </Style>
            </TextBlock.Style>
        </TextBlock>
