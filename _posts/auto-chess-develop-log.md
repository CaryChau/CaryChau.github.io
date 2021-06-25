## 自走棋开发日记（一）

### 收集策略棋牌类型游戏信息



### 反编译APK包
1. APK包解压后的目录

| 目录 | 介绍 |
| ----- | ---- |
| assets    | 原始资源文件夹，对应着Android工程的assets文件夹，一般用于存放原始的图片、txt、css等资源文件  |
|lib | 存放应用需要的引用第三方SDK的so库。比如一些底层实现的图片处理、音视频处理等。这是根据不同CPU 型号而划分的，如 ARM，ARM-v7a，x86等 |
|META-INF | 保存apk签名信息，保证apk的完整性和安全性 |
|res |资源文件夹，其中的资源文件包括了布局(layout)，常量值(values)，颜色值(colors)，尺寸值(dimens)，字符串(strings)，自定义样式(styles)等 |
| AndroidManifest.xml | 全局配置文件，里面包含了版本信息、activity、broadcasts等基本配置。不过这里的是二进制的xml文件，无法直接查看，需要反编译后才能查看。 |
| classes.dex| 安卓代码核心部分，dex是在Dalvik虚拟机上可以执行的文件。如果有classes.dex和classes2.dex两个文件，说明工程的方法数较多，进行了拆分|
| resources.arsc| 记录资源文件和资源id的映射关系|

2. 通过AssetStudio.exe提取AB包

3. Assembly-CSharp.pdb文件的利用


### Unity编译Android的原理解析和APK打包分析

由于UnityPlayer类做了混淆，关于渲染的核心功能也封装在native代码中，关于Scene转换到到UnityPlayer作为FrameLayout，只能做一个简单的推测：通过调用Android的GL渲染引擎，在native层进行渲染，并同步到FrameLayout在UnityPlayerActivity上进行显示。

五、Unity打包Android apk的结构探究
通过对比Unity打包的apk，与普通的Android apk的文件差别，找到Unity文件存放的目录，对应存放在Android Studio目录，最后通过AS完成对Unity相关文件的打包。


### 技能编辑器
#### 序列化问题
1. 多态序列化
2. PropertyAttribute扩展Inspector
3. 嵌套ScriptableObject可以保存路径解耦
4. SerializedObject和ScriptableObject配合使用，可以方便在EditorWindow里面绘制内容

#### 使用Odin Inspector建议
- 使用Odin写的Editor Window是非常方便快捷的，比Unity自带的EditorWindow以非常短的时间开发更好的Window
- MonoBehaviour类需继承SerializedMonoBehaviour，该类尽量不要作为预制体去处理。


### 自定义编辑器窗口
#### asmdef目的
自定义明晰的依赖关系

#### 资源更新问题
1. application.streamingAssetes获取的路径是已经带了file:/或者jar:file:/协议的，application.persistentPath获取到的路径是不带协议的
2. streamingAssetes在android和ios上是只读的，并且不能用File操作
3. File操作的路径是不需要带文件协议的

#### [资源加载与优化（一）](https://blog.csdn.net/smile_Ho/article/details/107288409)


#### 动态库之间的引用问题

