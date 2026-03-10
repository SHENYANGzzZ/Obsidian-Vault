---
created: 2026-03-06
tags: #教程 #git #同步
---

# 使用 Git 同步 Obsidian Vault

## 为什么使用 Git
- ✅ 完全免费
- ✅ 版本控制
- ✅ 跨设备同步
- ✅ 数据安全

## 设置步骤

### 1. 创建 GitHub 仓库
```bash
# 在 GitHub 上创建私有仓库
# 仓库名: obsidian-vault
```

### 2. 初始化本地仓库
```bash
cd /path/to/your/vault
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/username/obsidian-vault.git
git push -u origin main
```

### 3. 安装 Obsidian Git 插件
- 在 Obsidian 设置中搜索 "Obsidian Git"
- 安装并启用

### 4. 配置自动同步
- 设置自动拉取间隔: 10 分钟
- 设置自动推送间隔: 10 分钟
- 启用启动时自动拉取

## 日常使用

### 手动同步
- `Ctrl/Cmd + P` 打开命令面板
- 输入 "Git: Commit all changes"
- 输入 "Git: Push"

### 自动同步
插件会自动在后台同步，无需手动操作。

## 注意事项
- ⚠️ 确保 .obsidian 文件夹被包含
- ⚠️ 添加 .gitignore 排除缓存文件
- ⚠️ 多设备使用时注意冲突

## .gitignore 示例
```
.DS_Store
.trash/
workspace
workspace.json
```

## 相关笔记
- [[Obsidian插件推荐]]
