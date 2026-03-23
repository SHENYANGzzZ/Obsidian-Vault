---
created: 2026-03-20
tags: #技术笔记 #ArcGIS #water-web
---

# ArcGIS TileInfo 配置

**项目**：[[water-web]]
**标签**：#技术笔记 #ArcGIS #water-web

---

## 概念

TileInfo 用于定义瓦片金字塔的层级参数，是 ArcGIS 地图服务的核心配置。

---

## 核心属性

| 属性             | 说明                        | 示例值              |
| ---------------- | --------------------------- | ------------------- |
| dpi              | 屏幕分辨率                  | 90.71428571427429   |
| lods             | 层级细节（Level of Detail） | 见下方              |
| size             | 单张瓦片像素尺寸            | [256, 256]          |
| origin           | 瓦片切片原点坐标            | { x: -180, y: 90 }  |
| spatialReference | 坐标系                      | WGS84 / WebMercator |

---

## lods 配置

每个层级包含：

```javascript
{
  level: 0,                    // 层级编号
  levelValue: "1",             // 层级值
  scale: 295828763.7958547,    // 比例尺（⚠️ 注意精度）
  resolution: 0.703125         // 分辨率
}
```

---

## ⚠️ 常见问题

### 精度丢失警告

**问题**：`This number literal will lose precision at runtime`

**原因**：scale 值小数位过多（如 30 位），超出 JS 浮点精度（约 16 位）

**解决**：将 scale 截断到 16 位有效数字

```javascript
// ❌ 丢失精度
scale: 295828763.79585470937713011037;

// ✅ 保留 16 位
scale: 295828763.7958547;
```

> 详见：[[JavaScript 浮点精度]]

---

## 配置示例

```javascript
tileInfo = new arcgisModules.TileInfo({
  dpi: 90.71428571427429,
  lods: [
    {
      level: 0,
      levelValue: "1",
      scale: 295828763.7958547,
      resolution: 0.703125,
    },
    {
      level: 1,
      levelValue: "2",
      scale: 147914381.89792735,
      resolution: 0.3515625,
    },
    // ... 更多层级
  ],
  size: [256, 256],
  origin: { x: -180, y: 90 },
  spatialReference,
});
```

---

## 相关笔记

- [[坐标转换]]
- [[JavaScript 浮点精度]]
- [[water-web 项目笔记]]
