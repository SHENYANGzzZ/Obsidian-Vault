---
created: 2026-03-30
updated: 2026-03-30
tags: #资源/工具 #obsidian #codex #skill
---

# 本地 Skills 索引

> 这篇笔记整理本机已安装的 Codex/Agent skills，方便按用途查找，而不是每次都在终端里盲翻目录。

## 概览

- Skills 根目录：`/Users/shenyang/.agents/skills`
- 当前已识别技能数：49
- 主要用途：编码、研究、Obsidian、飞书、内容生产、自动化

## 命名规律

- 当前本地 skills 已切换为中文目录名，并保留英文兼容软链接
- 当前格式：`中文名称` 或 `中文名称-版本号`
- 例如：`编码工作流-1.0.4`、`Obsidian本体同步-1.0.1`、`飞书文档-1.2.7`
- 说明：为了兼顾可读性和兼容性，现采用“中文目录 + 英文软链接”方案

## 按用途整理

### 编码开发

- `code-1.0.4`：通用编码工作流
- `debug-pro-1.0.0`：问题排查与调试
- `executing-plans-0.1.0`：执行已有实施计划
- `writing-plans-0.1.0`：先写计划再开工
- `frontend-design-3-0.1.0`：前端界面设计与实现
- `ui-ux-pro-max-0.1.0`：UI/UX 设计与优化
- `git-essentials-1.0.0`：Git 工作流
- `tmux-1.0.0`：tmux 会话控制
- `test-runner-1.0.0`：测试执行与结果整理
- `opencode-controller-1.0.0`：Opencode 会话与代理控制

### 架构与工程质量

- `architecture-designer-0.1.0`：架构设计与评审
- `security-auditor-1.0.0`：安全审计
- `supabase-postgres-best-practices`：Postgres 最佳实践
- `clawdefender-1`：防注入与安全校验
- `skill-vetter-1.0.0`：第三方 skill 安全审查
- `self-improving-1.1.3`：代理自我改进
- `agent-self-reflection-1.0.0`：定期复盘

### Obsidian 与知识管理

- `obsidian-ontology-sync-1.0.1`：Obsidian 与本体/结构化知识同步
- `memory-1.0.2`：长期记忆管理
- `session-logs-1.0.0`：会话日志检索
- `find-skills`：查找适合的新 skill

### 飞书协作

- `feishu-doc-1.2.7`：读取飞书文档/表格/wiki
- `feishu-chat-history`：读取群聊历史
- `feishu-send-file`：发送文件到飞书
- `feishu-screenshot`：截图并发送飞书
- `feishu-cron-reminder`：飞书定时提醒
- `feishu-perm`：飞书权限管理
- `feishu-drive-1.0.0`：飞书云盘相关能力

### 研究与信息获取

- `autoglm-websearch`：联网搜索
- `autoglm-deepresearch`：深度研究
- `autoglm-open-link`：打开网页并提取正文
- `autoglm-search-image`：搜图
- `autoglm-browser-agent`：浏览器自动化
- `aminer-open-academic-1.0.5`：学术数据检索
- `market-research-1.0.0`：市场研究
- `research-paper-writer-0.1.0`：学术论文写作
- `interview-designer-1.0.0`：面试设计
- `backtest-expert-0.1.0`：量化回测
- `a-stock-analysis-1.0.0`：A 股分析

### 内容与增长

- `blog-writer-0.1.0`：博客写作
- `copywriting-0.1.0`：营销文案
- `content-strategy-0.1.0`：内容策略
- `seo-1.0.3`：SEO 审计与策略
- `seo-content-writer-2.0.0`：SEO 内容写作
- `social-content-generator-0.1.0`：社媒内容生成
- `social-media-scheduler-1.0.0`：社媒排期

### 自动化与工具

- `automation-workflows-0.1.0`：自动化工作流设计
- `1password-1.0.1`：1Password CLI 使用
- `ffmpeg-video-editor-1.0.0`：FFmpeg 视频处理
- `video-frames-1.0.0`：视频抽帧
- `autoglm-generate-image`：文生图
- `brainstorming-0.1.0`：需求与方案发散
- `skill-creator-0.1.0`：创建/设计新 skill

## 当前使用建议

### 高频常用组合

- 编码任务：`code` + `git-essentials` + `debug-pro`
- 需求梳理：`brainstorming` + `writing-plans` + `executing-plans`
- Obsidian 整理：`obsidian-ontology-sync` + `memory` + `session-logs`
- 飞书协作：`feishu-doc` + `feishu-chat-history` + `feishu-send-file`
- 深度研究：`autoglm-websearch` + `autoglm-deepresearch` + `autoglm-open-link`

## 待补充

- [ ] 为高频 skill 增加个人使用场景笔记
- [ ] 记录每个 skill 的最佳触发词
- [ ] 建立“skill -> 常见任务”对照表

## 相关笔记

- [[资源 MOC]]
- [[本地仓库索引]]
- [[开发环境配置]]
- [[我对Obsidian的实践体会]]

---

*最后更新：2026-03-30*
