# ArcGIS Interaction Decoupling Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** 按图层能力重新拆分 ArcGIS 地图交互逻辑，统一节点与区域的命中/选中/编辑链路，同时把管线图层明确收口为只读详情链路，降低 `index.vue` / `maphook.ts` 耦合度。

**Architecture:** 地图交互拆成“图层能力定义、事件分发、命中解析、选中状态、业务控制器”五层。节点与区域属于“可选中 + 可编辑”图层，管线属于“只读详情”图层，辅助标签图层属于“辅助命中”图层，三类图层不再混用同一套交互模型。

**Tech Stack:** Vue 3 Composition API、TypeScript、ArcGIS 4.25、esri-loader、Element Plus。

---

## 进度同步（2026-04-24）

### 已完成

- 已建立图层能力定义：`types.ts`、`layerCapabilityRegistry.ts`
- 已抽离命中解析：`nodeHitController.ts`、`areaHitController.ts`、`lineHitController.ts`
- 已建立统一选中态入口：`selectionStateController.ts`，旧的 `info.attributes` / `info.checkedItems` / `info.checkedCount` 镜像字段已移除
- 已完成节点交互层与节点业务分流层第一版：`nodeInteractionController.ts`、`generalNodeCrudController.ts`、`companyNodeCrudController.ts`
- 已完成 `sketchController.ts`，已关闭默认 `updateOnGraphicClick`，节点与区域点击统一手动进入 `sketchVM.update([graphic])`
- 已完成 `areaInteractionController.ts` 兼容清理，区域交互已统一依赖 selection state
- 已完成 `lineDetailController.ts`，管线详情正式从编辑链路中剥离
- 已完成 `mapInteractionController.ts`，`maphook.ts` 点击事件已只保留事件绑定与分发
- 页面层对旧字段 `info.attributes` / `info.checkedItems` / `info.checkedCount` 的直接依赖已清理
- 已删除废弃的 `utils/areaEditController.ts` 旧实现

### 进行中

- `index.vue` 仍保留少量按钮包装函数与 refresh 入口，距离“只负责 UI 和 controller 装配”的最终形态还有继续瘦身空间

### 待验证

- 按文档底部的手动验证清单实测节点、区域、管线三类链路

## 背景与核心问题

当前 `src/views/business/arcgis/index.vue` 与 `src/views/business/arcgis/maphook.ts` 同时承担了以下职责：

- 地图点击事件注册
- `hitTest` 命中结果解析
- 浏览态详情分发
- 绘图态编辑分发
- `SketchViewModel` 生命周期监听
- 页面按钮状态同步
- 弹窗编辑、拖拽更新、删除、刷新链路

这导致几个结构性问题：

1. 没有先定义“图层能力”，导致节点、区域、管线容易被误当成同一种交互对象。
2. 节点与区域的命中规则不一致；浏览态详情与绘图态编辑也未完全共享命中结果。
3. `info.attributes`、`info.checkedItems`、`SketchViewModel` 内部选中态是三套并存状态。
4. 区域已经开始 controller 化，节点还停留在页面散写，结构继续演化会越来越分裂。
5. 公司节点和普通节点既有共性，又有明显业务差异，目前没有清晰分层。

## 图层交互能力矩阵

这一节是本次计划的前提。后续所有拆分都必须建立在“不同图层能力不同”的前提上，而不是按“全都可编辑”设计。

### A. 只读详情型图层

这类图层只允许点击查看详情，不进入编辑体系：

- 管线图层 `PonitLayer.line`
- 未来如果有其它只展示 popup 的业务图层，也归到这一类

能力边界：

- 支持：浏览态点击、详情 popup、详情刷新回调
- 不支持：Sketch 编辑、选中框、修改按钮、删除按钮、批量选中

### B. 可选中 + 可编辑型图层

这类图层需要进入统一的选中态与编辑态体系：

- 普通节点图层
- 公司节点图层
- 区域图层

能力边界：

- 支持：命中解析、单选/多选、Sketch 编辑、修改按钮、删除按钮
- 其中节点支持批量排序 / 批量删除，区域仅支持当前设计允许的编辑和删除

### C. 辅助命中型图层

这类图层不直接承载业务编辑，但允许点击后映射回真实业务图形：

- 区域标签图层 `areaLabelLayer`
- 未来如果节点标签也要支持点击，则节点标签图层也属于这一类

能力边界：

- 支持：命中后映射回主图形
- 不支持：直接进入 popup 业务、直接进入 Sketch 业务

## 目标状态

重构完成后需要满足：

