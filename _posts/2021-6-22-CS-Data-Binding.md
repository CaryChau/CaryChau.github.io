## 数据绑定
[**本文章**](https://docs.microsoft.com/zh-cn/archive/msdn-magazine/2016/july/data-binding-a-better-way-to-implement-data-binding-in-net)讲了重构的内容

但是派生的属性仍需要侦听每个基础属性上的 ValueChanged 事件。解决此问题需要两个步骤。首先，我将 ValueChanged 事件提取到一个单独的接口中：

```
public interface INotifyValueChanged // No generic type!
{
  event EventHandler<ValueChangedEventArgs> ValueChanged;
}
public interface IProperty<TValue> : INotifyValueChanged
{
  string Name { get; }
  TValue Value { get; }
}
```

整理汇总：现在我有了**视图模型**需要实施的接口和用于进行数据绑定的属性的 NotifyProperty 类。最后一步是构造 NotifyProperty；为此，你仍需要以某种方式传入一个属性名。如果你恰好在使用 C# 6，就可以轻松地通过 nameof 运算符实现这一点。如果不是，你可以借助表达式创建 NotifyProperty，例如通过使用扩展方法（很不幸，这次 Caller­MemberName 无法发挥作用）：

```
public static NotifyProperty<T> CreateNotifyProperty<T>(
  this IRaisePropertyChanged owner,
  Expression<Func<T>> nameExpression, T initialValue)
{
  return new NotifyProperty<T>(owner,
    ObjectNamingExtensions.GetName(nameExpression),
    initialValue);
}
// Listing of GetName provided earlier
```

通过这种方式，你仍将支付反射成本，但仅限于在创建对象时支付，而不是每次属性更改时都支付。如果（你在创建许多对象时）这仍然太过昂贵，你可以总是缓存对 GetName 的调用，并将其保存为视图模型类中的一个静态只读值。图 2 表示的是以上两种情况中的**简单视图模型**的示例。

具有NotifyProperty的基本视图模型
```
public class LogInViewModel : IRaisePropertyChanged
{
  public LogInViewModel()
  {
    // C# 6
    this.m_userNameProperty = new NotifyProperty<string>(
      this, nameof(UserName), null);
    // Extension method using expressions
    this.m_userNameProperty = this.CreateNotifyProperty(() => UserName, null);
  }
  private readonly NotifyProperty<string> m_userNameProperty;
  public string UserName
  {
    get
    {
      return m_userNameProperty.Value;
    }
    set
    {
      m_userNameProperty.SetValue(value);
    }
  }
  // Plus the IRaisePropertyChanged code in Figure 1 (otherwise, use a base class)
}
```

总的来说，我获得了更改“数据模型”端的灵活性，却以在“视图”端失去灵活性为代价。总之，由你决定是否优势取胜，并决定使用此方法进行绑定。

如果你对用于 XAML 中绑定的属性进行重命名，我会说，这未必成功。
备选做法是在代码隐藏文件中**手动编码数据绑定**。
```
// Constructor
public LogInDialog()
{
  InitializeComponent();
  LogInViewModel forNaming = null;
  //手动编码
  m_textBoxUserName.SetBinding(TextBox.TextProperty,
    ObjectNamingExtensions.GetName(() => forNaming.UserName);
  // Or with C# 6, just nameof(LogInViewModel.UserName)
}
```


另一篇C#程序优化：

三十八、定制和支持数据绑定

1. BindingMananger和CurrencyManager这两个对象实现了控件和数据源之间的数据传输；
2. 数据绑定的优势：使用数据绑定要比编写自己的代码简单得多；应该将它用于文本数据项之外的范围 —— 其他显示属性也可以被绑定；对于 Windowos Forms 数据绑定能够处理多个控件同步的检查相关数据源；
3. 在对象不支持所需的属性时，可以通过屏蔽当前的对象，然后添加一个想要的对象来支持数据绑定。