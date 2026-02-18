---
name: tech-design
description: Generate technical design documents using a standard template and save them to Yuque. Use when the user needs to write a technical design doc, RFC, or architecture proposal for a new feature, system, or project.
license: Apache-2.0
compatibility: Requires yuque-mcp server connected to a Yuque account
metadata:
  author: chen201724
  version: "1.0"
---

# Tech Design — Technical Design Document Generator

Help the user write a structured technical design document following a standard template, then save it to Yuque.

## When to Use

- User wants to write a technical design document or RFC
- User says "帮我写技术方案", "write a tech design", "我要写个设计文档"
- User describes a feature/system and needs it formalized into a design doc

## Required MCP Tools

All tools are from the `yuque-mcp` server:

- `yuque_search` — (Optional) Search for related existing docs for context
- `yuque_list_repos` — Find the target knowledge base
- `yuque_create_doc` — Create the design document

## Reference

The full template is in [references/template.md](./references/template.md). Load it when generating the document.

## Workflow

### Step 1: Understand the Requirements

Gather from the user:

| Field | Required | Description |
|-------|----------|-------------|
| 项目/功能名称 | Yes | What is being designed |
| 背景与问题 | Yes | Why this is needed, what problem it solves |
| 目标 | Yes | What success looks like |
| 约束条件 | No | Technical constraints, timeline, budget |
| 已有方案 | No | Any existing approaches or prior art |

If the user provides a brief description, ask clarifying questions:
- "这个功能要解决什么问题？"
- "有什么技术约束吗？比如必须用某个框架、要兼容现有系统？"
- "预期的时间节点是什么？"

### Step 2: (Optional) Search for Context

If relevant, search Yuque for related existing documents:

```
Tool: yuque_search
Parameters:
  query: "<related keywords>"
  type: "doc"
```

This helps:
- Avoid duplicating existing designs
- Reference prior decisions
- Understand the current architecture

### Step 3: Generate the Design Document

Load the template from `references/template.md` and fill in each section based on the user's input and your technical analysis.

Key sections to fill:

1. **背景 (Background)** — Problem statement, current situation
2. **目标 (Goals)** — What this design achieves, non-goals
3. **方案设计 (Design)** — The core technical approach
   - Architecture diagram description (describe in text/ASCII if needed)
   - Core components and their responsibilities
   - Data model / API design
   - Key flows (sequence of operations)
4. **技术选型 (Tech Stack)** — Why specific technologies were chosen
5. **方案对比 (Alternatives)** — Other approaches considered and why they were rejected
6. **排期 (Timeline)** — Milestones and estimated effort
7. **风险评估 (Risks)** — What could go wrong and mitigation strategies
8. **参考资料 (References)** — Related docs, links, prior art

### Step 4: Review with User

Present the draft to the user before saving. Ask:
- "方案内容是否准确？有需要调整的地方吗？"
- "要补充其他技术细节吗？"

### Step 5: Save to Yuque

```
Tool: yuque_list_repos
Parameters:
  login: "<group_login>"
  type: "group"    # or "user" for personal repos
```

Find or ask for the target repo (often "技术方案" or "设计文档" or "RFC").

```
Tool: yuque_create_doc
Parameters:
  repo_id: "<namespace>"    # e.g., "mygroup/mybook"
  title: "[技术方案] <项目名称>"
  body: "<formatted design document>"
  format: "markdown"
```


### Step 6: Confirm

```markdown
✅ 技术方案已创建（草稿状态）！

📄 **[[技术方案] 项目名称](文档链接)**
📚 已保存到：「知识库名称」

### 文档结构
- 背景与目标
- 方案设计（含 X 个核心模块）
- 技术选型对比
- 排期（预计 X 周）
- 风险评估（X 个风险点）

💡 文档为草稿状态，请团队评审后发布。
```

## Guidelines

- Write the design doc in Chinese (default) unless the user specifies English
- Be specific in the design section — include data models, API signatures, flow descriptions
- For tech stack comparison, use a table with pros/cons
- Keep the document actionable — someone should be able to implement from this doc
- If the user's requirements are vague, make reasonable assumptions and note them clearly with "【假设】" markers
- Don't over-engineer — match the design complexity to the project scope

## Error Handling

| Situation | Action |
|-----------|--------|
| User provides very vague requirements | Ask 2-3 targeted questions before generating |
| `yuque_search` finds conflicting existing designs | Mention them and ask user how to reconcile |
| `yuque_create_doc` fails | Show error, offer to output the markdown for manual copy |
| User wants to update an existing design doc | Use `yuque_search` to find it, then suggest creating a v2 or appendix |

## Example

User: "帮我写一个用户权限系统的技术方案，要支持 RBAC，用 Go 实现，需要兼容现有的用户表。"

1. Clarify: timeline? expected scale? existing auth system?
2. Search: `yuque_search(query="权限系统 RBAC")` for existing docs
3. Generate design doc with:
   - Background: current permission gaps
   - Design: RBAC model (users, roles, permissions), Go service architecture
   - Data model: roles table, permissions table, user_roles mapping
   - API: permission check endpoints
   - Tech stack: Go + PostgreSQL + Casbin (with comparison to alternatives)
   - Timeline: 3 phases over 4 weeks
   - Risks: migration complexity, backward compatibility
4. Present draft for review
5. Save as draft to Yuque