- `mapView.on("click")` 只负责事件分发，不写节点/区域/管线业务细节。
- 节点、区域、管线都有各自明确的命中规则和交互边界。
- 浏览态详情与绘图态编辑共享标准化命中对象，不再各自猜目标 graphic。
- 页面按钮只读取统一的 selection state，不再直接依赖 `info.attributes`。
- 节点与区域编辑统一手动接管 `SketchViewModel.update(...)`，不再混用默认点击编辑。
- 管线图层不进入编辑体系，只保留详情 controller。
- 公司节点与普通节点共享交互层，但 CRUD 与刷新逻辑继续分流。

## 目录设计

### 新增文件

- `src/views/business/arcgis/controllers/types.ts`
- `src/views/business/arcgis/controllers/layerCapabilityRegistry.ts`
- `src/views/business/arcgis/controllers/mapInteractionController.ts`
- `src/views/business/arcgis/controllers/nodeHitController.ts`
- `src/views/business/arcgis/controllers/areaHitController.ts`
- `src/views/business/arcgis/controllers/lineHitController.ts`
- `src/views/business/arcgis/controllers/selectionStateController.ts`
- `src/views/business/arcgis/controllers/nodeInteractionController.ts`
- `src/views/business/arcgis/controllers/generalNodeCrudController.ts`
- `src/views/business/arcgis/controllers/companyNodeCrudController.ts`
- `src/views/business/arcgis/controllers/areaInteractionController.ts`
- `src/views/business/arcgis/controllers/lineDetailController.ts`
- `src/views/business/arcgis/controllers/sketchController.ts`

### 主要修改文件

- `src/views/business/arcgis/index.vue`
- `src/views/business/arcgis/maphook.ts`

## 状态模型重构

把当前混杂在 `info` 里的交互状态拆成三组语义状态。

### 1. interactionState

负责地图模式和当前操作图层：

- `mode: "browse" | "draw"`
- `activeLayer`
- `activeLayerName`
- `activeFeatureType: "node" | "area" | "line" | null`
- `layertype`

### 2. selectionState

负责当前选中对象，仅服务“可编辑图层”：

- `selectedGraphic`
- `selectedGraphics`
- `selectedFeatureType`

约束：

- 单选详情、修改弹窗、当前选中名称展示都只读 `selectedGraphic`
- 批量排序、批量删除都只读 `selectedGraphics`
- `checkedCount` 改成派生值，不再手写维护
- `info.attributes` 逐步淘汰
- 管线图层不写入 `selectionState`

### 3. viewState

负责页面 UI 但不参与编辑语义：

- `coordinates`
- `mapShow`
- `isOpen`
- `flowAnimationEnabled`

## 模块职责拆分

### A. layerCapabilityRegistry

先定义每个图层具备什么交互能力，避免在点击事件里到处写 `if/else` 猜图层。

输出建议：

- `featureType`
- `supportsDetail`
- `supportsSelection`
- `supportsSketchEdit`
- `supportsBatchActions`
- `supportsAuxiliaryHit`

这个 registry 是整个系统的前置配置层。

### B. mapInteractionController

负责统一注册和调度地图原始事件：

- `bindMapClick(view)`
- `bindPointerMove(view)`
- `handleBrowseClick(...)`
- `handleDrawClick(...)`

规则：

- 浏览态点击只允许走详情链路
- 绘图态点击只允许走 selection / edit 链路
- controller 本身不理解节点、区域、管线细节，只依赖 capability registry 和各自的 hit controller

### C. nodeHitController

负责节点命中规则统一：

- 过滤 `hitTestResults` 中非节点要素
- 统一公司节点与普通节点的命中选择逻辑
- 处理重叠点时的优先级策略
- 输出标准化 node graphic

关键要求：

- 浏览态 popup 与绘图态编辑必须共用这一套命中结果
- 公司节点与普通节点在“命中解析层”合并，不要分裂成两套点击规则

### D. areaHitController

负责区域命中规则统一：

- 面图层直接命中 polygon
- 标签图层命中后按 `id` 回查 polygon
- 命中非区域对象时返回 `null`

后续如果编辑态要临时隐藏标签层、或 hover 标签联动面高亮，只改这里。

### E. lineHitController

负责管线命中规则统一：

- 从 `hitTestResults` 中解析标准化 line graphic
- 输出只读详情对象

边界：

- 不写 selection state
- 不触发 Sketch 编辑
- 不进入按钮状态控制

### F. selectionStateController

负责统一维护选中态，仅面向可编辑图层：

- `setSingleSelection(graphic, featureType)`
- `setMultiSelection(graphics, featureType)`
- `clearSelection()`
- `syncSelectionFromSketchEvent(event)`

