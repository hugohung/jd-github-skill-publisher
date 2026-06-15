# 🚀 GitHub Skill 发布管理工具

> **WorkBuddy Skill** — 管理 WorkBuddy Skill 在 GitHub 上的完整生命周期：发布、更新、查询、删除

[![Version](https://img.shields.io/github/v/release/hugohung/workbuddy-skill-github-skill-publisher?style=flat-square)](https://github.com/hugohung/workbuddy-skill-github-skill-publisher/releases)
[![License](https://img.shields.io/github/license/hugohung/workbuddy-skill-github-skill-publisher?style=flat-square)](LICENSE)
[![WorkBuddy](https://img.shields.io/badge/WorkBuddy-skill-orange.svg?style=flat-square)](https://www.codebuddy.cn)
[![GitHub Stars](https://img.shields.io/github/stars/hugohung/workbuddy-skill-github-skill-publisher?style=flat-square)](https://github.com/hugohung/workbuddy-skill-github-skill-publisher/stargazers)
[![GitHub Issues](https://img.shields.io/github/issues/hugohung/workbuddy-skill-github-skill-publisher?style=flat-square)](https://github.com/hugohung/workbuddy-skill-github-skill-publisher/issues)
[![gh CLI](https://img.shields.io/badge/gh%20CLI-required-blue?style=flat-square)](https://cli.github.com/)

---

## 📋 核心能力

本 Skill 提供 WorkBuddy Skill 在 GitHub 上的完整生命周期管理，一键发布、更新、查询和删除。

### ✨ 功能特性

| 功能 | 说明 | 核心能力 |
|------|------|----------|
| 📤 **发布** | 一键将本地 Skill 发布到 GitHub 公开仓库 | 自动生成 README、MIT LICENSE、创建 Release |
| 🔄 **更新** | 修改后推送新版本 | 自动创建新 Release 并生成版本变更说明 |
| 📋 **查询** | 查看所有已发布/未发布的 Skill 状态 | 包含发布时间、版本号、仓库地址等详情 |
| 🗑️ **删除** | 安全删除 GitHub 仓库 | 需二次确认避免误删，自动校验仓库归属 |
| 📦 **批量发布** | 一次发布多个未发布的 Skill | 支持选择发布顺序、跳过已存在的仓库 |

---

## 🎯 适用场景

- 🚀 **首次发布 Skill** — 将本地开发的 Skill 发布到 GitHub，方便分享和安装
- 🔄 **更新已发布 Skill** — 推送新版本并创建 Release
- 📋 **管理多个 Skill** — 查询所有 Skill 的发布状态
- 🗑️ **清理不需要的仓库** — 删除过时或错误的 Skill 仓库
- 📦 **批量操作** — 一次性发布多个未发布的 Skill

---

## 🚀 快速开始

### 前置要求

1. **安装 gh CLI**
   ```bash
   # macOS
   brew install gh
   
   # 其他平台请参考官方文档
   # https://github.com/cli/cli#installation
   ```

2. **登录 GitHub**
   ```bash
   gh auth login
   ```
   
   > ⚠️ 确保登录账号为 **hugohung**，且有仓库创建/删除权限

3. **验证登录状态**
   ```bash
   gh auth status
   ```

---

## 📖 使用方式

在 WorkBuddy 对话中直接说：

### 功能一：发布 Skill

```
帮我把 my-skill 发布到 GitHub
```

```
发布 skill 到 github
```

**执行流程**：
1. 读取 `~/.workbuddy/skills/<skill-name>/SKILL.md` 获取技能信息
2. 检查 GitHub 上是否已存在同名仓库
3. 在 Skill 目录下生成 `README.md`（如果不存在）
4. 生成 `LICENSE`（MIT），如果不存在
5. 提交代码并推送
6. 创建 Release
7. 输出仓库地址和 zip 下载链接

**返回示例**：
```
✅ 发布成功！
仓库地址：https://github.com/hugohung/workbuddy-skill-my-skill
版本：v1.0.0
已自动生成 README、LICENSE，并创建 v1.0.0 Release

📥 下载链接：
https://github.com/hugohung/workbuddy-skill-my-skill/archive/refs/tags/v1.0.0.zip
```

---

### 功能二：更新 Skill

```
帮我把 my-skill 更新到新版本
```

```
更新 skill 版本
```

**执行流程**：
1. 读取 `SKILL.md` 获取当前版本
2. 询问用户新版本号（遵循 semver：patch/minor/major）
3. 更新 `SKILL.md` 中的 version 字段
4. 提交并推送
5. 创建新 Release

**返回示例**：
```
✅ 更新成功！
仓库地址：https://github.com/hugohung/workbuddy-skill-my-skill
新版本：v1.1.0

📥 下载链接：
https://github.com/hugohung/workbuddy-skill-my-skill/archive/refs/tags/v1.1.0.zip
```

---

### 功能三：查询 Skill

```
我有哪些 Skill 还没发布？
```

```
查看已发布的 skill 仓库
```

**返回示例**：
```
📋 已发布的 Skill (4个)：
✅ drama-topic-research → v2.0.0
   https://github.com/hugohung/workbuddy-skill-drama-topic-research
✅ jd-ai-short-drama-helper → v1.0.0
   https://github.com/hugohung/workbuddy-skill-jd-ai-short-drama-helper

⏳ 未发布的 Skill (2个)：
- test-skill：本地路径 ~/.workbuddy/skills/test-skill
- demo-skill：本地路径 ~/.workbuddy/skills/demo-skill
```

---

### 功能四：删除 Skill

```
帮我把 test-skill 的仓库删除
```

```
删除 github 上的 skill 仓库
```

> ⚠️ **危险操作，必须二次确认！**

**执行流程**：
1. 列出用户要删除的仓库信息
2. **明确警告**：删除后无法恢复，所有人将无法访问
3. 要求用户明确回复"确认删除 <仓库名>"
4. 确认后执行删除
5. 告知用户本地 Skill 文件不受影响

---

### 功能五：批量发布

```
帮我把所有未发布的 skill 都发布到 GitHub
```

```
批量发布 skill
```

**执行流程**：
1. 扫描本地 `~/.workbuddy/skills/` 目录
2. 对比 GitHub 已发布仓库
3. 列出所有未发布的 Skill，让用户选择要发布哪些
4. 按顺序逐个执行发布流程

---

## 📦 安装方式

### 方式一：WorkBuddy 用户

1. 下载 [最新 Release zip](https://github.com/hugohung/workbuddy-skill-github-skill-publisher/releases/latest)
2. 在 WorkBuddy **技能管理** → **上传技能**，选择 zip 文件
3. 安装 `gh` CLI 并登录 GitHub（见上方"前置要求"）
4. 重启 WorkBuddy 即可使用

### 方式二：从源码安装

```bash
git clone https://github.com/hugohung/workbuddy-skill-github-skill-publisher.git ~/.workbuddy/skills/github-skill-publisher
```

安装 `gh` CLI 并登录 GitHub 后，重启 WorkBuddy 即可。

---

## 🔧 仓库命名规则

格式：`workbuddy-skill-<skill-name>`

例如：
- `drama-topic-research` → `workbuddy-skill-drama-topic-research`
- `jd-ai-short-drama-helper` → `workbuddy-skill-jd-ai-short-drama-helper`

---

## 📝 仓库描述格式规则

所有 GitHub 仓库的 description 必须使用以下格式：

```
<emoji> <中文标题> | <中文功能说明>
```

**示例**：
- `🎬 京东AI短剧剧本助手 | 通过对话生成符合京东规范的短剧剧本、人物设定、场景设定和封面提示词`
- `🚀 GitHub Skill发布管理工具 | 管理WorkBuddy Skill在GitHub上的完整生命周期：发布、更新、查询、删除`
- `🔥 热门话题调研专家 | 自动搜集18个平台热榜，分析短剧适配度，生成题材推荐报告`

**常用 emoji 参考**：
- 🎬 影视/短剧相关
- 🛠️ 工具/开发相关
- 🚀 发布/部署相关
- 🔥 热门/趋势相关
- 📊 数据/分析相关
- 🎨 设计/图片相关
- 📝 文档/写作相关

---

## 🔄 版本号规则

遵循 [语义化版本 2.0.0](https://semver.org/lang/zh-CN/)（SemVer）：

- **patch**（补丁版本）：向后兼容的问题修正 → `v1.0.0` → `v1.0.1`
- **minor**（次版本号）：向后兼容的功能新增 → `v1.0.0` → `v1.1.0`
- **major**（主版本号）：不兼容的 API 修改 → `v1.0.0` → `v2.0.0`

---

## ⚠️ 注意事项

1. **gh CLI 必须安装并登录** — 本 Skill 依赖 `gh` CLI 操作 GitHub
2. **删除操作需二次确认** — 避免误删重要仓库
3. **版本号必须遵循 SemVer** — 确保版本管理规范
4. **每次发布/更新都应创建 Release** — 方便用户下载特定版本
5. **不要修改用户 SKILL.md 的核心内容** — 只更新 version 字段

---

## 🐛 错误处理

### gh CLI 未安装

提示用户运行：
```bash
brew install gh
```

### 未登录 GitHub

提示用户运行：
```bash
gh auth login
```

### 仓库已存在

自动切换到更新流程，而非创建新仓库。

### git push 失败

检查 remote 是否正确，尝试重新设置：
```bash
git remote -v
git remote set-url origin https://github.com/hugohung/workbuddy-skill-<name>.git
```

### Release 标签已存在

提示用户使用新版本号。

---

## 🔗 相关 Skill

- [**Skill 介绍页文案生成器**](https://github.com/hugohung/workbuddy-skill-skill-intro-writer) — 为 Skill 生成介绍页文案
- [**序列帧动图工具集**](https://github.com/hugohung/workbuddy-skill-frame-anim-tool) — 序列帧与动图双向转换
- [**热门话题及短剧题材调研专家**](https://github.com/hugohung/workbuddy-skill-drama-topic-research) — 调研短剧题材和热点话题

---

## 📄 License

[MIT License](LICENSE)

---

## 👨‍💻 作者

**honghaoxiang**

- GitHub: [@hugohung](https://github.com/hugohung)
- WorkBuddy: [codebuddy.cn](https://www.codebuddy.cn)

---

## 🙏 致谢

- [GitHub CLI](https://cli.github.com/) — 提供 GitHub 命令行工具
- [WorkBuddy](https://www.codebuddy.cn) — 提供 AI 助手平台
- 所有贡献者和用户的支持

---

## 📋 更新日志

所有版本更新记录请查看 [Releases 页面](https://github.com/hugohung/workbuddy-skill-github-skill-publisher/releases)

### v1.1.0 (2026-06-15)
- ✨ 新增仓库描述格式规则
- ✨ 新增 README 自动生成功能
- 🔧 优化发布流程，提升稳定性

### v1.0.0 (2026-06-12)
- ✨ 首次发布
- ✨ 支持 Skill 发布、更新、查询、删除功能
- ✨ 支持批量发布

---

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request！

1. 提交 Issue 前请先查看[已有 Issue](https://github.com/hugohung/workbuddy-skill-github-skill-publisher/issues)，避免重复
2. 提交 PR 请遵循代码规范，并补充对应测试用例
3. 版本更新需同步更新 `SKILL.md` 与版本号

---

## 🐛 问题反馈

使用中遇到问题请通过以下渠道反馈：

- 提交 [GitHub Issue](https://github.com/hugohung/workbuddy-skill-github-skill-publisher/issues/new)
- 在 WorkBuddy 社区寻求帮助

---

**⭐ 如果这个 Skill 对你有帮助，欢迎 Star 和分享！**
