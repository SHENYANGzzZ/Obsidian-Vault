# 仓库整理 - 实施计划

**当前时间**：2026-03-31

## [x] 任务 1：创建 07-对话记录 目录结构
- **优先级**：P0
- **Depends On**：None
- **Description**：
  - 创建 `07-对话记录/` 根目录
  - 创建 `07-对话记录/2026-03/` 子目录（按月份组织）
- **Success Criteria**：
  - 目录结构符合 CLAUDE.md 规范
- **Test Requirements**：
  - `programmatic` TR-1.1：目录 `07-对话记录/2026-03/` 存在
- **Notes**：对话记录保存路径规范：`07-对话记录/YYYY-MM/`

## [x] 任务 2：处理收集箱文件 - 30天极限拿证方案
- **Priority**：P1
- **Depends On**：None
- **Description**：
  - 分析文件内容，确定分类
  - 将文件移动到合适的目录（考虑到是学习计划，可能放在 `01-项目/` 或 `02-领域/`）
  - 添加适当的标签
- **Success Criteria**：
  - 收集箱清空或文件已正确分类
- **Test Requirements**：
  - `human-judgement` TR-2.1：文件已移动到合适的目录并添加了标签
- **Notes**：这是一个软考学习计划，建议创建新项目或放在领域目录

## [x] 任务 3：检查并补充 water-web 项目开发日志
- **Priority**：P1
- **Depends On**：None
- **Description**：
  - 检查现有开发日志
  - 补充缺失的日期（2026-03-28 到 2026-03-31）
  - 从 tasks.md 和对话记录中提取内容
- **Success Criteria**：
  - 开发日志连续完整
- **Test Requirements**：
  - `programmatic` TR-3.1：2026-03-28 到 2026-03-31 的开发日志文件存在
- **Notes**：参考 tasks.md 中的更新历史

## [x] 任务 4：同步项目对话记录到 07-对话记录
- **Priority**：P2
- **Depends On**：Task 1
- **Description**：
  - 将 water-web 项目的对话记录复制/移动到 `07-对话记录/2026-03/`
  - 保持项目内的副本作为项目专属记录
- **Success Criteria**：
  - 对话记录在两个位置都存在（或按约定处理）
- **Test Requirements**：
  - `human-judgement` TR-4.1：对话记录已同步到正确位置
- **Notes**：需要确认是复制还是移动
