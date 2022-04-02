# WPF— —布局

布局就是组织控件的位置和大小。布局的关键要求就是适应窗口大小和显示设置的变化。WPF提供了以下布局：



## 1. Canvas

Canvas确定一个区域，在该区域内，子控件根据相对于Canvas的坐标确定位置。

```xaml
<Window x:Class="wpf_02_layout.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:wpf_02_layout"
        mc:Ignorable="d"
        Title="MainWindow" Height="350" Width="350">
    <Canvas Width="300" Height="300" Background="AntiqueWhite">
        <Rectangle Width="100" Height="100" Canvas.Left="0" Canvas.Top="0" Fill="Black"/>
        <Rectangle Width="100" Height="100" Canvas.Left="100" Canvas.Top="100" Fill="Red"/>
        <Rectangle Width="100" Height="100" Canvas.Left="50" Canvas.Top="50" Fill="Blue"/>
    </Canvas>
</Window>
```

子控件使用`Canvas.Left`和`Canvas.Top`确定子控件相对于Canvas的位置，当然也可以设置`Canvas.Bottom`和`Canvas.Right`。

结果：

![image-20210929104337216](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-02/82e4129d6da37346e5f94e81ec7be0ff--5f25--image-20210929104337216.png)



## 2. DockPanel

DockPanel将区域分成四个部分：Top、Bottom、Left和Right。子控件可以通过`DockPanel.Dock`选择将该控件放置在哪一个子区域内。

```xaml
<Window x:Class="wpf_02_layout.DockPanelWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:wpf_02_layout"
        mc:Ignorable="d"
        Title="DockPanelWindow" Height="350" Width="350">
    <DockPanel>
        <TextBox DockPanel.Dock="Top">DockPanel.Dock = Top</TextBox>
        <TextBox DockPanel.Dock="Bottom">DockPanel.Dock = Bottom</TextBox>
        <TextBox DockPanel.Dock="Left">DockPanel.Dock = Left</TextBox>
        <TextBox DockPanel.Dock="Right">DockPanel.Dock = Right</TextBox>
    </DockPanel>
</Window>
```

结果：

![image-20210929105324455](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-02/f2a360fdf3abfa605dab132ec20a92b3--b250--image-20210929105324455.png)



## 3. Grid

Grid表示表格布局，将区域按行列分为格子，控件放置在格子中。

-   首先在Grid中定义该表格有几行几列；
-   然后紧接着在Grid中定义子控件，并设置`Grid.Row`和`Grid.Column`确定该子控件放置在哪个位置；

```xaml
<Window x:Class="wpf_02_layout.GridWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:wpf_02_layout"
        mc:Ignorable="d"
        Title="GridWindow" Height="350" Width="350">
    <Grid Background="AliceBlue">
        <!-- 定义表格有几列 -->
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <!-- 定义表格有几行 -->
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition />
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>

        <!-- 表格第一行内容 -->
        <Label Grid.Row="0" Grid.Column="0" Content="姓名" HorizontalAlignment="Center"></Label>
        <Label Grid.Row="0" Grid.Column="2" Content="年龄" HorizontalAlignment="Center"></Label>
        <!-- 表格第二行内容 -->
        <Label Grid.Row="1" Grid.Column="0" Content="张三" HorizontalAlignment="Center"></Label>
        <Label Grid.Row="1" Grid.Column="2" Content="20" HorizontalAlignment="Center"></Label>
        <!-- 表格第三行内容 -->
        <Button Grid.Row="2" Grid.Column="0" Content="确定" HorizontalAlignment="Center"></Button>
        <Button Grid.Row="2" Grid.Column="2" Content="取消" HorizontalAlignment="Center"></Button>
    </Grid>
</Window>

```

结果：

![image-20210929111805449](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-02/1c9601edc0d797d3902da733936d017e--7af3--image-20210929111805449.png)



## 4. StackPanel

StackPanel布局是将子控件依次序排列在一行或一列，我们可以设置Orientation的值来确定子控件的方向。

```xaml
<Window x:Class="wpf_02_layout.StackPanelWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:wpf_02_layout"
        mc:Ignorable="d"
        Title="StackPanelWindow" Height="350" Width="350">
    <StackPanel Orientation="Horizontal">
        <Label Content="标签1" Background="AliceBlue"/>
        <Label Content="标签2" Background="AntiqueWhite"/>
        <Label Content="标签3" Background="Aqua"/>
        <Label Content="标签4" Background="Aquamarine"/>
        <Label Content="标签5" Background="Azure"/>
        <Label Content="标签6" Background="Beige"/>
        <Label Content="标签7" Background="Bisque"/>
    </StackPanel>
</Window>
```

上例中子控件水平扩展，结果：

![image-20210929113126720](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-02/d2f5dd8b20805bfdc6fe04c2cde8e812--4426--image-20210929113126720.png)

如果我们将Orientation设置位Vertical，则子控件垂直扩展，则结果为：

![image-20210929113240157](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-02/31abbe2bcd9964b1edb3575caef59621--932b--image-20210929113240157.png)



## 5. WrapPanel

WrapPanel与StackPanel相似，都是将子控件放置在一行或一列，但是与StackPanel不同的是，当子控件数量过多，导致在一行中放不下所有的子控件时，WrapPanel会将多余的子控件放置在下一行或下一列；而StackPanel并不会。

```xaml
<Window x:Class="wpf_02_layout.WrapPanelWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:wpf_02_layout"
        mc:Ignorable="d"
        Title="WrapPanelWindow" Height="200" Width="200">
    <WrapPanel Orientation="Horizontal">
        <Label Content="标签1" Background="AliceBlue"></Label>
        <Label Content="标签2" Background="AntiqueWhite"></Label>
        <Label Content="标签3" Background="AliceBlue"></Label>
        <Label Content="标签4" Background="AntiqueWhite"></Label>
        <Label Content="标签5" Background="AliceBlue"></Label>
        <Label Content="标签6" Background="AntiqueWhite"></Label>
        <Label Content="标签7" Background="AliceBlue"></Label>
        <Label Content="标签8" Background="AntiqueWhite"></Label>
    </WrapPanel>
</Window>
```

![image-20210929144939273](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-02/0de5838889a23f5ce20ff985b5b5590e--5dca--image-20210929144939273.png)

如果将Orientation设置为Vertical，结果为：

![image-20210929145015257](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-02/9f0ca3a5a1907e16cf3bfdd5d04983e7--6d8a--image-20210929145015257.png)



## 6. UniformGrid

UniformGrid 使得组件能自动、均匀占据父组件的控件。

```xml
<Page x:Class="WPF_02_Layout.UniGridPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
      mc:Ignorable="d" 
      d:DesignHeight="450" d:DesignWidth="450"
      Title="UniformGridPage">

    <UniformGrid>
        <Button Content="Button1"></Button>
        <Button Content="Button1"></Button>
        <Button Content="Button1"></Button>
        
        <Button Content="Button1"></Button>
        <Button Content="Button1"></Button>
        <Button Content="Button1"></Button>
        
        <Button Content="Button1"></Button>
        <Button Content="Button1"></Button>
        <Button Content="Button1"></Button>
    </UniformGrid>
</Page>
```

结果如下：

![image-20220104085458437](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-02/74b60a16fb3336ce84e6eb8fa1ed50d4--2ab1--image-20220104085458437.png)