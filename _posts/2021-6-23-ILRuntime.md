## ILRuntime
ILRuntime不同于CLR自家提供的JIT编译器，ILRuntime是一个IL运行时，使得在没有JIT的环境下实现代码的热更新。

ILRuntime透过Mono.Cecil库来读取DLL的PE信息.

ILRuntime利用unsafe和非托管内存实现了自己的IL托管栈,数据结构的定义:
```
struct StackObject
{
    public ObjectTypes ObjectType;
    public int Value; //高32位
    public int ValueLow; //低32位
}
enum ObjectTypes
{
    Null,//null
    Integer,
    Long,
    Float,
    Double,
    StackObjectReference,//引用指针，Value = 指针地址, 
    StaticFieldReference,//静态变量引用,Value = 类型Hash， ValueLow= Field的Index
    Object,//托管对象，Value = 对象Index
    FieldReference,//类成员变量引用，Value = 对象Index, ValueLow = Field的Index
    ArrayReference,//数组引用，Value = 对象Index, ValueLow = 元素的Index
}
```

