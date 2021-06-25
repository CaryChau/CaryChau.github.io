## GamePlay工具集

本文内容：

- 数种重要的Gameplay框架及插件，简述它们的原理
- 介绍这些Gameplay框架的适用场合，并进行对比

以下就开始介绍：

- [实体组件系统（Entity-Component-System）](https://en.m.wikipedia.org/wiki/Entity_component_system)
- [节点可视化编程（Node-based Visual Scripting）](https://en.m.wikipedia.org/wiki/Visual_programming_language)
  - [状态机（Finite State Machine）](https://en.m.wikipedia.org/wiki/Finite-state_machine)
  - [行为树（Behavior Tree）](https://en.m.wikipedia.org/wiki/Behavior_tree_(artificial_intelligence,_robotics_and_control))
  - [事件驱动可视化编程（Event Driven Visual Scripting）](https://en.m.wikipedia.org/wiki/Event-driven_programming)
  - [非线性编辑（Non-linear editing）](https://en.m.wikipedia.org/wiki/Non-linear_editing_system)


### 第三方Gameplay插件
Unity Asset Store里有一些比较不错的Gameplay具体实现插件。它们是：  
**状态机**：[NodeCanvas](https://www.assetstore.unity3d.com/en/#!/content/14914)、[PlayMaker](https://www.assetstore.unity3d.com/en/#!/content/368)  
**行为树**：[NodeCanvas](https://www.assetstore.unity3d.com/en/#!/content/14914)、[BehaviorDesigner](https://www.assetstore.unity3d.com/en/#!/content/15277)  
**事件驱动可视化编程**：[FlowCanvas](https://www.assetstore.unity3d.com/en/#!/content/33903)  
**非线性编辑**：[Unity Director Sequencer](http://unity3d.com/cn/unity/roadmap)（尚未发布）、[Slate、Flux](https://www.assetstore.unity3d.com/en/#!/content/56558)（出名但不好）

开发者应该发挥开发的能力去评估一款第三方插件是否优秀，评估的角度包括：
- 是否满足基本需求
- 是否开源（这很重要，因为代码即文档、文档不透彻更新不及时、二次修改的可能）
- 运行性能、反序列化性能
- 版本迭代、作者、社区是否活跃
- UI、操作、体验

如果决定了某个第三方插件，我们不应该轻易修改它，而是优先去扩展它。

第三方插件建议都摆放在“Standard Assets”目录下，因为该目录与其他文件夹的脚本是处于不同的两个dll，从而防止普通开发者错误地把具体项目业务逻辑感染进通用逻辑里。

这样子，我们可以通过继承、partial、extend、static function等途径进行第三方插件地扩展。  
对于一些不紧急的插件修改，可以通过社区和作者交流，让其进行修改。

### 实体组件系统（Entity-Component-System）
**生存期**

ECS提供API，进行ENtity、Component的生存期管理，以及生存期相关事件的**回调**，生存期以Unity的术语为例，一般指的是：
- 创建（Awake）
- 有效（OnEnable）
- 启动（Start）
- 轮转（Update）
- 无效（OnDisable）
- 销毁（OnDestroy）

实现生存期的**重难点**：
- 如何确保“同时”创建的Entity的所有Start都发生在Awake之后。比如可以使用**ms_gameObjectsWillStart列表**实现。
- 如何确保创建销毁不会影响轮转阶段。每一次Tick()都会对组件列表进行遍历调用Update()。用户在Update()内调用创建或销毁后，如果ECS立刻将其从列表中添加或移除，这将可能影响遍历逻辑。所以ECS会在**Tick的开始阶段或末尾阶段**才真正将Entity、Component添加或移除到最终列表里。比如可以使用**ms_gameObjectsWillStart列表**和**ms_gameObjectsWillDestroy队列**实现。
- 如何确保高效的轮转。比如通过接口（Unity通过反射检测Update()等函数）让用户有权限规定某些自定义的Component是否接受Update。

**通信**  
Entity之间可以通信、Component之间也可以通信。通信方式有以下三种：
- 事件（GameObject.SendMessage()）
- 搜索并直接依赖（GameObject.Find()、GameObject.GetComponent()）
- 也有一些做法，是将数据（**黑板**）也作为通信方式（GetProperty()、SetProperty()），但Unity并无此设计