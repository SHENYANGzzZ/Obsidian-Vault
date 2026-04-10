---
created: 2026-03-30
updated: 2026-04-10
tags: [项目, water-web, GIS, Vue]
aliases:
  - water-web项目笔记
  - water-web
  - 港区水务管理平台
---

# water-web 项目笔记

## 项目概览

**项目名称**：港区水务管理平台  
**技术栈**：Vue 3 + TypeScript + Element Plus + ArcGIS  
**项目类型**：水务行业管理系统前端

## 当前阶段

### GIS 功能增强

1. **管道粗细按管径显示**：基于 `c_SIZE` 字段动态调整线宽
2. **管道流动动画效果**：为有流向的管道添加粒子流动效果
3. **地图上阀门开关控制**：点击阀门节点可直接开关操作

## 笔记分工

- `05-每日/`：每日原始输入入口，先记录当天实际工作内容
- `worklog/`：从每日归档后的项目工作记录，保留项目时间线
- `plans/`：阶段计划、专题方案、跨天或跨周内容
- `weekly-reports/`：提交给公司的周报，不承载内部原始工作记录
- `README.md`：项目入口、规则说明和关键链接

## 目录结构

```text
water-web/
├── README.md
├── plans/                  # 阶段计划、专题方案
├── worklog/                # 从每日归档后的项目工作记录
└── weekly-reports/         # 提交给公司的周报
```

## 记录规则

### 日常记录

- 每天先写到 `05-每日/`
- 每日中的项目工作按项目分块记录
- 项目块标题统一使用 `## [[项目名]]`
- 块内内容自由书写，不强制限定“完成情况 / 问题 / 未完成”等字段

### 项目归档

- 每周整理一次，把每日中的项目区块原样归档到对应项目的 `worklog/`
- 归档时保留原始内容表达，不做摘要改写
- 同项目同日期内容合并，完全重复的文本去重
- 归档完成后，可从每日中删除已归档的项目区块

### 长周期内容

- 放不进某一天的内容，再进入 `plans/`
- `plans/` 只保留阶段计划、专题验证、路线设计等内容
- 不再单独维护 `tasks.md` 或 `dev-logs/`

## 关键文件

- `src/views/business/arcgis/index.vue`：GIS 地图主页面
- `src/views/business/arcgis/maphook.ts`：地图工具函数
- `src/views/business/arcgis/dialog/nodePopupTemp.vue`：节点详情弹窗

## 当前重点

1. 技术验证结果持续沉淀到 `plans/`
2. 每日执行过程统一写入 `05-每日/`
3. 每周按项目归档到 `worklog/`
4. 对公司输出继续维护 `weekly-reports/`

## 相关链接

- [[待开发计划]]
- [[ArcGIS技术验证计划]]
- [[01-项目/water-web/weekly-reports/2026-W14|2026-W14 周报]]
- [[01-项目/water-web/weekly-reports/2026-W15|2026-W15 周报]]

---

**最后更新**：2026-04-10  
**更新者**：Codex
