---
name: weekly-report
description: Generate a personal weekly report by scanning your Yuque knowledge bases for documents created or updated this week. For personal/individual use — summarizes your own documentation activity.
license: Apache-2.0
compatibility: Requires yuque-mcp server connected to a Yuque account with personal Token
metadata:
  author: chen201724
  version: "2.0"
---

# Weekly Report — Personal Documentation Activity Report

Scan your personal Yuque knowledge bases, find documents created or updated this week, and generate a structured personal weekly report.

## When to Use

- User asks for a personal weekly report based on their Yuque activity
- User says "生成我的周报", "my weekly report", "本周我写了什么文档"
- End of week personal documentation activity review

## Required MCP Tools

All tools are from the `yuque-mcp` server:

- `yuque_list_repos` — List personal knowledge bases
- `yuque_list_docs` — List documents in a repo (to find recent activity)
- `yuque_create_doc` — Create the weekly report document

## Workflow

### Step 1: Identify the Report Period

Determine the report period:
- **Report period** — default to the current week (Monday to Sunday)

If the user doesn't specify, use the current week.

### Step 2: Collect Data

#### 2a. List Personal Repos

```
Tool: yuque_list_repos
Parameters:
  type: "user"
```

#### 2b. Scan Each Repo for Recent Activity

For each personal knowledge base, list documents and filter by this week:

```
Tool: yuque_list_docs
Parameters:
  namespace: "<repo_namespace>"
```

From the results, identify:
- Documents created this week (by `created_at`)
- Documents updated this week (by `updated_at`)

### Step 3: Analyze the Data

Calculate and identify:
- **Total new documents** this week
- **Total updates** this week
- **Most active repos** (which knowledge bases had the most activity)
- **Knowledge base activity distribution** (how activity is spread across repos)
- **Notable highlights** (any unusually high activity, new repos, etc.)

### Step 4: Generate the Report

Use this template:

```markdown
# 📊 个人知识周报

> **周期**：YYYY-MM-DD（周一）至 YYYY-MM-DD（周日）
> **生成时间**：YYYY-MM-DD HH:MM

---

## 📈 本周概览

| 指标 | 本周 |
|------|------|
| 新建文档 | XX 篇 |
| 更新文档 | XX 篇 |
| 活跃知识库 | XX 个 |

---

## 📝 文档动态

### 新建文档

| # | 文档标题 | 知识库 | 创建时间 |
|---|---------|--------|----------|
| 1 | [标题] | [库名] | MM-DD |
| 2 | [标题] | [库名] | MM-DD |

### 更新文档

| # | 文档标题 | 知识库 | 更新时间 |
|---|---------|--------|----------|
| 1 | [标题] | [库名] | MM-DD |

---

## 📚 知识库活跃度

| 知识库 | 新建 | 更新 | 总文档 | 活跃度 |
|--------|------|------|--------|--------|
| [库名] | X 篇 | X 篇 | XX 篇 | 🟢 高 |
| [库名] | X 篇 | X 篇 | XX 篇 | 🟡 中 |

---

## 📊 趋势分析

- [对本周数据的简要分析，2-3 句话]
- [值得关注的亮点]

---

## 💡 下周计划建议

1. **[建议 1]**：[具体建议内容]
2. **[建议 2]**：[具体建议内容]

---

> 📌 本报告基于个人语雀知识库活动数据自动生成，数据截至 YYYY-MM-DD。
```

### Step 5: Save to Yuque

Ask the user which personal repo to save to, or suggest a "周报" repo if one exists.

```
Tool: yuque_create_doc
Parameters:
  repo_id: "<namespace>"    # e.g., "username/weekly-reports"
  title: "个人知识周报 YYYY-MM-DD ~ YYYY-MM-DD"
  body: "<formatted report>"
  format: "markdown"
```

### Step 6: Confirm

```markdown
✅ 个人周报已生成并保存！

📄 **[个人知识周报 日期范围](文档链接)**
📚 已归档到：「知识库名称」

### 本周亮点
- 共新建 X 篇文档，更新 X 篇
- 最活跃知识库：[库名]
- [其他亮点]
```

## Guidelines

- This is a personal report — no "成员贡献" section, use "知识库活跃度" instead
- If no activity this week, create a brief report noting zero activity and suggest topics to document
- Keep suggestions constructive and specific
- Use emoji in headers for visual scanning but keep the tone professional
- Default report language is Chinese
- For team weekly reports, use the `weekly-report` skill in the `yuque-group` plugin

## Error Handling

| Situation | Action |
|-----------|--------|
| `yuque_list_repos` returns empty | Inform user no personal repos found |
| `yuque_list_docs` fails for a repo | Skip that repo, note it in the report |
| No activity this week | Create a brief report noting zero activity, suggest topics to write about |
| Too many repos to scan | Focus on the most recently active repos (top 10) |
