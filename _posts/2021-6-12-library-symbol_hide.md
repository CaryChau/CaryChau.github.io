## 动态库、静态库、符号隐藏

在实际应用中，我们需要动态导出特定的符号（有些接口不对外暴露，只内部适用）。

**为什么动态库会隐藏函数/类？**

- 输出库符号受限提高程序的模块性，并隐藏细节
- 动态库的符号越少，启动和运行越快
- 导出所有符号会增加内存

**__declspec**

- MSVC：微软的C++编译器
- 拓展修饰：不同的编译器对C++增加了标准语法外的魔法糖
- __declspec：拓展修饰符
- __declspec(dllexport)：在DLL源文件中声明要输出的C++类、函数及数据。
- __declspec(dllimport)：在外部程序声明由DLL输出的C++类、函数及数据。

**MSVC如何封装动态库**
- 封装：类/函数在库外部给别人用：使用__declspec(dllexport)修饰类/函数

**cmake和头文件如何写**

- 只有msvc才有动态库封装部分接口给外部使用，其他的如gcc会封装所有的函数给外部使用
- **封装**使用__declspec(dllexport)修饰函数/类，**调用**使用__declspec(dllimport)修饰函数/类
```
// 动态库封装的cmakelist.txt
if(WIN32)
    if(MSVC)
        add_definitions(-DUSE_SHARED)
    else()
        add_definitions(-DNO_MSVC)
    endif()
endif()

// 静态库封装的cmakelist.txt
if(WIN32)
    if(MSVC)
        add_definitions(-DUSE_STATIC)
    else()
        add_definitions(-DNO_MSVC)
    endif()
endif()

// Qt的封装
#define Q_DECL_EXPORT __declspec(dllexport)
#define Q_DECL_IMPORT __declspec(dllimport)
```

**其他编译器如何实现封装部分函数？**

- GCC4.0后，提供visibility属性，可以应用到函数、变量、模板以及C++类
```
#ifdef __GNUC__ >= 4
    #ifdef FBC_EXPORT
        #define FBC_API_PUBLIC __attribute__((visibility ("default")))
        #define FBC_API_LOCAL __attribute__((visibility("hidden")))
    #else
        #define FBC_API_PUBLIC
        #define FBC_API_LOCAL
    #endif
#else
    #error "------ requires gcc version >= 4.0 ------"
#endif
```

**静态库__declspec()使用？**

使用MSVC
- 封装静态库：使用__declspec(dllexport)修饰
- 使用静态库：不能用__declspec(dllimport)修饰，因为找不到动态库
- 建议：静态库的代码dllexport、dllimport都不要用

**静态库隐藏符号功能**

gcc可以直接 -fvisibility=hidden
每个cpp文件为一个编译单元，单元有三类符号（不包含局部非静态程序变量）：

- 全局符号
- 自己用的符号
- 依赖外部的符号

**编译阶段**：只要声明就可以  
**链接阶段**：链接器就是把这些符号相互链接，少了那个都会链接失败。

静态库加载动态库后使用、动态库加载静态库后使用？

//Todo
```
静态库没有链接这个步骤，静态库如果用了其他静态库或者动态库需要手动链接。
动态库有链接步骤，链接静态库则把静态库全部放进来，链接动态库则只放入符号再程序启动时候链接。
动态库可以由多个程序共享, 节省内存，易于升级。静态库外部依赖少, 更易于部署。
```

