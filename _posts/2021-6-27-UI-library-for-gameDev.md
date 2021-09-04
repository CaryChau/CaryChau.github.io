## UI工具库（游戏开发）
### [UnityUIExtensions](https://bitbucket.org/UnityUIExtensions/unity-ui-extensions/wiki/Home)
#### 新增UI组件：
- Accordion：Acordian 样式的控件，使用动画片段。

- ComboBox：组合框控件。

- HSVPicker：颜色选取器 UI。

- SelectionBox：RTS 样式选择框控件。

- HorizontalScrollSnap：弹性滚动列表。

- UIButton：改进的按钮控件与其他事件。

- UIWindowBase：可拖动窗口的实现。

- ComboBox：对于文本的固定组合框实现。

- AutoCompleteComboBox：自动完成选择文本组合框。

- DropDownList：一个基本的下拉列表中的文本和图像支持。

- BoundToolTip：工具提示。

#### Effect 组件

- BestFitOutline：改进的轮廓效果。
- CurvedText：Text顶点manipulator。
- Gradient：适用于任何UI对象的顶点颜色渐变。
- LetterSpacing：允许 finers 文字间距控件。
- NicerOutline：另一个outline 控件。
- RaycastMask：强化的mask 组件能够处理的图像数据的一个例子。启用对映像部件并不只是Rect Transform。
- UIFlippable：图像组件，翻转图形效果。

#### Additional 组件
- ReturnKeyTrigger：
- TabNavigation：选项卡导航。
- FlowLayoutGroup：更高效的网格样式布局组。

### Doozy UI
- 使用本地uGUI
- 简单易学，直观的设计
- 高级编辑器集成
- UI动画系统
- 用户界面导航系统
- UI效果，用户界面内的ParticleSystems
- 用户界面通知系统
- 用户界面触发器，零代码
- 场景加载器


### FairyGUI
该库有着自己的独立编辑器。
- 提供了丰富的组件，几乎涵盖游戏内所有常用效果。内置了一些游戏内常用效果：Loading、预告、强调等等
- 解决了UI中一些常见问题，如特效裁剪、模型裁剪
- 相对于UGUI和NGUI提出的动静分离的DrawCall优化方案，FairyGUI优化技术是动态后期优化，在制作时对UI人员基本没有要求。（在实际测试过程中使用UGUI和FairyGUI制作相同的界面，FairyGUI的DrawCall只有UGUI的1/3，但是Saved by batching却是后者的3倍，FairyGUI的文档中提到“深度调整技术”就是提交顶点数据到GPU前先通过CPU将材质调整到连续的RenderingOrder值上，虽然降低了GPU的使用但是却消耗了CPU的使用）


### IMGUI与QT
它们中的大多数是自定义小部件，或也非常特定于引擎的数据视图。 Qt 中的自定义小部件很快变得非常复杂。 如果数据是树/列表，您可以使用 QTreeView 创建delegate、hook、signal等。但是当它变得非常复杂时，您必须将其呈现为 QScrollArea 或 QPixmap。

IMGUI 中自定义小部件的代码具有硬件加速功能，您无需逐像素绘制和逐块绘制。 使用 IMGUI 执行此操作的代码要简单得多，并且对于任何从事游戏工作的人来说都非常熟悉。 为了说明这一点，这里是用 Qt qhexedit2、QHexEdit 编写的十六进制编辑器小部件，有数百万个用 Qt 编写的十六进制编辑器，您可以搜索它。

IMGUI 小部件是高度交互的，每帧都可以更新一切，对于传统的 UI 框架，这可能会带来巨大的性能损失，因为它是更传统的静态 UI 解决方案。

由于IMGUI已经在引擎里了，无需弄清楚如何在您的引擎正在使用的渲染器和框架的渲染 API 在下面使用的渲染器之间进行互操作。

### 渲染系统与GUI系统的区别




问题1：?数据驱动、技能处理与xml数据与**模板方法**模式

回答：用配置表配置的技能，里面包含的是有规律的数据，故需要模板方法来处理这个重复的问题。


问题2：UI框架

回答：了解认识fairyGUI

问题3：UI的逻辑与显示分层（子窗口、兄弟窗口、MVC）

回答：这种设计可以耦合性很低，管理起资源呢？

问题4：写UI的上面这些经验（以前写载具、地图界面的写法——对象之间的消息传递）与游戏开发的工具流的相似点

回答：当时知道属性可以用来传信息、还有自定义方法

问题5：有限状态机与游戏的联系，全流程工具链制作？
问题6：设计时要选用的模型、面对数学问题时要选用的模型
问题7：内存管理与上面的问题主题的联系
问题8：继承的出现是偷懒的结果
问题9：数据驱动的编程方式是什么？优点是什么？

回答：当转换思维方式之后，组件、事件、逻辑处理、样式都是一份数据，我们只需要把数据的**状态和转换**设计好，剩下的实现则由具现方式（模版引擎、事件机制等）来实现。

将重复的编程构造分解为数据和转换模式。当以这种方式使用时，这种新数据通常被纯粹主义者称为元数据。


Reference:
https://blog.codingnow.com/2020/07/game_ui.html