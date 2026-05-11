---
created: 2026-05-11
updated: 2026-05-11
tags: [CtnmarketWeb, ctngencargo, 重构, 计划, 高内聚低耦合]
project: CtnmarketWeb
status: planned
---

# ctngencargo 代码结构重构计划

## 背景

`src/views/ctngencargo` 当前已经完成一轮功能性整理，包括：

- 集装箱新增页文件更名为 `base_info_add.vue`
- 集装箱查看模式按路由 `mode=view` 禁用表单并隐藏保存按钮
- 集装箱 / 件杂货删除确认逻辑已排除取消和关闭确认框的误报
- `ctngencargo/api/request.js` 已开始集中封装 CTN_GEN_CARGO 相关接口

下一步在做结构性重构前，先提交当前功能修复版本，作为可回退基线。重构必须分阶段小步提交，避免一次性改动过大导致业务行为不可控。

## 当前代码状态判断

`src/views/ctngencargo` 当前主要文件规模如下：

- `cust_table.vue`：客户查询列表，约 900 行，承担筛选、表格、权限、路由、删除、接口调用、字段兼容等多种职责
- `container/base_info_add.vue`：集装箱基础资料新增页，约 700 行
- `container/base_info_edit.vue`：集装箱基础资料编辑 / 查看页，约 600 行
- `breakbulk/bb_cust_edit.vue`：件杂货客户新增 / 编辑 / 查看页，约 680 行
- `api/request.js`：当前接口封装入口，约 220 行

主要问题：页面组件承担过多业务细节，表单、校验、字典加载、路由跳转、错误处理、接口适配耦合在同一个 Vue 文件里。

## 重构原则

- 保持现有业务行为不变，先拆结构，再优化行为
- 页面组件只负责页面编排，不直接承载大量业务细节
- 纯函数优先抽到 `shared` / `utils`，降低组件间重复
- 接口按业务域拆分，页面只依赖语义化 API 方法
- 每个阶段都要能单独构建通过，并可单独提交

## 阶段 0：提交当前功能修复基线

目标：在结构重构前先提交一版当前可运行代码。

建议提交前检查：

- [ ] `git status --short` 确认当前变更范围
- [ ] `yarn build` 确认编译通过
- [ ] 手工确认集装箱查看页：无保存按钮，表单不可编辑，仅保留返回
- [ ] 手工确认删除确认框：取消 / 关闭不提示删除失败
- [ ] 提交当前功能修复版本

建议提交说明：

```text
fix: 优化 ctngencargo 客户维护查看与删除交互
```

## 阶段 1：抽公共常量、错误处理和确认框工具

目标：先抽低风险公共逻辑，不改页面结构。

建议新增：

```text
src/views/ctngencargo/constants.js
src/views/ctngencargo/utils/error.js
src/views/ctngencargo/utils/confirm.js
```

职责：

- `constants.js`：统一维护 `BUSI_TYPE`、本地缓存 key、通用路由 path
- `utils/error.js`：统一 `getErrorMessage(error)`
- `utils/confirm.js`：统一 `isConfirmCancel(error)`、删除确认包装

验收标准：

- [ ] 删除确认取消 / 关闭行为不变
- [ ] 所有删除失败提示仍能显示真实接口错误
- [ ] `yarn build` 通过

## 阶段 2：拆 `cust_table.vue` 的列表配置与权限逻辑

目标：降低列表页复杂度，让列表页只做容器编排。

建议新增：

```text
src/views/ctngencargo/customer-list/columns.js
src/views/ctngencargo/customer-list/permissions.js
src/views/ctngencargo/customer-list/query-model.js
src/views/ctngencargo/customer-list/route-actions.js
```

拆分内容：

- `columns.js`：CMM / BB 表格列配置
- `permissions.js`：`canQuery`、`canMutate`、`canReview`
- `query-model.js`：默认日期、地区参数、查询参数构造
- `route-actions.js`：查看、新增、编辑、服务记录跳转

验收标准：

- [ ] `cust_table.vue` 行数明显下降
- [ ] 列表筛选、分页、查看、新增、编辑、删除行为不变
- [ ] `BUSI_TYPE=CMM` 与 `BUSI_TYPE=BB` 均可正常查询
- [ ] `yarn build` 通过

## 阶段 3：抽集装箱基础资料公共模型与校验

目标：减少 `base_info_add.vue` 与 `base_info_edit.vue` 的重复。

建议新增：

```text
src/views/ctngencargo/container/shared/models.js
src/views/ctngencargo/container/shared/validators.js
src/views/ctngencargo/container/shared/mapper.js
src/views/ctngencargo/container/shared/dicts.js
```

拆分内容：

- `models.js`：`createCustomerInfo`、`createSaleManInfo`、`createLinkman`
- `validators.js`：必填校验、简介长度校验、联系人固话校验
- `mapper.js`：省市区格式转换、提交 payload 构造
- `dicts.js`：企业性质、企业类别、行业、开发状态、省市区、组织、用户加载

验收标准：

- [ ] 新增页与编辑页继续共用原接口
- [ ] 新增、暂存、补充业务信息、编辑保存行为不变
- [ ] 查看模式仍只读
- [ ] `yarn build` 通过

## 阶段 4：组件化集装箱基础资料表单

目标：让新增页、编辑页变薄，只保留模式和提交行为。

建议新增：

```text
src/views/ctngencargo/container/components/ContainerBaseForm.vue
src/views/ctngencargo/container/components/ContainerLinkmanTable.vue
src/views/ctngencargo/container/components/ContainerActionBar.vue
```

组件职责：

- `ContainerBaseForm.vue`：基础资料字段展示和编辑
- `ContainerLinkmanTable.vue`：联系人列表维护
- `ContainerActionBar.vue`：保存、补充业务信息、返回等按钮区

验收标准：

- [ ] `base_info_add.vue` / `base_info_edit.vue` 只负责页面模式、加载、保存
- [ ] 表单禁用逻辑统一由 `disabled` prop 控制
- [ ] `yarn build` 通过

## 阶段 5：拆 API 层按业务域组织

目标：让接口封装高内聚，避免 `api/request.js` 继续膨胀。

建议结构：

```text
src/views/ctngencargo/api/index.js
src/views/ctngencargo/api/customer.js
src/views/ctngencargo/api/container.js
src/views/ctngencargo/api/breakbulk.js
src/views/ctngencargo/api/dict.js
```

拆分内容：

- `customer.js`：客户列表、自动补全、删除
- `container.js`：集装箱基础资料新增、编辑、详情
- `breakbulk.js`：件杂货客户新增、编辑、详情、删除
- `dict.js`：字典、省市区、组织、用户

验收标准：

- [ ] 页面 import 来源清晰
- [ ] 接口 URL 没有行为变更
- [ ] `yarn build` 通过

## 风险点

- `cust_table.vue` 同时支持 CMM / BB，字段大小写兼容不能贸然删除
- 集装箱删除接口语义需要继续确认是按 `id_cust` 还是 `id_acv`
- `base_info_add.vue` 和 `base_info_edit.vue` 虽重复较多，但新增页有默认配置、重复客户检测和跳业务信息逻辑，不能简单合并
- 件杂货和集装箱虽然同属客户维护，但表单字段和提交 payload 不一致，不建议强行合成一个大表单

## 推荐提交切分

1. `chore: add ctngencargo shared constants and error helpers`
2. `refactor: extract ctngencargo customer list config`
3. `refactor: extract container customer shared models and validators`
4. `refactor: split container base info form components`
5. `refactor: split ctngencargo api modules`

每个提交只做一个方向，提交前都运行 `yarn build`。
