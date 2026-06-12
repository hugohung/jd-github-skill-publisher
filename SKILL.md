---
name: github-skill-publisher
description: |
  WorkBuddy Skill 发布管理工具 - 将本地 WorkBuddy Skill 发布到 GitHub、更新版本、查询已发布仓库、删除仓库。
  触发词：发布skill、上传skill到github、更新skill版本、删除skill仓库、查看我的skill仓库、publish skill。
version: "1.0.0"
author: honghaoxiang
---

# GitHub Skill Publisher

管理 WorkBuddy Skill 在 GitHub 上的完整生命周期：发布、更新、查询、删除。

## 前置条件

- 已安装 `gh` CLI（`brew install gh`）
- 已登录 GitHub（`gh auth login`），当前账号为 **hugohung**
- Skill 目录位于 `~/.workbuddy/skills/<skill-name>/`

每次执行前，先运行 `gh auth status` 确认登录状态。如果未登录，提示用户在终端执行 `gh auth login`。

## 仓库命名规则

格式：`workbuddy-skill-<skill-name>`

例如：`drama-topic-research` → `workbuddy-skill-drama-topic-research`

## 操作流程

### 1. 发布新 Skill（publish）

将本地 Skill 发布到 GitHub 公开仓库。

**步骤：**

1. 读取 `~/.workbuddy/skills/<skill-name>/SKILL.md` 获取 skill 的 name、description、version
2. 确认该 Skill 目录下有 `.git`，如果没有则 `git init`
3. 检查 GitHub 上是否已存在同名仓库（`gh repo view hugohung/workbuddy-skill-<name>`）
   - 如果已存在 → 进入更新流程
4. 在 Skill 目录下生成 `README.md`（如果不存在），模板见下方
5. 在 Skill 目录下生成 `LICENSE`（MIT），如果不存在
6. 配置 git 用户信息（如未配置）：
   ```bash
   git config --global user.name "honghaoxiang"
   git config --global user.email "hugohung@users.noreply.github.com"
   ```
7. 提交代码并推送：
   ```bash
   cd ~/.workbuddy/skills/<skill-name>
   git add .
   git commit -m "feat: init <skill-name> v<version>"
   git branch -M main
   gh repo create hugohung/workbuddy-skill-<name> --public --source=. --push --description "<description>"
   ```
8. 创建 Release：
   ```bash
   gh release create v<version> --repo hugohung/workbuddy-skill-<name> --title "v<version>" --notes "<description>"
   ```
9. 输出仓库地址和 zip 下载链接：
   - 仓库：`https://github.com/hugohung/workbuddy-skill-<name>`
   - 下载：`https://github.com/hugohung/workbuddy-skill-<name>/archive/refs/tags/v<version>.zip`

### 2. 更新已发布 Skill（update）

更新已发布 Skill 的代码和版本。

**步骤：**

1. 读取 `SKILL.md` 获取当前 version
2. 询问用户新版本号（遵循 semver：patch/minor/major），或根据变更自动建议
3. 更新 `SKILL.md` 中的 version 字段
4. 提交并推送：
   ```bash
   cd ~/.workbuddy/skills/<skill-name>
   git add .
   git commit -m "<commit message>"
   git push
   ```
5. 创建新 Release：
   ```bash
   gh release create v<new-version> --repo hugohung/workbuddy-skill-<name> --title "v<new-version>" --notes "<更新说明>"
   ```
6. 输出新的 zip 下载链接

### 3. 查询已发布仓库（list）

查看所有已发布到 GitHub 的 Skill 仓库。

**步骤：**

1. 运行：
   ```bash
   gh repo list hugohung --limit 50 --json name,description,url --jq '.[] | select(.name | startswith("workbuddy-skill-"))'
   ```
2. 对比本地 `~/.worbuddy/skills/` 目录，标注哪些已发布、哪些未发布
3. 以表格形式展示：

| 本地 Skill | GitHub 仓库 | 版本 | 状态 |
|---|---|---|---|
| drama-topic-research | workbuddy-skill-drama-topic-research | v2.0.0 | ✅ 已发布 |
| png2apng | - | - | ⏳ 未发布 |

### 4. 删除仓库（delete）

删除 GitHub 上的 Skill 仓库。

**⚠️ 危险操作，必须二次确认！**

**步骤：**

1. 列出用户要删除的仓库信息
2. **明确警告**：删除后无法恢复，所有人将无法访问
3. 要求用户明确回复"确认删除 <仓库名>"
4. 确认后执行：
   ```bash
   gh repo delete hugohung/<repo-name> --yes
   ```
5. 告知用户本地 Skill 文件不受影响

### 5. 批量发布（batch-publish）

一次性发布多个未发布的 Skill。

**步骤：**
1. 扫描本地 `~/.workbuddy/skills/` 目录
2. 对比 GitHub 已发布仓库
3. 列出所有未发布的 Skill，让用户选择要发布哪些
4. 按顺序逐个执行发布流程

## README.md 模板

发布新 Skill 时，如果目录下没有 `README.md`，自动生成：

```markdown
# <Skill 显示名>

> WorkBuddy Skill — <一句话描述>

## 功能特性

- <从 SKILL.md 中提取核心功能点>

## 安装方式

### WorkBuddy 用户

1. 下载 [Release zip](../../releases/latest)
2. 在 WorkBuddy 技能管理 → 上传技能，选择 zip 文件

### 从源码安装

\`\`\`bash
git clone https://github.com/hugohung/workbuddy-skill-<name>.git ~/.workbuddy/skills/<name>
\`\`\`

## 使用方式

在 WorkBuddy 对话中直接说：
> "<触发示例>"

## License

MIT License
```

## MIT LICENSE 内容

```
MIT License

Copyright (c) <当前年份> honghaoxiang

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## 错误处理

| 场景 | 处理方式 |
|---|---|
| `gh` 未安装 | 提示用户运行 `brew install gh` |
| 未登录 GitHub | 提示用户运行 `gh auth login` |
| 仓库已存在 | 切换到更新流程而非创建 |
| git push 失败 | 检查 remote 是否正确，尝试重新设置 |
| Release 标签已存在 | 提示用户使用新版本号 |

## 重要规则

- 删除操作必须二次确认，不可跳过
- 版本号遵循 semver 规范（major.minor.patch）
- 每次发布/更新都应创建对应的 Release
- 不要修改用户 SKILL.md 的核心内容，只更新 version 字段
- 如果 Skill 目录已有 `.git` 和 remote，直接用现有配置 push
