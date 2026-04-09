---
created: 2026-03-06
updated: 2026-04-09
tags: #moc #项目
---

# 项目 MOC

> 这个页面只保留当前仍然有效的项目入口和项目工作流说明。

## 进行中项目

```dataview
TABLE status AS "状态"
FROM "01-项目"
WHERE contains(file.tags, "#项目") OR status = "进行中"
SORT file.mtime DESC
```

## 已归档项目

```dataview
TABLE file.mtime AS "更新时间"
FROM "04-归档"
WHERE contains(file.tags, "#项目")
SORT file.mtime DESC
```

## 当前项目工作流

### 1. 建立项目目录

在 `01-项目/项目名/` 下维护：

- `README.md`
- `plans/`
- `worklog/`
- `weekly-reports/`

### 2. 日常执行

- 每天先记录到 `05-每日/`
- 项目工作按 `## [[项目名]]` 分块记录
- 每周再归档到对应项目的 `worklog/`

### 3. 长周期与对外输出

- `plans/`：阶段计划、专题方案、长期安排
- `weekly-reports/`：提交给公司的周报

## 当前重点项目

- [[01-项目/water-web/README|water-web 项目笔记]]
- [[本地仓库索引]]

## 项目模板

- [[项目模板]]

## 检查清单

创建新项目时：

- [ ] 创建项目目录
- [ ] 创建 README
- [ ] 明确当前阶段目标
- [ ] 确认是否需要 `plans/`
- [ ] 添加到本页

项目归档时：

- [ ] 确认项目已停止活跃维护
- [ ] 保留 README、plans、worklog、weekly-reports 中仍有价值的内容
- [ ] 移动到 `04-归档/`

## 相关链接

- [[PARA方法介绍]]
- [[知识管理 MOC]]
- [[领域 MOC]]

---

_最后更新：2026-04-09_
