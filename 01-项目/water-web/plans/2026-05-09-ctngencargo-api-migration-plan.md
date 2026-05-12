---
created: 2026-05-09
updated: 2026-05-09
tags: [water-web, ctngencargo, 接口替换, 路由迁移, 计划]
project: water-web
---

# ctngencargo 路由迁移与接口替换计划

## 背景

本轮已将 `src/views/ctngencargo` 从“单路由 + `query.page` 内嵌组件”的方式，逐步调整为“独立路由跳转到对应组件”的方式：

- 件杂货：改为独立路由 `/ctngencargo/bb_cust_edit`
- 集装箱：改为独立路由 `/ctngencargo/baseinfo`、`/ctngencargo/baseinfoedit`
- 旧的内嵌组件引用已从 `cust_query.vue` 中清理

当前还需要继续梳理 `ctngencargo` 下实际依赖的后端接口，并逐步从旧接口切到新的 `CTN_GEN_CARGO` 控制器。

## 已确认的新接口

### 1. `api/CTN_GEN_CARGO/BbCustInfo`

- `POST /api/CTN_GEN_CARGO/BbCustInfo`：新增件杂货客户信息
- `PUT /api/CTN_GEN_CARGO/BbCustInfo`：更新件杂货客户信息
- `DELETE /api/CTN_GEN_CARGO/BbCustInfo?custId=...`：删除件杂货客户信息
- `GET /api/CTN_GEN_CARGO/BbCustInfo/Cust?custId=...`：根据客户 ID 获取客户信息
- `GET /api/CTN_GEN_CARGO/BbCustInfo/Unit`：获取当前登录用户办事处（件杂货）

### 2. `api/CTN_GEN_CARGO/CmmCustAcv`

- `GET /api/CTN_GEN_CARGO/CmmCustAcv/Acv?id_acv=...&if_archived=...`：查询客户档案信息
- `GET /api/CTN_GEN_CARGO/CmmCustAcv/Cust?id_cust=...`：根据客户 ID 查询客户信息
- `GET /api/CTN_GEN_CARGO/CmmCustAcv/ByCust?ID_CUST=...`：根据客户 ID 查询业务记录
- `POST /api/CTN_GEN_CARGO/CmmCustAcv?id_cust=...&if_acv=...`：新增客户业务档案
- `PUT /api/CTN_GEN_CARGO/CmmCustAcv?id_acv=...&if_acv=...`：修改客户业务档案
- `DELETE /api/CTN_GEN_CARGO/CmmCustAcv?id_acv=...`：删除客户业务档案

### 3. `api/CTN_GEN_CARGO/CmmCustArchives`

- `GET /api/CTN_GEN_CARGO/CmmCustArchives/Page?...`：客户档案分页查询
- `GET /api/CTN_GEN_CARGO/CmmCustArchives/Back?id_customer=...&id_record=...&type=...&flag=...&reason=...`：领导退回 / 审核通过客户信息或客户服务记录

## 当前 `src/views/ctngencargo` 实际涉及接口

### 列表页 `cust_table.vue`

- `GET API/CTN_GEN_CARGO/CUSTOMER/PAGE`
- `DELETE api/client/Cust_Acv`
- `DELETE api/CTN_GEN_CARGO/BbCustInfo`

### 件杂货 `breakbulk/bb_cust_edit.vue`

- `api/bb_client/bb_cust_info/unit`
- `API/BB_CLIENT/bb_cust_info/cust`
- `api/CTN_GEN_CARGO/BbCustInfo`
- 以及字典、地区等公共接口

### 集装箱 `container/base_info.vue`

- `api/client/Cust_Config`
- `api/client/CMM_CUST_ARCHIVES_QUERY/BYNAME`
- `api/client/CMM_CUST_ARCHIVES_QUERY/ALL`
- `api/client/Cust_Acv`
- 以及组织、字典、地区等公共接口

### 集装箱 `container/base_info_edit.vue`

- `api/client/Cust_Acv/cust`
- `api/client/CMM_CUST_ARCHIVES_QUERY/ALL`
- `api/client/CMM_CUST_ARCHIVES_QUERY`
- `api/client/Cust_Acv`
- 以及组织、字典、地区等公共接口

## 可直接替换的接口

### 件杂货

1. `api/bb_client/bb_cust_info/unit`
   替换为：`GET /api/CTN_GEN_CARGO/BbCustInfo/Unit`

2. `API/BB_CLIENT/bb_cust_info/cust`
   替换为：`GET /api/CTN_GEN_CARGO/BbCustInfo/Cust?custId=...`

3. 提交接口已基本切换到：`api/CTN_GEN_CARGO/BbCustInfo`

### 集装箱

4. `POST api/client/Cust_Acv`
   替换为：`POST /api/CTN_GEN_CARGO/CmmCustAcv?id_cust=...&if_acv=...`

5. `GET api/client/Cust_Acv/cust?id_cust=...`
   替换为：`GET /api/CTN_GEN_CARGO/CmmCustAcv/Cust?id_cust=...`

6. `PUT api/client/Cust_Acv`
   替换为：`PUT /api/CTN_GEN_CARGO/CmmCustAcv?id_acv=...&if_acv=...`

## 暂不直接替换的接口

### 1. 列表分页

当前页面用的是：`GET API/CTN_GEN_CARGO/CUSTOMER/PAGE`

虽然新接口里有 `GET /api/CTN_GEN_CARGO/CmmCustArchives/Page`，但现页面还传了 `BUSI_TYPE`，而已知截图里的 `Page` 参数中没有看到 `BUSI_TYPE`。这意味着当前 `CUSTOMER/PAGE` 可能是“集装箱 + 件杂货统一分页”的聚合接口，不能直接替换，需先确认后端参数模型。

### 2. 集装箱删除

当前页面用的是：`DELETE api/client/Cust_Acv`，现代码按 `id_cust` 删除。

而新接口是：`DELETE /api/CTN_GEN_CARGO/CmmCustAcv?id_acv=...`，参数语义变了，不能只改 URL，必须连前端删除逻辑一起改成按 `id_acv` 删除。

### 3. 业务配置 / 母公司搜索 / 字典类接口

这批接口在当前截图提供的新控制器中没有直接替代项，暂时保留：

- `api/client/Cust_Config`
- `api/client/CMM_CUST_ARCHIVES_QUERY/BYNAME`
- `api/client/CMM_CUST_ARCHIVES_QUERY/ALL`
- `api/client/CMM_CUST_ARCHIVES_QUERY`
- `api/MPA/ORG_T_UNIT`
- `api/MPA/SYS_T_USER/ALL`
- 字典、地区接口

## 建议的后续顺序

1. 先替换件杂货详情与办事处接口
2. 再替换集装箱新增 / 详情 / 修改接口
3. 单独核对 `CUSTOMER/PAGE` 与 `CmmCustArchives/Page` 的参数兼容性
4. 单独改造集装箱删除逻辑，确认是按 `id_acv` 还是 `id_cust` 删除
5. 后续如增加审核按钮，直接接 `CmmCustArchives/Back`

## 今日已完成的前置改造

- `ctngencargo` 从内嵌组件切换为独立路由
- 件杂货、集装箱新增/编辑组件已迁到新路由
- 旧的 `query.page` 组件引用已清理
- 权限判断已统一为 `canQuery / canMutate / canReview`
- `BUSI_TYPE` 已支持缓存与路由双向同步
