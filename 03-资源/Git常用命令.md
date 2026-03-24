---
created: 2026-03-24
tags: [工具, Git, 版本控制]
category: 开发工具
---

# Git 常用命令指南

> Git 版本控制核心命令速查表

## 📦 基础配置

### 用户配置
```bash
# 设置用户名和邮箱
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"

# 查看配置
git config --list
git config user.name
```

### 常用配置
```bash
# 设置默认编辑器
git config --global core.editor "code --wait"

# 设置默认分支名
git config --global init.defaultBranch main

# 启用颜色输出
git config --global color.ui auto

# 设置换行符处理（macOS/Linux）
git config --global core.autocrlf input

# 设置换行符处理（Windows）
git config --global core.autocrlf true
```

## 🚀 仓库操作

### 初始化仓库
```bash
# 初始化新仓库
git init

# 克隆远程仓库
git clone https://github.com/user/repo.git
git clone git@github.com:user/repo.git

# 克隆指定分支
git clone -b develop https://github.com/user/repo.git
```

### 远程仓库管理
```bash
# 查看远程仓库
git remote -v

# 添加远程仓库
git remote add origin https://github.com/user/repo.git

# 修改远程仓库地址
git remote set-url origin https://github.com/user/new-repo.git

# 删除远程仓库
git remote remove origin
```

## 📝 日常操作

### 查看状态
```bash
# 查看工作区状态
git status

# 查看简短状态
git status -s

# 查看分支图
git log --graph --oneline --all

# 查看最近提交
git log --oneline -10
```

### 添加文件
```bash
# 添加指定文件
git add filename.txt

# 添加所有更改
git add .

# 添加所有txt文件
git add *.txt

# 交互式添加
git add -i
```

### 提交更改
```bash
# 提交更改
git commit -m "feat: add new feature"

# 修改最后一次提交
git commit --amend -m "fix: correct commit message"

# 修改最后一次提交（不修改消息）
git commit --amend --no-edit

# 跳过暂存区直接提交
git commit -am "fix: quick fix"
```

## 🔄 分支操作

### 分支管理
```bash
# 查看所有分支
git branch -a

# 创建新分支
git branch feature/new-feature

# 切换分支
git checkout feature/new-feature

# 创建并切换分支
git checkout -b feature/new-feature

# 删除分支
git branch -d feature/old-feature

# 强制删除分支
git branch -D feature/old-feature

# 重命名分支
git branch -m old-name new-name
```

### 分支合并
```bash
# 合并分支
git merge feature/new-feature

# 变基合并
git rebase main

# 中止合并
git merge --abort

# 中止变基
git rebase --abort

# 继续变基（解决冲突后）
git rebase --continue
```

## 🔀 远程同步

### 拉取更新
```bash
# 拉取并合并
git pull origin main

# 拉取但不合并
git pull --no-ff origin main

# 变基拉取
git pull --rebase origin main

# 获取远程更新（不合并）
git fetch origin
```

### 推送更改
```bash
# 推送到远程
git push origin main

# 推送新分支
git push -u origin feature/new-feature

# 强制推送（危险！）
git push --force origin main

# 删除远程分支
git push origin --delete feature/old-feature
```

## 🔍 查看历史

### 查看提交
```bash
# 查看提交历史
git log

# 查看简洁历史
git log --oneline

# 查看带图形的历史
git log --graph --oneline --all

# 查看指定文件的修改历史
git log -p filename.txt

# 查看指定时间范围的历史
git log --since="2026-01-01" --until="2026-03-24"
```

### 查看差异
```bash
# 查看工作区与暂存区的差异
git diff

# 查看暂存区与仓库的差异
git diff --staged

# 查看两个分支的差异
git diff main feature/new-feature

# 查看指定文件的差异
git diff filename.txt
```

### 查看信息
```bash
# 查看提交详情
git show commit-hash

# 查看文件修改统计
git diff --stat

# 查看贡献者统计
git shortlog -sn

# 查看文件修改行数
git diff --numstat
```

## ↩️ 撤销操作

### 撤销更改
```bash
# 撤销工作区更改
git checkout -- filename.txt

# 撤销所有工作区更改
git checkout -- .

# 撤销暂存区更改
git reset HEAD filename.txt

# 撤销所有暂存区更改
git reset HEAD
```

### 重置提交
```bash
# 软重置（保留更改在暂存区）
git reset --soft HEAD~1

# 混合重置（保留更改在工作区）
git reset HEAD~1

# 硬重置（丢弃所有更改）
git reset --hard HEAD~1

# 重置到指定提交
git reset --hard commit-hash
```

