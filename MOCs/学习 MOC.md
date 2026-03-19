---
created: 2026-03-06
tags: #moc #学习
---

# 📚 学习 MOC

> 学习 MOC 汇总所有学习相关的笔记和资源。

## 🎯 核心理念

- [[Zettelkasten方法]] - 学习的底层方法
- [[卡片笔记写作法]] - 实践指南
- [[原子笔记的价值]] - 什么是好的笔记

## 📖 正在学习

```dataview
TABLE file.mtime AS "更新时间"
FROM ""
WHERE contains(file.tags, "#学习") AND contains(file.tags, "#status/进行中")
SORT file.mtime DESC
```

### Obsidian 相关
- [[GitHub优秀Obsidian案例学习]]
- [[Obsidian插件推荐]]
- [[快速开始指南]]

### 知识管理
- [[PARA方法介绍]]
- [[收集箱处理工作流]]
- [[标签管理规范]]

## 📚 学习资源

### 必读
- [[How to Take Smart Notes]] - 卡片笔记写作法原书
- [[Zettelkasten方法]] - 核心方法论

### 推荐
- [[MOC的作用]]
- [[笔记链接的艺术]]
- [[如何建立笔记链接网络]]

## 📝 学习项目

> 注：`学习Obsidian最佳实践` 项目已归档至 [[04-归档]]

## ✅ 已完成

```dataview
TABLE file.ctime AS "完成时间"
FROM ""
WHERE contains(file.tags, "#学习") AND contains(file.tags, "#status/已完成")
```

## 🔄 学习工作流

```
捕获想法 → Inbox → 处理 → 永久笔记 → 建立链接 → MOC
```

详见：[[收集箱处理工作流]]

## 📊 学习统计

```dataview
TABLE COUNT(file) AS "笔记数"
FROM ""
WHERE contains(file.tags, "#学习")
GROUP BY file.day
```

---

*最后更新：2026-03-19*
