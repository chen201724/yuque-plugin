---
name: weekly-report
description: Generate personal or team weekly reports from Yuque activity data. For personal use, summarizes your document creation and editing activity. For team use, includes member contributions and group stats.
license: Apache-2.0
compatibility: Requires yuque-mcp server. Personal mode works with any Yuque API Token. Team mode requires a Group Access Token with `statistic:read` permission.
metadata:
  author: yuque
  version: "2.0"
---

# Weekly Report — Personal / Team Documentation Activity Report

Collect activity data from Yuque and generate a structured weekly report. Supports two modes:

- **Personal mode** (default): Uses your own document activity to generate a personal weekly report.
- **Team mode**: Uses group stats APIs to generate a team-level weekly report. Requires a Group Access Token.

## When to Use

- User asks for a weekly report based on Yuque activity
- User says "生成周报", "我的周报", "个人周报", "team weekly report", "本周文档活动总结"
- End of week documentation activity review

## Required MCP Tools

All tools are from the `yuque-mcp` server:

**Personal mode:**

- `yuque_user_info` — Get current user information
- `yuque_list_repos` — List user's personal knowledge bases (type=user)
- `yuque_list_docs` — List documents in each knowledge base
- `yuque_create_doc` — Create the weekly report document

**Team mode (additional):**

- `yuque_group_doc_stats` — Get document activity stats for a group
- `yuque_group_member_stats` — Get member contribution stats for a group

## Workflow

### Step 1: Determine Mode — Personal or Team

**Do NOT proactively ask for a team identifier!**

Determine the mode based on the user's input:

| User says | Mode |
|-----------|------|
| "生成周报" / "我的周报" / "个人周报" / no team specified | → **Personal mode** |
| "帮我生成本周周报" (without team name) | → **Personal mode** |
| "XX 团队的周报" / specifies a group login | → **Team mode** |

**Rule: When in doubt, default to personal mode.** Only use team mode when the user explicitly provides a team/group name.

---

### Personal Mode

#### Step 2a: Get User Info

```
Tool: yuque_user_info
```

Returns: user name, login, avatar, etc. Use for the report header.

#### Step 3a: List Personal Repos

```
Tool: yuque_list_repos
Parameters:
  login: "<user_login>"    # from yuque_user_info
  type: "user"
```

Returns: list of personal knowledge bases.

#### Step 4a: Collect Document Activity

For each knowledge base, list recent documents:

```
Tool: yuque_list_docs
Parameters:
  namespace: "<repo_namespace>"
```

From the returned documents, filter for this week's activity:
- **New documents**: `created_at` falls within this week (Monday–Sunday)
- **Updated documents**: `updated_at` falls within this week AND `updated_at` ≠ `created_at`
- Collect: title, word_count, created_at, updated_at, repo name

#### Step 5a: Analyze Personal Data

Calculate:
- **Total new documents** created this week
- **Total documents updated** this week
- **Total word count** of new documents
- **Most active knowledge base** (by number of doc changes)
- **Daily activity distribution** (which days were most productive)

#### Step 6a: Generate Personal Report

Use this template:

```markdown
# 📊 个人知识周报

> **作者**：[用户名]
> **周期**：YYYY-MM-DD（周一）至 YYYY-MM-DD（周日）
> **生成时间**：YYYY-MM-DD HH:MM

---

## 📈 本周概览

| 指标 | 数量 |
|------|------|
| 新建文档 | XX 篇 |
| 更新文档 | XX 篇 |
| 新增字数 | ~XX 字 |
| 活跃知识库 | XX 个 |

---

## 📝 新建文档

| # | 文档标题 | 知识库 | 字数 | 创建时间 |
|---|---------|--------|------|----------|
| 1 | [标题] | [库名] | ~X 字 | MM-DD |
| 2 | [标题] | [库名] | ~X 字 | MM-DD |

## ✏️ 更新文档

| # | 文档标题 | 知识库 | 更新时间 |
|---|---------|--------|----------|
| 1 | [标题] | [库名] | MM-DD |
| 2 | [标题] | [库名] | MM-DD |

---

## 📚 知识库活跃度

| 知识库 | 新建 | 更新 | 活跃度 |
|--------|------|------|--------|
| [库名] | X 篇 | X 篇 | 🟢 高 |
| [库名] | X 篇 | X 篇 | 🟡 中 |

---

## 📊 本周小结

- [对本周文档活动的简要总结，2-3 句话]
- [知识产出的亮点或趋势]

---

> 📌 本报告基于语雀个人文档活动数据自动生成，数据截至 YYYY-MM-DD。
```

#### Step 7a: Save to Yuque

Ask the user which repo to save to, or suggest a suitable one.

```
Tool: yuque_create_doc
Parameters:
  repo_id: "<namespace>"
  title: "个人知识周报 YYYY-MM-DD ~ YYYY-MM-DD"
  body: "<formatted report>"
  format: "markdown"
```

#### Step 8a: Confirm

```markdown
✅ 个人周报已生成并保存！

📄 **[个人知识周报 日期范围](文档链接)**
📚 已归档到：「知识库名称」

### 本周亮点
- 共新建 X 篇文档，更新 X 篇
- 新增约 X 字
- 最活跃知识库：[库名]
```

---

### Team Mode

> ⚠️ Team mode requires a **Group Access Token** with `statistic:read` permission. Personal tokens cannot access group stats.

#### Step 2b: Collect Team Data