要求：

- 页面任何修改/删除/排序按钮都不能再直接散写 `attributes` / `checkedItems`
- 节点、区域共用这组接口
- 点击空白或点击非当前可编辑对象时，统一清理选中态

### G. nodeInteractionController

这是节点的“共性交互层”，节点和公司节点在这里合并。

负责：

- `activateNodeEdit(graphic)`
- `clearNodeSelection()`
- `openNodeEditDialog(graphic)` 的统一入口
- 节点与公司节点的 Sketch 激活逻辑
- 把命中的节点 graphic 交给后续 CRUD controller 分流

### H. generalNodeCrudController / companyNodeCrudController

这是节点的“业务执行层”，普通节点与公司节点在这里分流。

普通节点负责：

- 新增普通节点
- 修改普通节点
- 拖拽更新普通节点
- 删除普通节点
- 刷新普通节点和相关管线

公司节点负责：

- 新增公司节点
- 修改公司节点
- 拖拽更新公司节点
- 删除公司节点
- 刷新公司节点，不影响普通节点与管线

原则：

- 交互层合并
- 业务层分流
- 不建议完全合并为一个大 controller，也不建议点击链路完全拆成两套

### I. areaInteractionController

负责区域编辑相关业务入口：

- `activateAreaEdit(graphic)`
- `openAreaEditDialog(graphic)`
- `handleAreaCreate(event)`
- `handleAreaUpdate(event)`
- `handleAreaDelete()`

职责边界：

- 区域标签点击与面点击统一进入同一套区域编辑链路
- 编辑失败时统一回滚 `sketchVM.undo()`
- 命中无效区域时清除 selection state 并取消编辑态

### J. lineDetailController

负责管线详情行为：

- `openLineDetail(graphic)`
- `refreshAfterLineDetailAction()`

职责边界：

- 只服务只读详情图层
- 不接 `SketchViewModel`
- 不写 selection state
- 不参与编辑按钮逻辑

### K. sketchController

负责创建与绑定 `SketchViewModel`：

- `createSketchVM(view, layer, featureType)`
- `bindCreateHandlers(...)`
- `bindUpdateHandlers(...)`
- `bindDeleteHandlers(...)`
- `destroySketchVM()`

关键要求：

- 节点与区域绘图态统一关闭默认 `updateOnGraphicClick`
- 点击命中后再显式进入 `sketchVM.update([graphic])`
- `SketchViewModel` 不再直接决定页面侧选中状态写法
- 管线图层完全不创建 Sketch 编辑链路

## 节点与公司节点的设计结论

这一点需要单独写清楚，避免后续又走偏。

### 结论

- 命中逻辑合并
- 选中逻辑合并
- Sketch 激活逻辑合并
- 业务 CRUD 与刷新逻辑分流

### 原因

共性：

- 都是点要素
- 都可选中
- 都可进入编辑态
- 都能拖拽更新几何位置
- 都需要按钮状态与当前选中对象一致

差异：

- 接口不同
- DTO 不同
- 弹窗不同
- 刷新逻辑不同
- 删除逻辑不同

因此不建议：

- 完全合并成一个大而全的 node controller
- 或完全拆成两条独立点击链路

推荐：

- `nodeHitController + nodeInteractionController` 合并复用
- `generalNodeCrudController + companyNodeCrudController` 明确分流

## 管线图层的设计结论

这一点也必须写死。

### 结论

管线图层属于“只读详情型图层”，不进入编辑控制器体系。

### 保留的能力

- 浏览态点击详情
- popupTemplate 或详情面板展示
- 详情操作后的刷新

### 明确排除的能力

- 进入 Sketch 编辑
- 进入 selection state
- 修改/删除按钮启用逻辑
- 批量操作逻辑

### 直接影响

后续 controller 设计中，不能再把管线纳入 `selection/edit` 主链路。

## 实施阶段

### Task 1: 建立图层能力注册表与统一类型（已完成）

**Files:**
- Create: `src/views/business/arcgis/controllers/types.ts`
- Create: `src/views/business/arcgis/controllers/layerCapabilityRegistry.ts`
- Modify: `src/views/business/arcgis/index.vue`
- Modify: `src/views/business/arcgis/maphook.ts`

**Steps:**
1. 定义 `FeatureType`、`InteractionMode`、`LayerCapability`、`SelectionState` 等类型。
2. 把普通节点、公司节点、区域、管线、辅助标签图层的能力写入 registry。
3. 将 `info` 里的旧字段映射到新的语义状态模型，但暂时保留兼容读写。
4. 为页面提供统一读取函数，例如“当前图层是否支持编辑”“当前图层是否支持详情”。
5. 运行 `pnpm typecheck`。

