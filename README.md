# 语雀 Claude Code Plugin / Yuque Claude Code Plugin

一键为 Claude Code 集成语雀知识库 AI 能力。

One-click Yuque knowledge base integration for Claude Code.

## ✨ 包含能力 / What's Included

| 类型 | 数量 | 来源 |
|------|------|------|
| MCP Tools | 25 | [yuque-mcp](https://github.com/yuque/yuque-mcp-server) |
| Agent Skills | 10 | [yuque-skills](https://github.com/yuque/yuque-skills) |
| Agent | 1 | yuque-assistant |

### 25 MCP Tools

通过 [yuque-mcp](https://github.com/yuque/yuque-mcp-server) 提供，覆盖语雀 API 全部核心功能：

- 文档 CRUD（创建、读取、更新、删除）
- 知识库管理
- 搜索（全文搜索、知识库内搜索）
- 团队与协作（成员、权限）
- 统计与分析
- 更多...

### 10 Agent Skills

Skills 来自 [yuque-skills](https://github.com/yuque/yuque-skills) 仓库，直接内置在本插件中。

#### 👤 个人 Skills（4）

| Skill | 描述 |
|-------|------|
| `smart-search` | 自然语言搜索语雀文档，智能摘要回答 |
| `meeting-notes` | 从会议内容自动生成结构化会议纪要 |
| `weekly-report` | 汇总一周工作，生成周报并发布到语雀 |
| `tech-design` | 根据需求生成技术方案文档 |

#### 👥 团队 Skills（6）

| Skill | 描述 |
|-------|------|
| `onboarding-guide` | 为新成员生成入职指南 |
| `knowledge-report` | 分析团队知识库健康度，生成月报 |
| `team-wiki-init` | 一键初始化团队知识库结构 |
| `doc-review` | 文档质量审查与改进建议 |
| `permission-audit` | 知识库权限审计与安全报告 |
| `content-migration` | 从其他平台迁移内容到语雀 |

### Agent

`yuque-assistant` — 语雀知识库专业助手，擅长文档管理、知识搜索、内容创作和团队协作。

## 📦 安装 / Installation

### 方式一：通过 Marketplace 安装（推荐）

1. 添加语雀 Marketplace：

```
/plugin marketplace add yuque/yuque-ecosystem
```

2. 安装插件：

```
/plugin install yuque@yuque-ecosystem
```

### 方式二：通过 GitHub 仓库直接安装

1. Clone 仓库到本地：

```bash
git clone git@github.com:yuque/yuque-plugin.git /path/to/yuque-plugin
```

2. 从本地目录安装：

```bash
claude plugin install --dir /path/to/yuque-plugin
```

## 🔄 更新 / Upgrade

### 更新 Plugin（Skills & Agent）

当我们发布新版本的 Skills 或 Agent 时，你可以通过以下方式更新：

1. 更新 Marketplace 目录：
   ```
   /plugin marketplace update
   ```

2. 重新安装 Plugin 以获取最新版本：
   ```
   /plugin install yuque@yuque-ecosystem
   ```

### 更新 MCP Server

MCP Server（yuque-mcp）通过 `npx -y yuque-mcp` 运行，每次启动时会自动检查并使用最新版本，无需手动更新。

如果需要指定版本：
```json
{
  "mcpServers": {
    "yuque": {
      "command": "npx",
      "args": ["-y", "yuque-mcp@1.0.0"]
    }
  }
}
```

### 查看版本

- Plugin 版本：查看 `/plugin` 界面的 Installed tab
- MCP Server 版本：`npx yuque-mcp --version`

## ⚙️ 配置 / Configuration

安装后需要设置语雀 Token 环境变量：

```bash
# 必需 — 语雀个人访问令牌
export YUQUE_TOKEN="your-yuque-token"

# 可选 — 自定义 API 地址（不设置则使用 yuque-mcp 内置默认值 https://www.yuque.com/api/v2）
export YUQUE_API_URL="https://your-yuque-instance.com/api/v2"
```

### 获取 Token / Get Your Token

1. 登录 [语雀](https://www.yuque.com)
2. 进入 **个人设置** → **Token** → **新建**
3. 勾选需要的权限（建议全选读写权限）
4. 复制生成的 Token

## 🔗 相关项目 / Related Projects

- [yuque-mcp-server](https://github.com/yuque/yuque-mcp-server) — 语雀 MCP Server（25 Tools）
- [yuque-skills](https://github.com/yuque/yuque-skills) — 语雀 Agent Skills（10 Skills）
- [yuque-ecosystem](https://github.com/yuque/yuque-ecosystem) — 语雀 AI 生态主页 & Plugin Marketplace

## 📄 License

MIT