##### 2b-i. Document Activity Stats

```
Tool: yuque_group_doc_stats
Parameters:
  login: "<group_login>"
```

Returns: new docs created, docs updated, total views, etc.

##### 2b-ii. Member Contribution Stats

```
Tool: yuque_group_member_stats
Parameters:
  login: "<group_login>"
```

Returns: per-member doc count, word count, activity metrics.

##### 2b-iii. Repository List (for context)

```
Tool: yuque_list_repos
Parameters:
  login: "<group_login>"
  type: "group"
```

Provides repo names for richer context in the report.

#### Step 3b: Analyze Team Data

Calculate and identify:
- **Total new documents** this week
- **Total updates** this week
- **Most active members** (top 3-5 by contribution)
- **Most active repos** (if data available)
- **Week-over-week trends** (if previous data available)
- **Notable highlights** (any unusually high activity, new repos, etc.)

#### Step 4b: Generate Team Report

Use this template:

```markdown
# 📊 团队知识周报

> **团队**：[团队名称]
> **周期**：YYYY-MM-DD（周一）至 YYYY-MM-DD（周日）
> **生成时间**：YYYY-MM-DD HH:MM

---

## 📈 本周概览

| 指标 | 本周 | 上周 | 变化 |
|------|------|------|------|
| 新建文档 | XX 篇 | - | - |
| 更新文档 | XX 篇 | - | - |
| 总浏览量 | XX 次 | - | - |
| 活跃成员 | XX 人 | - | - |

---

## 📝 文档动态

### 新建文档

| # | 文档标题 | 作者 | 知识库 | 创建时间 |
|---|---------|------|--------|----------|
| 1 | [标题] | [作者] | [库名] | MM-DD |
| 2 | [标题] | [作者] | [库名] | MM-DD |

### 热门更新

| # | 文档标题 | 更新者 | 更新次数 |
|---|---------|--------|----------|
| 1 | [标题] | [作者] | X 次 |

---

## 👥 成员贡献

| 排名 | 成员 | 新建文档 | 更新文档 | 字数贡献 |
|------|------|----------|----------|----------|
| 🥇 | [姓名] | X 篇 | X 篇 | ~X 字 |
| 🥈 | [姓名] | X 篇 | X 篇 | ~X 字 |
| 🥉 | [姓名] | X 篇 | X 篇 | ~X 字 |

---

## 📊 趋势分析

- [对本周数据的简要分析，2-3 句话]
- [与上周对比的变化趋势]
- [值得关注的亮点或问题]

---

## 💡 建议

1. **[建议 1]**：[具体建议内容]
2. **[建议 2]**：[具体建议内容]

---

> 📌 本报告基于语雀团队活动数据自动生成，数据截至 YYYY-MM-DD。
```

#### Step 5b: Save to Yuque

Ask the user which repo to save to, or suggest a "周报" / "团队管理" repo if one exists.

```
Tool: yuque_create_doc
Parameters:
  repo_id: "<namespace>"
  title: "团队知识周报 YYYY-MM-DD ~ YYYY-MM-DD"
  body: "<formatted report>"
  format: "markdown"
```

#### Step 6b: Confirm

```markdown
✅ 团队周报已生成并保存！

📄 **[团队知识周报 日期范围](文档链接)**
📚 已归档到：「知识库名称」

### 本周亮点
- 共新建 X 篇文档，更新 X 篇
- 最活跃成员：[姓名]（X 篇文档）
- [其他亮点]
```

## Guidelines

- **Never proactively ask for a team identifier** — default to personal mode
- If week-over-week comparison data is not available, omit the "上周" and "变化" columns — don't fabricate numbers
- Keep suggestions constructive and specific
- If a team has many members, show top 5 in the main table and mention total count
- Use emoji in headers for visual scanning but keep the tone professional
- Default report language is Chinese
- For personal mode, if the user has many repos, focus on repos with activity this week

## Error Handling

| Situation | Action |
|-----------|--------|
| User doesn't specify team | Default to **personal mode**, do NOT ask for group login |
| `yuque_user_info` fails | Inform user, check yuque-mcp connection |
| `yuque_list_repos` returns empty | Inform user no knowledge bases found |
| `yuque_list_docs` returns no recent docs | Generate a brief report noting low activity this week |
| `yuque_group_doc_stats` fails | Check if group login is correct; suggest user verify they have a Group Access Token |
| `yuque_group_member_stats` fails | Generate report without member breakdown, note the gap |
| Group has no activity this week | Create a brief report noting zero activity |
| API returns partial data | Generate report with available data, note what's missing |

## Examples

### Example 1: Personal Mode

User: "帮我生成本周周报"

1. → Personal mode (no team specified)
2. `yuque_user_info()` → get user login
3. `yuque_list_repos(login="user-login", type="user")` → 3 repos
4. `yuque_list_docs(namespace="user/repo1")` → filter this week's docs
5. Generate personal report
6. Save and confirm

### Example 2: Team Mode

User: "帮我生成本周的团队周报，团队是 my-awesome-team"

1. → Team mode (team explicitly specified)
2. `yuque_group_doc_stats(login="my-awesome-team")` → 12 new docs, 34 updates
3. `yuque_group_member_stats(login="my-awesome-team")` → 8 active members
4. `yuque_list_repos(login="my-awesome-team", type="group")` → 5 repos for context
5. Generate team report
6. Save and confirm
