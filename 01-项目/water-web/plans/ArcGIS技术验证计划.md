# ArcGIS 技术验证记录 - 港区水务管理平台 GIS 功能增强

---

created: 2026-03-30
updated: 2026-03-31
tags: #记录 #技术验证 #ArcGIS #water-web
project: water-web
status: 已验证
---

## 结论

当前更准确的定位是：**ArcGIS GIS 功能增强的技术验证记录与实现现状说明**。

结论如下：

1. **管线流向动画已落地**，但实现方案不是 `CIMSymbol` 动画，而是自定义 Canvas 粒子动画层 `FlowLineLayer`。
2. **地图页面已经支持流向动画开关**，并根据 `direct` 字段区分单向和双向流动。
3. **主地图中的管线基础样式仍主要按 `m_LEVEL` 分级显示**，并未完全改造成“按 `c_SIZE` 决定管线线宽”的主渲染方案。
4. **阀门开关接口 `PUT /api/Adjust/OperCtrlDot` 已存在且已在业务页使用**，但目前并未直接接入 ArcGIS 地图弹窗。

因此，这项工作现在的真实状态不是“待验证”，而是：

- 一部分已经完成实现
- 一部分已经完成技术选型
- 一部分仍可作为后续增强项继续开发

---

## 当前代码实现概览

### 1. 地图基础能力

当前 GIS 主页面位于：

- `src/views/business/arcgis/index.vue`
- `src/views/business/arcgis/maphook.ts`

已确认能力：

- ArcGIS API for JavaScript **4.25**
- 通过 `esri-loader` 动态加载模块
- 使用 `CIMSymbol` 绘制主干/支管/末端管线样式
- 使用独立图层管理节点、管线、分区、标签
- 地图支持帮助面板、底图切换、搜索定位、图层操作

### 2. 数据来源

当前 ArcGIS 页面主要使用以下接口：

- `getNodeAll()`：获取节点数据
- `getDotDirect()`：获取上下级节点连线关系与流向信息
- `getAreaData()`：获取区域分区数据

已在代码中确认的关键字段：

- `direct`：流向，`0=未检测`、`1=单向`、`2=双向`
- `c_SIZE`：管径，如 `DN100`
- `deV_CLASS`：节点类别，可识别阀门点等类型

---

## 分项验证结论

### 一、管线流动动画

### 原验证目标

验证 ArcGIS 4.25 是否支持使用 `CIMSymbol` 动画实现管线流动效果。

### 当前真实实现

**结论：已实现，但未采用 `CIMSymbol` 动画方案。**

当前代码在 `src/views/business/arcgis/index.vue` 中：

- 使用 `getDotDirect()` 获取有流向的管线关系
- 将 `direct > 0` 的线段收集到 `polylinesWithFlow`
- 创建 `new FlowLineLayer(mapView, polylinesWithFlow)` 实现动画层

具体动画实现位于：

- `src/views/business/arcgis/flow-animation/FlowLineLayer.ts`
- `src/views/business/arcgis/flow-animation/utils.ts`

技术特征：

- 基于 **Canvas 叠加层** 绘制粒子流动效果
- 使用 `requestAnimationFrame` 驱动动画
- 支持 **单向流动** 和 **双向流动**
- 根据 `c_SIZE` 解析管径，动态调整粒子尺寸
- 在低缩放级别可暂停动画
- 视口外线段自动跳过，降低无效绘制

页面中还已提供开关：

- `info.flowAnimationEnabled`
- `toggleFlowAnimation()`

这说明“流向动画是否可行”已经被代码验证完成，且系统已选择 **自定义 Canvas 动画** 作为最终实现路径。

### 技术判断

这也间接说明：

- `CIMSymbolAnimationOffset` 并不是当前系统采用的正式方案
- 项目已经用更可控的 Canvas 层完成了替代实现
- 这部分不应继续作为“待验证”事项存在

---

### 二、管道粗细按管径显示

### 原验证目标

将管道线宽从按 `m_LEVEL` 显示，改为按 `c_SIZE` 显示。

### 当前真实实现

**结论：已实现，主地图线宽逻辑已切换到真实管径驱动。**

根据 2026-04-08 的实现结果，`src/views/business/arcgis/index.vue` 已接入 `getLineAll`，并建立 `id -> c_SIZE` 索引。主地图初始化画线与缩放后的线宽更新，均已统一改为按真实 `c_SIZE` 驱动。

同时，流动动画层也继续沿用按管径计算视觉参数的实现：

- `parsePipeSize(sizeStr)`
- `getLineWidthBySize(sizeStr)`
- `calcParticleSize(lineWidth)`

对应文件：

