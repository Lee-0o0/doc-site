# WPF— —样式

样式用来改变组件的外观。就像CSS一样。

如下，我们在资源中定义了一个样式，并制定目标类型为Button，则之后的Button都会应用样式中定义的属性：

```xaml
<Window x:Class="WPF_03_Style.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="450">
    <Window.Resources>
        <Style TargetType="Button">
            <Setter Property="Height" Value="50"></Setter>
            <Setter Property="Background" Value="Pink"></Setter>
            <Setter Property="Margin" Value="10 10 10 0"></Setter>
        </Style>
    </Window.Resources>
    
    <StackPanel>
        <Button>BUTTON1</Button>
        <Button>BUTTON2</Button>
        <Button>BUTTON3</Button>
    </StackPanel>
</Window>
```

效果：

![image-20220106191914436](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-02/e8d8d88e41edf4f6505237d1b31d9846--281d--image-20220106191914436.png)

我们可以在Button元素中改变样式中定义的属性，优先使用Button元素中定义的属性值。

上述的样式应用于所有Button，在实际应用中，我们只想特定样式应用于某一个或某几个Button，那么我们可以定义样式的Key，然后在指定的Button中引用样式：

```xaml
<Window x:Class="WPF_03_Style.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="450">
    <Window.Resources>
        <Style TargetType="Button" x:Key="PinkButton">
            <Setter Property="Height" Value="50"></Setter>
            <Setter Property="Background" Value="Pink"></Setter>
            <Setter Property="Margin" Value="10 10 10 0"></Setter>
        </Style>
    </Window.Resources>
    
    <StackPanel>
        <Button Style="{StaticResource PinkButton}">BUTTON1</Button>
        <Button>BUTTON2</Button>
        <Button>BUTTON3</Button>
    </StackPanel>
</Window>

```

效果如下：

![image-20220106192301618](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-02/559a2c07e32887a140f949e589c94ab5--de62--image-20220106192301618.png)

可以看到Button1应用了样式，但Button2和Button3保持原生样式。