### 回退版本
```bash
# 创建新提交回退
git revert commit-hash

# 回退多个提交
git revert HEAD~3..HEAD
```

## 🏷️ 标签管理

### 创建标签
```bash
# 创建轻量标签
git tag v1.0.0

# 创建附注标签
git tag -a v1.0.0 -m "Release version 1.0.0"

# 为指定提交创建标签
git tag -a v1.0.0 commit-hash -m "Release version 1.0.0"
```

### 管理标签
```bash
# 查看所有标签
git tag

# 查看标签详情
git show v1.0.0

# 推送标签到远程
git push origin v1.0.0

# 推送所有标签
git push origin --tags

# 删除本地标签
git tag -d v1.0.0

# 删除远程标签
git push origin --delete v1.0.0
```

## 🧹 清理操作

### 清理文件
```bash
# 查看未跟踪文件
git clean -n

# 删除未跟踪文件
git clean -f

# 删除未跟踪文件和目录
git clean -fd

# 删除忽略的文件
git clean -fx
```

### 垃圾回收
```bash
# 执行垃圾回收
git gc

# 清理松散对象
git gc --prune=now

# 优化仓库
git repack -a -d --depth=250 --window=250
```

## 🔧 高级技巧

### 暂存更改
```bash
# 暂存更改
git stash

# 暂存更改并添加描述
git stash push -m "work in progress"

# 查看暂存列表
git stash list

# 恢复暂存
git stash pop

# 恢复指定暂存
git stash apply stash@{1}

# 删除暂存
git stash drop stash@{0}

# 清空所有暂存
git stash clear
```

### 补丁操作
```bash
# 创建补丁
git diff > changes.patch

# 应用补丁
git apply changes.patch

# 创建格式化补丁
git format-patch -1 commit-hash

# 应用格式化补丁
git am 0001-commit-message.patch
```

### 子模块
```bash
# 添加子模块
git submodule add https://github.com/user/repo.git path/to/submodule

# 初始化子模块
git submodule init

# 更新子模块
git submodule update

# 克隆包含子模块的仓库
git clone --recurse-submodules https://github.com/user/repo.git
```

## 📋 常用工作流

### Git Flow
```bash
# 开发新功能
git checkout develop
git checkout -b feature/new-feature
# ... 开发 ...
git checkout develop
git merge --no-ff feature/new-feature
git branch -d feature/new-feature

# 发布版本
git checkout develop
git checkout -b release/1.0.0
# ... 准备发布 ...
git checkout main
git merge --no-ff release/1.0.0
git tag -a v1.0.0
git checkout develop
git merge --no-ff release/1.0.0
git branch -d release/1.0.0

# 修复bug
git checkout main
git checkout -b hotfix/fix-bug
# ... 修复 ...
git checkout main
git merge --no-ff hotfix/fix-bug
git tag -a v1.0.1
git checkout develop
git merge --no-ff hotfix/fix-bug
git branch -d hotfix/fix-bug
```

### GitHub Flow
```bash
# 创建功能分支
git checkout -b feature/new-feature
# ... 开发 ...
git push origin feature/new-feature
# 创建 Pull Request
# 合并后删除分支
git checkout main
git pull origin main
git branch -d feature/new-feature
```

## 🚨 常见问题

### 冲突解决
```bash
# 查看冲突文件
git status

# 手动解决冲突后
git add resolved-file.txt
git commit -m "resolve merge conflict"

# 使用合并工具
git mergetool
```

### 误删分支恢复
```bash
# 查看操作历史
git reflog

# 恢复分支
git checkout -b recovered-branch commit-hash
```

### 误提交敏感信息
```bash
# 从历史中删除文件
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch path/to/sensitive-file" \
  --prune-empty --tag-name-filter cat -- --all

# 使用 BFG Repo-Cleaner
bfg --delete-files sensitive-file.txt
git reflog expire --expire=now --all
git gc --prune=now --aggressive
```

## 📚 学习资源

### 官方文档
- [Git官方文档](https://git-scm.com/doc)
- [Pro Git书籍](https://git-scm.com/book/zh/v2)

### 在线资源
- [GitHub Guides](https://guides.github.com/)
- [Atlassian Git教程](https://www.atlassian.com/git/tutorials)

### 相关笔记
- [[Git工作流]] - 团队协作流程
- [[Git Hooks]] - 自动化钩子
- [[GitHub使用技巧]] - GitHub高级功能

---

> 💡 **提示**：养成频繁提交的习惯，每次提交都应该是一个完整的、有意义的更改。

*最后更新：2026-03-24*
