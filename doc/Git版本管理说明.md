# Git 版本管理说明

本文档说明如何使用 Git 管理 my_fast_lio 的版本，方便随时返回到之前的工作版本。

---

## 仓库信息

- **仓库地址**：https://github.com/362700054/zy
- **本地路径**：/home/zy/catkin_ws/src/my_fast_lio
- **远程名称**：origin
- **主分支**：main

---

## 快速命令速查

| 操作 | 命令 |
|------|------|
| 查看提交历史 | `git log --oneline` |
| 查看提交详情 | `git show <commit-hash>` |
| 查看当前状态 | `git status` |
| 查看当前分支 | `git branch` |
| 添加文件到暂存区 | `git add .` 或 `git add <文件>` |
| 提交更改 | `git commit -m "说明"` |
| 推送到GitHub | `git push -u origin main` |
| 创建标签 | `git tag -a v1.0 -m "说明"` |
| 推送标签 | `git push origin --tags` |

---

## 更新仓库到GitHub

### 第一次设置（已完成）

```bash
cd /home/zy/catkin_ws/src/my_fast_lio

# 1. 配置Git用户信息（已配置）
git config user.name "362700054"
git config user.email "362700054@qq.com"

# 2. 初始化Git仓库
git init

# 3. 添加所有文件
git add -A

# 4. 提交
git commit -m "Initial: my_fast_lio restore to FAST_LIO2 version"

# 5. 添加远程仓库
git remote add origin git@github.com:362700054/zy.git

# 6. 推送到GitHub（本地是master分支，推送到main）
git push -u origin master:refs/heads/main --force
```

---

## 日常更新流程

### 场景1：添加新功能

```bash
cd /home/zy/catkin_ws/src/my_fast_lio

# 1. 创建新功能分支
git checkout -b feature-body-filter

# 2. 编写/修改代码
# （修改代码...）

# 3. 编译测试
catkin_make

# 4. 测试运行
source devel/setup.bash
roslaunch my_fast_lio mine.launch

# 5. 如果测试通过，提交更改
git add .
git commit -m "feat: 添加body filter功能

实现基于立方体区域的车身点云过滤

具体修改：
- preprocess.h: 添加body filter相关方法和成员变量
- preprocess.cpp: 实现filterBodyPoints函数
- laserMapping.cpp: 添加可视化marker发布"

# 6. 合并到主分支
git checkout main
git merge feature-body-filter

# 7. 推送到GitHub
git push -u origin main

# 8. （可选）删除功能分支
git branch -d feature-body-filter
```

### 场景2：修复bug

```bash
cd /home/zy/catkin_ws/src/my_fast_lio

# 1. 查看当前状态
git status

# 2. 添加修改的文件
git add .

# 3. 提交修复
git commit -m "fix: 修复blind参数读取问题

修改preprocess.cpp中的参数读取路径"

# 4. 推送到GitHub
git push -u origin main
```

### 场景3：出现问题时快速回滚

```bash
cd /home/zy/catkin_ws/src/my_fast_lio

# 1. 查看提交历史
git log --oneline

# 输出示例：
# e09e7bd Initial: my_fast_lio restore to FAST_LIO2 version
# a1b2c3d feat: 添加body filter功能
# d4e5f6a feat: 添加ground constraint功能

# 2. 返回到上一个版本（方式1：checkout）
git checkout e09e7bd

# 或方式2：reset（更彻底，会丢弃之后的所有更改）
git reset --hard e09e7bd

# 3. 如果需要恢复到最新版本
git checkout main
```

### 场景4：保存当前快照

在添加多个功能前，可以先保存当前稳定版本：

```bash
cd /home/zy/catkin_ws/src/my_fast_lio

# 1. 创建标签
git tag -a v0.0 -m "稳定版本：原版FAST_LIO2"

# 2. 推送标签到GitHub
git push origin v0.0

# 3. 后续如果需要恢复
git checkout v0.0
```

---

## 推送命令完整说明

### 推送主分支

```bash
# 本地分支是master，远程是main
git push -u origin master:refs/heads/main --force

# 或使用强制推送（覆盖远程）
git push -u origin main --force
```

### 推送所有分支

```bash
git push --all origin
```

### 推送标签

```bash
git push origin --tags
```

---

## 查看GitHub上的版本

### 方式1：网页查看

访问：https://github.com/362700054/zy/commits/main

### 方式2：命令行查看

