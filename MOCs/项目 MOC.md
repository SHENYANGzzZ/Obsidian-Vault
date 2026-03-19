---
created: 2026-03-06
tags: #moc #项目
---

# 🚀 项目 MOC

> 项目 MOC 汇总所有项目相关的笔记和资源。

## 什么是项目？

项目是有明确目标和截止日期的一次性工作：
- 有明确的交付成果
- 有明确的完成时间
- 可以分解为具体任务

详见：[[PARA方法介绍]]

## 🚀 进行中项目

```dataview
TABLE status AS "状态", deadline AS "截止日期"
FROM "01-项目"
WHERE status = "进行中" OR contains(file.tags, "#status/进行中")
SORT deadline ASC
```

## 📋 计划中的项目

```dataview
TABLE status AS "状态"
FROM "01-项目"
WHERE status = "计划中" OR contains(file.tags, "#status/待办")
SORT file.ctime DESC
```

## ✅ 已完成项目

```dataview
TABLE file.ctime AS "完成时间"
FROM "04-归档"
WHERE contains(file.tags, "#项目")
SORT file.ctime DESC
```

## 🔄 项目生命周期

```
计划 → 执行 → 完成 → 归档
  ↓       ↓       ↓
调整   监控    复盘
```

### 1. 计划阶段
- 明确项目目标
- 设定截止日期
- 分解关键任务
- 创建项目笔记

### 2. 执行阶段
- 记录开发日志
- 更新任务进度
- 捕获遇到的问题
- 建立笔记链接

### 3. 完成阶段
- 确认交付成果
- 总结经验教训
- 更新项目状态

### 4. 归档阶段
- 移动到 04-归档
- 更新 MOC
- 保留关键文档

## 📝 项目模板

使用 [[项目模板]] 创建新项目

## 📊 项目统计

```dataview
TABLE COUNT(file) AS "项目数"
FROM "01-项目"
GROUP BY status
```

## ✅ 检查清单

创建新项目时：
- [ ] 设定明确的目标
- [ ] 确定截止日期
- [ ] 分解关键任务
- [ ] 创建项目笔记
- [ ] 添加到 [[项目 MOC]]

项目完成时：
- [ ] 确认所有任务完成
- [ ] 记录经验教训
- [ ] 移动到归档
- [ ] 更新 [[项目 MOC]]

---

## 相关链接

- [[PARA方法介绍]]
- [[知识管理 MOC]]
- [[领域 MOC]]

---

*最后更新：2026-03-19*