**Expected Result:**
后续所有逻辑都先查 capability registry，再决定走哪条交互链路。

### Task 2: 抽离命中解析控制器（已完成）

**Files:**
- Create: `src/views/business/arcgis/controllers/nodeHitController.ts`
- Create: `src/views/business/arcgis/controllers/areaHitController.ts`
- Create: `src/views/business/arcgis/controllers/lineHitController.ts`
- Modify: `src/views/business/arcgis/maphook.ts`

**Steps:**
1. 把节点 `hitTest` 解析从 `maphook.ts` 迁出。
2. 把区域标签 / 面图层命中解析迁出。
3. 新增管线专用命中解析。
4. 定义统一的标准化命中返回结构。
5. `maphook.ts` 点击事件只保留“按 capability 调对应 hit controller”。
6. 运行 `pnpm typecheck`。

**Expected Result:**
`maphook.ts` 不再出现节点/区域/管线的具体命中细节。

### Task 3: 抽离 selectionStateController（已完成）

**Files:**
- Create: `src/views/business/arcgis/controllers/selectionStateController.ts`
- Modify: `src/views/business/arcgis/index.vue`
- Modify: `src/views/business/arcgis/maphook.ts`

**Steps:**
1. 把 `attributes`、`checkedItems`、`checkedCount` 的直接赋值迁到 controller。
2. 所有按钮禁用态改成读取统一 selection state。
3. 区域点击无命中时，统一执行 `clearSelection()`。
4. 节点点击无命中时，也统一执行 `clearSelection()`。
5. 管线详情点击不写入 selection state。
6. 运行 `pnpm typecheck`。

**Expected Result:**
页面侧不再存在“显示状态已清空，但 Sketch 仍选中”或反过来的状态分裂。

### Task 4: 抽离节点交互层与节点业务分流层（已完成第一版）

**Files:**
- Create: `src/views/business/arcgis/controllers/nodeInteractionController.ts`
- Create: `src/views/business/arcgis/controllers/generalNodeCrudController.ts`
- Create: `src/views/business/arcgis/controllers/companyNodeCrudController.ts`
- Modify: `src/views/business/arcgis/index.vue`

**Steps:**
1. 将 `nodeEdit`、`pointCreate`、`pointUpdate`、节点删除前置逻辑迁出页面。
2. 建立统一节点交互层，负责激活编辑与选中态同步。
3. 将普通节点和公司节点 CRUD 逻辑拆到各自 controller。
4. `nodeEdit()` 改为只读取 `selectionState.selectedGraphic`。
5. 批量排序和批量删除改为只读取 `selectionState.selectedGraphics`。
6. 运行 `pnpm typecheck`。

**Expected Result:**
节点交互统一，业务分流明确，普通节点与公司节点既不硬并也不完全分裂。

### Task 5: 抽离区域交互层（已完成）

**Files:**
- Modify: `src/views/business/arcgis/controllers/areaInteractionController.ts`
- Modify: `src/views/business/arcgis/index.vue`

**Steps:**
1. 将当前区域 controller 收敛成“只负责区域交互与区域 CRUD”。
2. 从中移除命中解析逻辑，改为依赖 `areaHitController`。
3. 把 `areaCreate`、`areaUpdate`、`areaEdit`、`beforeAreaDelete` 迁进去。
4. 清理区域编辑态下的状态同步与失败回滚逻辑。
5. 运行 `pnpm typecheck`。

**Expected Result:**
区域点击、区域修改按钮、区域删除按钮都依赖同一 selection state 和同一交互控制器。

### Task 6: 抽离管线详情控制器（已完成）

**Files:**
- Create: `src/views/business/arcgis/controllers/lineDetailController.ts`
- Modify: `src/views/business/arcgis/index.vue`
- Modify: `src/views/business/arcgis/maphook.ts`

**Steps:**
1. 将管线详情打开逻辑收口到独立 controller。
2. 管线只走浏览态详情链路，不接入 selection state。
3. 如果详情操作需要刷新，只保留刷新接口，不接入编辑按钮体系。
4. 清理任何把管线误判为可编辑对象的逻辑。
5. 运行 `pnpm typecheck`。

**Expected Result:**
管线图层正式从编辑体系中剥离。

### Task 7: 抽离 sketchController 并统一手动接管编辑态（已完成）

**Files:**
- Create: `src/views/business/arcgis/controllers/sketchController.ts`
- Modify: `src/views/business/arcgis/index.vue`

