# 项目 MOC

这里汇总所有项目相关的笔记。

## 🚀 进行中项目
```dataview
TABLE status AS "状态", deadline AS "截止日期"
FROM "01-Projects"
WHERE status = "进行中"
SORT deadline ASC
```

## 📋 计划中的项目
```dataview
LIST
FROM "01-Projects"
WHERE status = "计划中"
```

## ✅ 已完成项目
```dataview
LIST
FROM "04-Archive"
WHERE contains(file.path, "项目")
SORT file.name DESC
```

## 创建新项目
使用模板：[[项目模板]]

## 项目清单
- [ ] 检查项目是否符合 SMART 原则
- [ ] 设定明确的截止日期
- [ ] 定义关键结果 (OKR)
