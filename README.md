# Obsidian 知识库

## 项目简介
这是一个基于 Obsidian 的个人知识库，采用 Zettelkasten 方法进行知识管理。通过结构化的组织和双向链接，构建个人知识网络。

## 目录结构

```
Obsidian-Vault/
├── 00-Inbox/           # 收件箱（临时存放）
├── 01-Projects/        # 项目（有明确截止日期）
├── 02-Areas/           # 领域（需要持续关注）
├── 03-Resources/       # 资源（参考材料）
├── 05-Daily/           # 日记（每日记录）
├── 06-Templates/       # 笔记模板
├── MOCs/               # 知识地图（索引）
└── .obsidian/          # Obsidian 配置文件
```

## 核心功能

### 知识管理方法
- **Zettelkasten 卡片盒方法**：原子化笔记，建立连接网络
- **MOC (Maps of Content)**：知识索引和导航
- **双向链接**：自动发现笔记间的关联
- **标签系统**：多维度分类笔记

### 笔记模板
- Zettelkasten 笔记模板
- 读书笔记模板
- 会议笔记模板
- 项目模板
- 每日日记模板

## 工作流程

1. **收集**：将想法收集到 Inbox
2. **处理**：将临时想法转化为原子笔记
3. **连接**：建立笔记间的链接关系
4. **回顾**：定期整理和回顾

## 技术栈

- **Obsidian**：知识管理工具
- **Git**：版本控制和同步
- **GitHub**：远程存储

## 同步方法

1. **Git 命令**：
   - 拉取：`git pull origin main`
   - 提交：`git add . && git commit -m "更新内容"`
   - 推送：`git push origin main`

2. **Obsidian Git 插件**：
   - 安装并配置自动同步
   - 设置拉取和推送间隔

## 核心笔记

- [[原子笔记的价值]]
- [[链接比文件夹更重要]]
- [[Zettelkasten方法]]
- [[MOC的作用]]

## 标签系统

- `#永久笔记` - Zettelkasten 笔记
- `#闪念笔记` - 临时想法
- `#项目` - 项目相关
- `#学习` - 学习内容

## 开始使用

1. 克隆仓库：`git clone https://github.com/SHENYANGzzZ/Obsidian-Vault.git`
2. 在 Obsidian 中打开该文件夹
3. 安装推荐的插件
4. 开始构建你的知识网络

## 插件推荐

- Obsidian Git：版本控制和同步
- Dataview：数据查询和可视化
- Templater：模板管理
- Calendar：日历视图

## 贡献

欢迎提交 Issue 和 Pull Request 来改进这个知识库！

## 许可证

MIT License

---

*最后更新：2026-03-10*