```bash
cd /home/zy/catkin_ws/src/my_fast_lio

# 查看最新5次提交
git log --oneline -5

# 查看提交详情
git show e09e7bd

# 查看与远程的差异
git diff origin/main

# 拉取远程最新更新
git pull origin main
```

---

## 常用Git配置

### 配置SSH密钥（推荐）

```bash
# 1. 生成SSH密钥（如果还没有）
ssh-keygen -t ed25519 -C "362700054" -f ~/.ssh/id_ed25519

# 2. 复制公钥
cat ~/.ssh/id_ed25519.pub

# 3. 添加到GitHub
# 访问 https://github.com/settings/keys

# 4. 配置远程URL为SSH
git remote set-url origin git@github.com:362700054/zy.git
```

### 配置用户信息（已设置）

```bash
cd /home/zy/catkin_ws/src/my_fast_lio

# 查看当前配置
git config user.name
git config user.email

# 如果需要修改
git config user.name "362700054"
git config user.email "362700054@qq.com"
```

---

## 提交信息规范

建议使用清晰的提交信息格式：

```
<类型>: <简要描述>

<详细说明>

具体修改：
- 文件1: 修改内容
- 文件2: 修改内容
```

### 提交类型

| 类型 | 说明 | 示例 |
|------|------|------|
| `feat` | 新功能 | `feat: 添加body filter功能` |
| `fix` | 修复bug | `fix: 修复blind参数读取问题` |
| `refactor` | 重构代码 | `refactor: 优化体素滤波算法` |
| `docs` | 文档更新 | `docs: 更新Git使用说明` |
| `style` | 代码格式调整 | `style: 统一代码风格` |
| `perf` | 性能优化 | `perf: 减少计算耗时` |
| `test` | 测试相关 | `test: 添加单元测试` |

---

## 版本标签规范

使用语义化版本号：

```
v<major>.<minor>.<patch>
```

- **major**: 重大更新（如添加新算法模块）
- **minor**: 小功能更新（如优化某模块）
- **patch**: bug修复或小改动

示例：
- `v1.0.0` - 初始版本（原版FAST_LIO2）
- `v1.1.0` - 添加body filter功能
- `v1.2.0` - 添加adaptive voxel filter功能
- `v1.3.0` - 添加ground constraint功能
```

---

## 当前版本状态

```bash
cd /home/zy/catkin_ws/src/my_fast_lio

# 查看当前提交
git log --oneline

# 查看最新标签
git tag

# 查看当前分支
git branch -vv
```

---

## 常见问题排查

### 问题1：推送时提示 "could not read Username"

**原因**：使用HTTPS推送时需要用户名和密码，环境不支持交互式输入

**解决**：使用SSH密钥或切换到本地有凭证缓存的环境

```bash
# 切换到SSH
git remote set-url origin git@github.com:362700054/zy.git
git push -u origin main
```

### 问题2：推送时提示 "源引用规格 main 没有匹配"

**原因**：本地分支名和远程分支名不一致

**解决**：使用正确的分支名

```bash
# 本地是master，远程是main
git push -u origin master:refs/heads/main --force
```

### 问题3：推送时提示 "远程仓库已存在"

**原因**：远程仓库已有内容

**解决**：使用强制推送

```bash
git push -u origin main --force
```

---

## 快速参考命令

| 命令 | 说明 |
|--------|------|
| `git init` | 初始化Git仓库 |
| `git status` | 查看工作目录状态 |
| `git log --oneline` | 查看提交历史（简洁版） |
| `git log --graph` | 查看提交历史（图形版） |
| `git add .` | 添加所有更改 |
| `git add <文件>` | 添加指定文件 |
| `git commit -m "说明"` | 提交更改 |
| `git checkout main` | 切换到主分支 |
| `git checkout -b <分支名>` | 创建并切换到新分支 |
| `git branch` | 查看所有分支 |
| `git branch -d <分支名>` | 删除分支 |
| `git merge <分支名>` | 合并分支到当前分支 |
| `git push -u origin main` | 推送到GitHub |
| `git pull origin main` | 拉取远程更新 |
| `git reset --hard <hash>` | 硬重置到某个提交 |
| `git tag` | 查看所有标签 |
| `git tag -a <标签>` | 创建注释标签 |
| `git push origin --tags` | 推送所有标签 |
| `git remote -v` | 查看远程仓库配置 |
| `git remote set-url origin <URL>` | 修改远程URL |

---

## 创建时间

2026-01-18
