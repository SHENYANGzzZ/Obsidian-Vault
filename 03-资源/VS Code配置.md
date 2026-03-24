---
created: 2026-03-24
tags: [工具, VS Code, 编辑器, 开发]
category: 开发工具
---

# VS Code 配置指南

> 高效的代码编辑器配置和插件推荐

## 📦 基础配置

### 用户设置
```json
{
  "editor.fontSize": 14,
  "editor.fontFamily": "JetBrains Mono, Fira Code, Consolas, monospace",
  "editor.fontLigatures": true,
  "editor.lineHeight": 24,
  "editor.tabSize": 2,
  "editor.wordWrap": "on",
  "editor.minimap.enabled": false,
  "editor.bracketPairColorization.enabled": true,
  "editor.guides.bracketPairs": true,
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "files.autoSave": "onFocusChange",
  "files.trimTrailingWhitespace": true,
  "files.insertFinalNewline": true,
  "terminal.integrated.fontSize": 14,
  "workbench.colorTheme": "One Dark Pro",
  "workbench.iconTheme": "material-icon-theme"
}
```

### 快捷键配置
```json
[
  {
    "key": "cmd+shift+p",
    "command": "workbench.action.showCommands"
  },
  {
    "key": "cmd+p",
    "command": "workbench.action.quickOpen"
  },
  {
    "key": "cmd+shift+f",
    "command": "workbench.action.findInFiles"
  },
  {
    "key": "cmd+b",
    "command": "workbench.action.toggleSidebarVisibility"
  },
  {
    "key": "cmd+j",
    "command": "workbench.action.togglePanel"
  }
]
```

## 🔌 必装插件

### 代码编辑
| 插件 | 用途 | 说明 |
|------|------|------|
| **Prettier** | 代码格式化 | 统一代码风格 |
| **ESLint** | JavaScript检查 | 代码质量检查 |
| **GitLens** | Git增强 | 查看代码历史 |
| **Error Lens** | 错误提示 | 行内错误显示 |

### 主题和图标
| 插件 | 用途 | 说明 |
|------|------|------|
| **One Dark Pro** | 深色主题 | 护眼舒适 |
| **Material Icon Theme** | 文件图标 | 美观清晰 |
| **GitHub Theme** | 主题 | GitHub风格 |

### 开发效率
| 插件 | 用途 | 说明 |
|------|------|------|
| **Path Intellisense** | 路径补全 | 自动补全路径 |
| **Auto Rename Tag** | 标签重命名 | HTML标签同步 |
| **Bracket Pair Colorizer** | 括号高亮 | 匹配括号颜色 |
| **indent-rainbow** | 缩进高亮 | 可视化缩进 |

### 语言支持
| 插件 | 用途 | 说明 |
|------|------|------|
| **Python** | Python支持 | 语法、调试、智能提示 |
| **JavaScript (ES6)** | JS代码片段 | 快速编写JS |
| **HTML CSS Support** | CSS支持 | HTML中CSS补全 |
| **Markdown All in One** | Markdown | MD增强 |

### Git 相关
| 插件 | 用途 | 说明 |
|------|------|------|
| **GitLens** | Git增强 | 查看代码历史、作者 |
| **Git Graph** | Git图形化 | 可视化分支和提交 |
| **Git History** | Git历史 | 文件修改历史 |

### 其他实用
| 插件 | 用途 | 说明 |
|------|------|------|
| **Live Server** | 本地服务器 | 前端实时预览 |
| **REST Client** | API测试 | 测试HTTP请求 |
| **Docker** | Docker支持 | 容器管理 |
| **Remote SSH** | 远程开发 | SSH连接远程服务器 |

## ⌨️ 常用快捷键

### 编辑操作
| 快捷键 | 功能 |
|--------|------|
| `Cmd + D` | 选中相同内容 |
| `Cmd + Shift + L` | 选中所有相同内容 |
| `Cmd + /` | 注释/取消注释 |
| `Cmd + ]` / `Cmd + [` | 增加/减少缩进 |
| `Option + ↑` / `Option + ↓` | 移动行 |
| `Option + Shift + ↑` / `Option + Shift + ↓` | 复制行 |
| `Cmd + Shift + K` | 删除行 |

