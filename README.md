# 语雀 Claude Code Plugin / Yuque Claude Code Plugin

语雀 AI 生态 Marketplace — 为 Claude Code 提供语雀知识库集成能力。

## 📦 Plugins

本仓库包含两个 Plugin，按使用场景选择安装：

| Plugin | 描述 | Skills | 环境变量 |
|--------|------|--------|----------|
| `yuque-personal` | 个人版 — 个人知识库 AI 集成 | 4 个 | `YUQUE_PERSONAL_TOKEN` |
| `yuque-group` | 团队版 — 团队知识库 AI 集成 | 6 个 | `YUQUE_GROUP_TOKEN` |

两个 Plugin 都包含 25 个 MCP Tools（由 [yuque-mcp](https://github.com/yuque/yuque-mcp-server) 提供）。

### 👤 yuque-personal — 个人版

适合个人用户，操作个人知识库：

| Skill | 描述 |
|-------|------|
| `smart-search` | 自然语言搜索个人文档，智能摘要回答 |
| `meeting-notes` | 自动生成结构化会议纪要，保存到个人知识库 |
| `weekly-report` | 汇总个人一周文档活动，生成周报 |
| `tech-design` | 根据需求生成技术方案文档 |

### 👥 yuque-group — 团队版

适合团队使用，操作团队知识库（包含个人版全部能力 + 团队专属能力）：

| Skill | 描述 |
|-------|------|
| `smart-search` | 自然语言搜索团队文档，智能摘要回答 |
| `meeting-notes` | 自动生成结构化会议纪要，保存到团队知识库 |
| `weekly-report` | 汇总团队一周文档活动，生成团队周报 |
| `tech-design` | 根据需求生成技术方案文档，保存到团队知识库 |
| `onboarding-guide` | 扫描团队知识库，为新成员生成入职阅读指南 |
| `knowledge-report` | 分析团队知识库健康度，生成知识管理月报 |

## 🚀 安装 / Installation

### 1. 添加 Marketplace

```bash
# 终端
claude plugin marketplace add yuque/yuque-plugin

# 或在 Claude Code 内部
/plugin marketplace add yuque/yuque-plugin
```

### 2. 安装 Plugin

```bash
# 安装个人版
claude plugin install yuque-personal@yuque

# 或安装团队版
claude plugin install yuque-group@yuque

# 也可以两个都装（使用不同的 Token）
```

## ⚙️ 配置 / Configuration

### 获取 Token

1. 登录 [语雀](https://www.yuque.com)
2. 进入 **个人设置** → **Token** → **新建**（或直接访问 [Token 设置页](https://www.yuque.com/settings/tokens)）
3. 勾选需要的权限（建议全选读写权限）
4. 复制生成的 Token

### 设置环境变量

根据安装的 Plugin 设置对应的环境变量：

```bash
# 个人版
echo 'export YUQUE_PERSONAL_TOKEN="your-personal-token"' >> ~/.zshrc

# 团队版
echo 'export YUQUE_GROUP_TOKEN="your-group-token"' >> ~/.zshrc

source ~/.zshrc
```

> 💡 个人 Token 和团队 Token 可以是同一个，也可以是不同的。团队 Token 需要有团队级别的访问权限。

## 🔄 更新 / Upgrade

```bash
# 更新个人版
claude plugin update yuque-personal@yuque

# 更新团队版
claude plugin update yuque-group@yuque
```

MCP Server（yuque-mcp）通过 `npx -y yuque-mcp@latest` 运行，每次启动自动使用最新版本。

## 🔗 相关项目 / Related Projects

- [yuque-mcp-server](https://github.com/yuque/yuque-mcp-server) — 语雀 MCP Server（25 Tools）
- [yuque-ecosystem](https://github.com/yuque/yuque-ecosystem) — 语雀 AI 生态主页

## 📄 License

MIT