- `src/views/business/arcgis/flow-animation/utils.ts`

说明当前状态是：

- `c_SIZE` 已经进入主地图渲染逻辑
- 主地图基础线宽已完成从 `m_LEVEL` 到 `c_SIZE` 的切换
- 静态管线与流动动画已使用同一套尺寸口径

### 技术判断

因此这项能力不应再标记为未完成，更准确的表述应为：

- **字段和算法已验证可用**
- **主地图基础线宽已完成从 `m_LEVEL` 到 `c_SIZE` 的切换**
- **动画层与主地图已统一使用管径驱动的尺寸口径**

---

### 三、阀门开关控制

### 原验证目标

在地图上直接开关阀门。

### 当前真实实现

**结论：接口和业务能力已存在，但 ArcGIS 地图页尚未接入。**

已确认接口：

- `src/views/business/water-control/api.ts`
- `dotSwitch(params)` -> `PUT /api/Adjust/OperCtrlDot`

说明“阀门开关控制”在系统中并不是空白能力，而是已有独立业务页支持。

但从 ArcGIS 页面当前代码看：

- `src/views/business/arcgis/dialog/nodePopupTemp.vue` 仅展示节点详情、绑定设备和实时信息
- 弹窗中没有阀门控制按钮
- ArcGIS 主页中也没有直接调用 `dotSwitch()`

也就是说：

- **阀门控制接口已验证存在**
- **阀门控制业务能力已实现**
- **但“地图弹窗直接控制阀门”这一步尚未落地到 ArcGIS 页面**

### 技术判断

这项内容现在更适合归类为：

- 非“技术可行性验证”问题
- 而是一个明确的 **页面集成增强项**

---

## 对原计划结论的修正

### 原计划中已经过时的部分

以下表述已不适合继续保留为“计划”措辞：

- “验证 ArcGIS 4.25 对 `CIMSymbol` 动画的支持程度”
- “如果支持则使用 `CIMSymbol` 动画，否则使用 Canvas 动画”
- “验证流动动画是否可行”
- “验证阀门控制接口是否存在”

因为当前代码已经给出真实答案：

- 流动动画：**已实现，采用 Canvas 动画层**
- 阀门控制接口：**已存在并已封装**
- 管径字段：**已进入主地图与动画渲染逻辑，主线宽改造已完成**

### 当前更合理的状态划分

| 项目 | 当前状态 | 说明 |
| --- | --- | --- |
| 管线流动动画 | 已实现 | 使用 `FlowLineLayer` Canvas 粒子动画 |
| 流向开关 | 已实现 | GIS 页面已有开关控制 |
| 双向流动表现 | 已实现 | `direct = 2` 时使用双向粒子 |
| 管径参与视觉计算 | 已实现 | 已用于主地图线宽与动画粒子大小 |
| 主地图线宽按管径显示 | 已实现 | 主地图已按真实 `c_SIZE` 动态映射 |
| 阀门控制接口 | 已实现 | `dotSwitch()` 已封装 |
| 地图弹窗直接阀门控制 | 未完成 | ArcGIS 页面尚未接入 |

---

## 建议的文档定位

这篇文档后续建议不再作为“计划”使用，而应作为：

- **ArcGIS 技术验证记录**
- 或 **GIS 功能增强实现现状记录**

如果后续还要继续推进 GIS 增强，更建议新增一篇真正面向现在的文档，例如：

- `ArcGIS GIS增强后续开发清单.md`
- `ArcGIS 地图页待完成功能.md`

可继续追踪的真实待办应当是：

1. 在节点弹窗中接入阀门开关控制
3. 根据业务需要决定是否保留或弱化箭头 + 粒子动画的双重表达
4. 评估首页地图 `home-page-1` 与主 ArcGIS 页是否需要统一渲染策略

---

## 相关代码位置

### ArcGIS 主页面

- `src/views/business/arcgis/index.vue`
- `src/views/business/arcgis/maphook.ts`

### 流动动画

- `src/views/business/arcgis/flow-animation/FlowLineLayer.ts`
- `src/views/business/arcgis/flow-animation/utils.ts`

### 地图弹窗

- `src/views/business/arcgis/dialog/nodePopupTemp.vue`

### 阀门控制接口

- `src/views/business/water-control/api.ts`
- `src/views/business/water-control/index.vue`

---

## 最终判断

**这篇文档已经过时，如果继续保留“技术验证计划”这个标题，会误导后续阅读者。**

更准确的现实是：

- 验证阶段已经结束
- 一部分功能已经实现
- 一部分功能还差页面集成或进一步优化

所以本次已将其改写为基于当前真实代码的技术验证记录。

---

*最后更新：2026-03-31*