**Steps:**
1. 将 `registerDrawTools()` 拆成 Sketch controller。
2. 节点和区域绘图态统一关闭默认 `updateOnGraphicClick`。
3. 点击节点或区域后显式调用 `sketchVM.update([graphic])`。
4. `SketchViewModel` 事件里不再直接散写页面状态。
5. 管线图层不创建 Sketch 编辑链路。
6. 运行 `pnpm typecheck`。

**Expected Result:**
节点和区域编辑模型一致，管线详情模型独立。

### Task 8: 页面瘦身与旧字段清理（进行中：旧字段清理已完成，页面瘦身待继续）

**Files:**
- Modify: `src/views/business/arcgis/index.vue`
- Modify: `src/views/business/arcgis/maphook.ts`

**Steps:**
1. 清理 `info.attributes`、`info.checkedCount`、`info.checkedItems` 的旧写法。
2. 保留页面模板、初始化调用、refresh 入口，其余业务逻辑全部迁移到 controllers。
3. 删除废弃注释和过时兼容分支。
4. 手动检查节点、区域、线的刷新入口是否仍然正确。
5. 运行 `pnpm typecheck`。

**Expected Result:**
`index.vue` 只负责 UI 和控制器装配，`maphook.ts` 只负责地图基础设施。

## 测试与验证清单

虽然仓库没有测试框架，仍需完成以下手动验证：

### 管线图层

1. 浏览态点击管线，确认详情 popup 正常打开。
2. 管线详情操作后，确认只刷新管线相关展示，不影响节点/区域编辑状态。
3. 管线图层不显示编辑态选中框，不参与修改/删除按钮。

### 普通节点图层

4. 浏览态点击普通节点，确认 popup 正常打开。
5. 绘图态点击普通节点，确认进入编辑选中框。
6. 绘图态点击重叠普通节点，确认命中规则稳定。
7. 拖动普通节点后保存成功，确认刷新点与线。
8. 普通节点批量排序、删除，确认只依赖多选集合。

### 公司节点图层

9. 浏览态点击公司节点，确认行为符合当前业务要求。
10. 绘图态点击公司节点，确认进入编辑选中框。
11. 拖动公司节点后保存成功，确认只刷新公司节点数据。
12. 公司节点修改、删除链路确认继续使用公司节点专用接口与弹窗。

### 区域图层

13. 绘图态点击区域面，确认进入编辑。
14. 绘图态点击区域标签，确认进入同一 polygon 编辑态。
15. 绘图态点击空白或非当前类型要素，确认 selection state 被清理。
16. 拖动区域后保存成功，确认标签样式未丢失。

### 通用状态验证

17. 退出绘图模式后，确认 Sketch、selection state、按钮状态全部清空。
18. 浏览态详情和绘图态编辑选中的对象一致。
19. 节点和区域均不再混用默认 `updateOnGraphicClick` 和手动编辑链路。

## 风险点

- ArcGIS `SketchViewModel` 默认行为较多，切到手动接管后要重点回归节点拖拽和多选。
- `info` 是当前页面很多逻辑的公共容器，迁移时要避免一次性删除太多兼容字段。
- 公司节点与普通节点接口结构不同，节点交互层合并时不能把业务 DTO 和刷新边界也硬并。
- 管线图层虽然不编辑，但仍有详情刷新需求，不能简单删除其现有交互入口。
- 当前没有自动化测试，建议每完成一个阶段就做一次手动回归。

## 建议提交节奏

建议每个阶段单独提交：

1. `refactor(arcgis): add interaction capability registry`
2. `refactor(arcgis): extract hit test controllers`
3. `refactor(arcgis): centralize selection state`
4. `refactor(arcgis): split node interaction and crud controllers`
5. `refactor(arcgis): extract area interaction controller`
6. `refactor(arcgis): isolate line detail interaction`
7. `refactor(arcgis): centralize sketch lifecycle`
8. `refactor(arcgis): trim page-level interaction logic`

## 完成标准

满足以下条件才算完成：

- 节点、区域、管线都只保留一套明确的交互能力定义。
- 节点和区域只有一套命中解析规则。
- 浏览态详情与绘图态编辑选中的对象一致。
- 公司节点与普通节点的交互层统一，但业务执行层仍然分流。
- 管线图层完全从编辑体系剥离，只保留详情链路。
- 所有地图编辑按钮都只依赖 selection state。
- `SketchViewModel` 生命周期不再直接散写页面状态。
- `index.vue` 主要剩下模板、初始化、刷新和控制器装配。
- `pnpm typecheck` 通过。