### 光标操作
| 快捷键 | 功能 |
|--------|------|
| `Cmd + ←` | 跳到行首 |
| `Cmd + →` | 跳到行尾 |
| `Option + ←` | 按单词移动 |
| `Cmd + ↑` | 跳到文件开头 |
| `Cmd + ↓` | 跳到文件结尾 |
| `Cmd + Shift + \` | 跳到匹配的括号 |

### 选择操作
| 快捷键 | 功能 |
|--------|------|
| `Shift + 方向键` | 扩展选择 |
| `Cmd + Shift + ←` | 选择到行首 |
| `Cmd + Shift + →` | 选择到行尾 |
| `Option + Shift + ←` | 按单词选择 |
| `Cmd + A` | 全选 |

### 搜索和替换
| 快捷键 | 功能 |
|--------|------|
| `Cmd + F` | 查找 |
| `Cmd + H` | 替换 |
| `Cmd + Shift + F` | 全局查找 |
| `Cmd + Shift + H` | 全局替换 |
| `Cmd + G` | 查找下一个 |
| `Cmd + Shift + G` | 查找上一个 |

### 文件操作
| 快捷键 | 功能 |
|--------|------|
| `Cmd + P` | 快速打开文件 |
| `Cmd + Shift + P` | 命令面板 |
| `Cmd + N` | 新建文件 |
| `Cmd + S` | 保存 |
| `Cmd + Shift + S` | 另存为 |
| `Cmd + W` | 关闭当前文件 |
| `Cmd + K, Cmd + W` | 关闭所有文件 |

### 窗口和面板
| 快捷键 | 功能 |
|--------|------|
| `Cmd + B` | 切换侧边栏 |
| `Cmd + J` | 切换面板 |
| `Cmd + \` | 分割编辑器 |
| `Cmd + 1` / `Cmd + 2` / `Cmd + 3` | 切换编辑器组 |
| `Cmd + Option + ←` / `Cmd + Option + →` | 切换编辑器组 |
| `Control + `` ` `` | 切换终端 |

## 🔧 工作区配置

### 项目配置文件
```json
// .vscode/settings.json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "files.exclude": {
    "**/.git": true,
    "**/.DS_Store": true,
    "**/node_modules": true
  }
}
```

### 代码片段
```json
// .vscode/snippets.code-snippets
{
  "React Component": {
    "prefix": "rfc",
    "body": [
      "import React from 'react';",
      "",
      "const ${1:ComponentName} = () => {",
      "  return (",
      "    <div>",
      "      $0",
      "    </div>",
      "  );",
      "};",
      "",
      "export default ${1:ComponentName};"
    ],
    "description": "React functional component"
  },
  "Console Log": {
    "prefix": "clg",
    "body": [
      "console.log('$1:', $1);"
    ],
    "description": "console.log with label"
  }
}
```

## 🎨 主题配置

### 自定义主题颜色
```json
// settings.json
{
  "workbench.colorCustomizations": {
    "editor.background": "#1e1e1e",
    "editor.foreground": "#d4d4d4",
    "editorCursor.foreground": "#aeafad",
    "editor.lineHighlightBackground": "#2a2d2e",
    "editor.selectionBackground": "#264f78",
    "editorLineNumber.foreground": "#858585",
    "editorLineNumber.activeForeground": "#c6c6c6"
  },
  "editor.tokenColorCustomizations": {
    "textMateRules": [
      {
        "scope": "comment",
        "settings": {
          "foreground": "#6a9955",
          "fontStyle": "italic"
        }
      }
    ]
  }
}
```

## 🚀 高级技巧

### 多光标编辑
| 操作 | 快捷键 |
|------|--------|
| 添加光标 | `Option + 点击` |
| 添加光标上方 | `Cmd + Option + ↑` |
| 添加光标下方 | `Cmd + Option + ↓` |
| 选择所有匹配 | `Cmd + Shift + L` |
| 选择下一个匹配 | `Cmd + D` |

### 代码折叠
| 快捷键 | 功能 |
|--------|------|
| `Cmd + Option + [` | 折叠代码块 |
| `Cmd + Option + ]` | 展开代码块 |
| `Cmd + K, Cmd + 0` | 折叠所有 |
| `Cmd + K, Cmd + J` | 展开所有 |

### 终端集成
```bash
# 在VS Code中打开终端
Control + `

# 新建终端
Cmd + Shift + `

# 切换终端
Cmd + Shift + [ / ]

# 终端中运行选中代码
Cmd + Option + Enter
```

## 📱 远程开发

### SSH 远程开发
```json
// .vscode/settings.json
{
  "remote.SSH.remotePlatform": {
    "server-name": "linux"
  }
}
```

### Docker 开发
```json
// .devcontainer/devcontainer.json
{
  "name": "Node.js Dev Container",
  "image": "mcr.microsoft.com/devcontainers/javascript-node:0-18",
  "features": {
    "ghcr.io/devcontainers/features/git:1": {}
  },
  "customizations": {
    "vscode": {
      "extensions": [
        "dbaeumer.vscode-eslint",
        "esbenp.prettier-vscode"
      ]
    }
  }
}
```

## 🔗 相关笔记

### 工具类
- [[Git常用命令]] - 版本控制
- [[iTerm2配置与使用指南]] - 终端工具
- [[开发环境配置]] - 环境搭建

### 开发技巧
- [[代码规范]] - 编码标准
- [[调试技巧]] - 问题排查
- [[性能优化]] - 优化方法

## 📚 学习资源

### 官方文档
- [VS Code官方文档](https://code.visualstudio.com/docs)
- [VS Code快捷键参考](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)

### 插件市场
- [VS Code插件市场](https://marketplace.visualstudio.com/vscode)

---

> 💡 **提示**：根据项目需求安装插件，不要过度安装影响性能。

*最后更新：2026-03-24*
