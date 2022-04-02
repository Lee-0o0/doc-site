# WPF— —绑定

本文主要介绍WPF中的绑定。



## 1. Binding的含义

绑定，即Binding，是一种在视图层和逻辑层同步数据的机制。

在WinForm开发中，视图层和逻辑层是分离的，如下：

![image-20220113093402463](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-02/5785a4ef63f70323cdc76c6c208a957c--8528--image-20220113093402463.png)

如果我们在视图层改变“名字”，逻辑层里的Name是不会改变的，需要我们编写代码触发事件来改变逻辑层里的对象属性。同理，逻辑层里的对象属性如果改变了，视图层显示的内容也不会发生改变，同样需要我们编写代码手动改变视图层的内容。

在WPF中引入了Binding机制，可以帮助我们在视图层和逻辑层自动同步数据。

![image-20220113093807750](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-02/99fb734796a6717dc03254497993d1fc--56f0--image-20220113093807750.png)

这样，如果视图层某个显示内容和逻辑层的某个对象属性绑定在一起后，如果我们改变视图层的内容，那么逻辑层的对象属性也会自动改变。同理，改变逻辑层的对象属性，也会自动改变视图层的显示内容。

我们一般将逻辑层的数据对象称为Binding源，将视图层的控件对象称为Binding目标。



## 2. Binding的入门案例

下面就来介绍一下Binding的入门案例。新建项目后，创建一个学生类：

```c#
/// <summary>
/// 学生类
/// </summary>
public class Student : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;

    private string name;

    public string Name
    {
        get
        {
            return name;
        }
        set
        {
            name = value;
            if(PropertyChanged != null)
            {
                PropertyChanged.Invoke(this,new PropertyChangedEventArgs("Name"));
            }
        }
    }
}
```

注意，如果要让该类某个属性具备通知Binding该属性发生了变化，需要实现`INotifyPropertyChanged`接口，并在属性set语句中激发`PropertyChanged`事件。

然后在视图层防止一个文本框和按钮：

```xaml
<Window x:Class="WPF_04_Binding.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:WPF_04_Binding"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="450">
    <StackPanel>
        <TextBox x:Name="textBox" Margin="30 10 30 10" Height="30"></TextBox>
        <Button Margin="30 0 30 10" Height="30" Click="Button_Click">Click Me</Button>
        <Button Margin="30 0 30 10" Height="30" Click="Button_Click_1">显示Name的值</Button>
    </StackPanel>
</Window>
```

在逻辑层中完成绑定：

```c#
/// <summary>
/// Interaction logic for MainWindow.xaml
/// </summary>
public partial class MainWindow : Window
{
    private Student student;

    public MainWindow()
    {
        InitializeComponent();
        // 创建数据源
        student = new Student() { Name = "张三" };
        // 创建绑定
        Binding binding = new Binding();
        binding.Source = student;
        binding.Path = new PropertyPath("Name");
        // 将数据源和数据目标绑定在一起
        BindingOperations.SetBinding(this.textBox, TextBox.TextProperty, binding);
    }

    /// <summary>
    /// 点击按钮
    /// </summary>
    private void Button_Click(object sender, RoutedEventArgs e)
    {
        student.Name += "张三";
    }
    
    /// <summary>
    /// 显示Name的值
    /// </summary>
    private void Button_Click_1(object sender, RoutedEventArgs e)
    {
        MessageBox.Show("student.Name = " + student.Name);
    }
}
```

这样，当我们点击按钮时，会改变数据源`student`中的`Name`属性，由于绑定机制的存在，视图层中文本框的内容也会随之发生改变。如果我们在文本框中输入文本，并按下“显示Name的值”按钮，会发现逻辑层里的对象属性也自动发生了改变。

![image-20220113095624803](https://cdn.jsdelivr.net/gh/Lee-0o0/image-store/PicGo/2022-04-02/5d374d49166217984061eaffca55a5b7--6b02--image-20220113095624803.png)



## 3. Binding的路径

所谓绑定的路径，就是选择Binding源中的哪个属性需要绑定到Binding目标中。





## Binding的源



## Binding的模式



## Binding的数据校验与转换



## 多路Binding(MultiBinding)



## 总结



## 参考资料

[1] 刘铁猛 著.《WPF深入浅出》
