## 时间是幻觉
### 调查
独立游戏人  
[E胖](http://www.chuapp.com/article/281875.html)  
先前E胖是独立漫画人，具备使用绘画表达想法的能力。  
[以撒潘祭](https://twinfinite.net/2012/10/big-sloppy-slomper-chompers/4/)

吹哥  
鱼哥  
Tommy，[游戏开发的感想](https://isetta.io/interviews/TommyRefenes-interview/)


### 时间是幻觉（物理界说法）
[爱因斯坦：过去和未来可能只是幻觉](https://tech.sina.com.cn/d/i/2020-04-15/doc-iirczymi6379170.shtml)

[Nature文章](https://www.nature.com/articles/d41586-018-04558-7)：
整个宇宙都遵循量子力学和热力学定律，时间由此而生。

与热力学和贝叶斯概率论有相似之处，它们都依赖于熵的概念，因此可以用来论证时间的流动是宇宙的主观特征，而不是物理描述的客观部分。  
风暴不是一件事，而是一系列事件的集合。  
他认为，我们对时间流动的感知完全取决于我们无法了解世界的所有细节。  
量子不确定性导致我们不能知道宇宙中所有粒子的位置和速度。

[回圈量子引力](https://zh.wikipedia.org/zh-hans/%E8%BF%B4%E5%9C%88%E9%87%8F%E5%AD%90%E9%87%8D%E5%8A%9B)

[AssetsFromMayaToUnity](https://blog.csdn.net/wangyihero8/article/details/104804587)

### Tetris的NP性问题
资料：  
https://www.osgeo.cn/post/11226

### 多个摄像机的组合使用
https://blog.csdn.net/qq_42672770/article/details/119148259

### 场景淡入淡出
**思路**：用一层图像，进行透明度变化，覆盖当前渲染。


### Unity引擎的一些细节
1. assets 资产和scene objects场景对象之间的区别

   1. 场景对象只存在于一个单一的场景。这些包括GameObjects 游戏对象和它们的components 组件以及prefabs预置的实例。Assets 存在project 中，可以在任何场景中引用。这些包括动Animator Controllers, prefab assets 和 StateMachineBehviours。由于scene 与一个特定的对象可能不会loaded加载，asset 资产不能引用一个场景对象。由于asset 资产总是存在于project中，场景objects 可以引用assets。

这样的话，若要实现信息转移，需要不停地写入到硬盘，再读取。 

### 摄像机Camera.main问题
Internally, Unity caches all GameObjects with the "MainCamera" tag. When you access this property, Unity returns the first valid result from its cache.

上文表明，摄像机main已经是缓存起来的了。

### 静态物、动态物
能者多劳

### Unity的脚本周期与enabled
Active如果一开始就用了的话，那么start()周期时不会对false的作用。

### Unity闪退