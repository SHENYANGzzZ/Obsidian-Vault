---
created: 2026-03-20
tags: #技术笔记 #JavaScript #water-web
---

# JavaScript 浮点精度

**项目**：[[water-web]]
**标签**：#技术笔记 #JavaScript #water-web

---

## 概念

JavaScript 使用 IEEE 754 双精度浮点数，精度上限约 **16 位有效数字**。

| 范围         | 值                          |
| ------------ | --------------------------- |
| 最大安全整数 | 9007199254740991 (2^53 - 1) |
| 有效数字     | 约 16 位                    |

---

## 常见场景

### 长小数精度丢失

```javascript
// 运行时实际值
295828763.79585470937713011037;
// → 295828763.7958547（截断到 16 位）
```

### 警告信息

```
This number literal will lose precision at runtime.
```

---

## 解决方案

### 方案 1：截断到 16 位（推荐）

```javascript
// 适用于瓦片地图等场景，精度足够
scale: 295828763.7958547;
```

### 方案 2：使用 BigInt

```javascript
// 适用于大整数场景
const bigNum = 99999999999999999n;
```

### 方案 3：使用字符串

```javascript
// 适用于需要精确存储的场景
const num = "295828763.79585470937713011037";
```

---

## 使用场景

- [[ArcGIS TileInfo 配置]] 中的 scale 值
- 金融计算中的金额处理
- 科学计算中的高精度需求

---

## 参考资料

- [Number.MAX_SAFE_INTEGER - MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER)
- [IEEE 754 浮点数标准](https://en.wikipedia.org/wiki/IEEE_754)

---

## 相关笔记

- [[坐标转换]]
- [[ArcGIS TileInfo 配置]]
- [[water-web 项目笔记]]
