## 阴影算法概述
渲染阴影场景涉及两个主要的绘图步骤。第一个生成阴影贴图本身，第二个将其应用于场景。
### 创建阴影贴图
- 从灯光角度渲染场景，提取深度缓冲区
- 