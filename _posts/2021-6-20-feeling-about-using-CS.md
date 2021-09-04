## C#使用感想
自从学了C++后，再回去写C#，发现C#真的是不知道如何描述，里面所有语法都是类型，函数、接口、迭代器等等，要使用某一个非逻辑符号，都得声明对象出来，才能投喂给C#编译器。

从在C++上非逻辑符号的原样，演变到C#的一切皆类型。

### 一切皆因“类型”


### 类型的开始-多态
```
class bird
{
    public virtual void shout()
    {

    }
}

class derived_bird : bird
{
    public override void shout()
    {
        WriteLine("I am derived");
    }
}

void call_bird(bird mBird)
{
    //取决于传入的类型是哪个子类
    mBird.shout();
}
```

- **静态语言**必须确定对象的类型！  
- 用接口的方式是作为多态的一种主流

### 类型的延续-委托
将函数委托成一个类型,是Lambda衍生出来的（C++）。
```
delegate float Mydelegate(float x);

class Program
{
    float amazing(Mydelegate m, float x)
    {
        return m(x);
    }

    float power(float x)
    {
        return (float)Math.Pow(x, 2);
    }

    main()
    {
        WriteLine(amazing(power, 6));
    }
}
```

.NET提供了一个已经定义好的Action委托，其无返回值，无参数，可以直接使用。

### 类型的进阶-泛型
与C++里的泛型算法类似，只用于容器。

注意：因泛型是可以传入完全不同的类型，所以泛型使用的内部，不能指定某个类型的自有方法（不同类型，无法判断）

所以泛型通常当容器等使用。

### 类型的高阶-泛型委托

泛型结合委托：
```
public delegate void Mydelegate<T>(T t);

class Program
{
    void doSomething(Mydelegate<string> m, string str)
    {
        m(str);
    }
    Main()
    {
        doSomething(Console.WriteLine, "hello");
    }
}
```

.NET为了方便也提供了Action的泛型委托。
泛型委托Action中可以多达16个参数，也有具备返回值多参数的泛型委托Func。

泛型还有另一种用途是反射，通常会在泛型函数里，函数里执行了GetType等操作，在asp.net core等框架中被大量使用。

Reference：
http://doc.laokoushuo.com/csclass/

### C# List使用感想
List容器支持了**泛型**接口IList<T>，泛型接口里面具有**通用**容器要有的方法（C#里惯称方法），索引、插入、移除三个方法。

#### 性能考虑
使用List<T>在多数情况下会更好，有值类型和引用类型的分类，值类型需要考虑实现和装箱。
在元素被使用之前，值类型的元素没必要装箱。但如果超过500个元素的list，保存到非装箱元素的内存大于生成引用类型需要的内存。

若使用值类型，则需要支持接口IEquatable，如果不支持，Contains方法会调用Object.Equals(Object)，该方法会对List元素装箱，同样需要支持IComparable接口，可以防止装箱元素二分查找和排序方法。

不要自定义强类型封装容器，因为.NET Framework已经为你实现了容器，且CLR可以共享微软中间语言代码和元数据。

### 语法糖
语法：词汇表上的字符串，可以由用户/第三方开发商添加更多字符串进去。

语法糖即是第三方为了用户设计了方便书写的字符串，使词汇表的含量更多了。

### 程序集概念
程序集中含有：类型元数据（描述程序中定义的每一类型和成员，二进制形式）、程集元数据（程序集清单、版本号、名称）、IL Code（这些都被装在exe、dll中）、资源文件。