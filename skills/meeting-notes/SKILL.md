---
name: meeting-notes
description: Format meeting content into structured meeting notes and archive them to Yuque. Use when the user provides meeting information (agenda, discussion points, decisions) and wants to create a well-formatted meeting notes document in their Yuque knowledge base.
license: Apache-2.0
compatibility: Requires yuque-mcp server connected to a Yuque account
metadata:
  author: yuque
  version: "1.0"
---

# Meeting Notes — Format and Archive to Yuque

Take raw meeting information from the user, format it into a standard meeting notes template, and create a document in the appropriate Yuque knowledge base.

## When to Use

- User shares meeting content and wants it saved to Yuque
- User says "帮我整理会议纪要", "create meeting notes", "记录一下今天的会议"
- User pastes unstructured meeting notes and wants them formatted

## Required MCP Tools

All tools are from the `yuque-mcp` server:

- `yuque_list_repos` — List available knowledge bases to find the target repo
- `yuque_create_doc` — Create the meeting notes document

## Workflow

### Step 1: Gather Meeting Information

Extract or ask for the following from the user's input:

| Field | Required | Example |
|-------|----------|---------|
| 会议主题 (Topic) | Yes | "Q1 产品规划评审" |
| 会议日期 (Date) | Yes (default: today) | "2024-01-15" |
| 会议时间 (Time) | No | "14:00-15:30" |
| 参会人员 (Attendees) | Yes | "张三、李四、王五" |
| 主持人 (Host) | No | "张三" |
| 记录人 (Recorder) | No | "AI 助手" |

If the user provides raw/unstructured content, extract these fields from context. If critical fields are missing (topic, attendees), ask the user.

### Step 2: Structure the Content

Organize the meeting content into these sections:

1. **议题 (Agenda Items)** — What was discussed
2. **讨论要点 (Discussion Points)** — Key arguments, opinions, data shared
3. **决议 (Decisions)** — What was decided
4. **待办事项 (Action Items)** — Who does what by when
5. **备注 (Notes)** — Anything else worth recording

For each action item, ensure it has:
- **负责人** (Owner): Who is responsible
- **截止日期** (Deadline): When it's due
- **具体内容** (Description): What needs to be done

### Step 3: Format the Document

Use this template:

```markdown
# 会议纪要：[会议主题]

| 项目 | 内容 |
|------|------|
| 📅 日期 | YYYY-MM-DD |
| ⏰ 时间 | HH:MM - HH:MM |
| 📍 地点 | [线上/会议室名称] |
| 👥 参会人 | [姓名列表] |
| 🎙️ 主持人 | [姓名] |
| ✍️ 记录人 | [姓名/AI 助手] |

---

## 📋 议题

1. [议题 1]
2. [议题 2]
3. [议题 3]

---

## 💬 讨论要点

### 议题 1：[标题]

- [要点 1]
- [要点 2]
- [要点 3]

### 议题 2：[标题]

- [要点 1]
- [要点 2]

---

## ✅ 决议

1. **[决议 1]**
   - 说明：[补充细节]
2. **[决议 2]**
   - 说明：[补充细节]

---

## 📌 待办事项

| # | 事项 | 负责人 | 截止日期 | 状态 |
|---|------|--------|----------|------|
| 1 | [具体任务] | [姓名] | YYYY-MM-DD | ⬜ 待开始 |
| 2 | [具体任务] | [姓名] | YYYY-MM-DD | ⬜ 待开始 |
| 3 | [具体任务] | [姓名] | YYYY-MM-DD | ⬜ 待开始 |

---

## 📝 备注

- [其他需要记录的内容]

---

> 本纪要由 AI 助手整理，如有遗漏请补充。
```

### Step 4: Confirm Target Repository

Ask the user which Yuque knowledge base to save to, or use `yuque_list_repos` to find it.

**Default to personal repos. Do NOT proactively ask for a team identifier!**

```
Tool: yuque_list_repos
Parameters:
  login: "<user_login>"
  type: "user"    # default to personal repos
```

If the user explicitly specifies a team/group knowledge base:

```
Tool: yuque_list_repos
Parameters:
  login: "<group_login>"
  type: "group"
```

If the user hasn't specified a repo:
1. List available personal repos and ask the user to pick one
2. If there's an obvious "会议纪要" or "Meeting Notes" repo, suggest it
3. If the user says "就放那个会议的库", match by name
4. Only use group repos when the user explicitly mentions a team name

### Step 5: Create the Document

```
Tool: yuque_create_doc
Parameters:
  repo_id: "<namespace>"    # e.g., "mygroup/mybook"
  title: "会议纪要：[主题] - YYYY-MM-DD"
  body: "<formatted markdown content>"
  format: "markdown"
```

### Step 6: Confirm to User

After creation, respond with:

```markdown
✅ 会议纪要已创建！

📄 **[会议纪要：主题 - 日期](文档链接)**
📚 已归档到：「知识库名称」

### 摘要
- 共讨论 X 个议题
- 形成 X 项决议
- 产生 X 个待办事项
```

## Guidelines

- Default to Chinese for the document content (Yuque users are primarily Chinese-speaking)
- Use emoji sparingly in section headers for visual scanning — as shown in the template
- Keep the document well-structured; prefer tables for action items
- If the user provides audio transcription, clean up filler words and organize by topic
- Preserve the user's original wording for decisions and action items — don't paraphrase important commitments

## Error Handling

| Situation | Action |
|-----------|--------|
| User provides very little info | Ask for at least: topic, attendees, key decisions |
| `yuque_list_repos` returns empty | Try personal repos first (`type: "user"`), then ask user for the exact repo name or ID |
| `yuque_create_doc` fails (403) | Tell user they may lack write permission to this repo |
| `yuque_create_doc` fails (other) | Show error, suggest user check yuque-mcp connection |
| No clear action items | Still create the doc, note "本次会议无明确待办事项" |

## Example

User: "帮我整理一下今天下午的会议。参加的有张三、李四、王五。讨论了新版本发布计划，决定下周三发布 v2.0，张三负责前端打包，李四负责后端部署，王五写发布公告。"

1. Extract: topic="新版本发布计划", date=today, attendees=张三/李四/王五
2. Structure: 1 agenda item, 1 decision (下周三发布 v2.0), 3 action items
3. Format using template
4. `yuque_list_repos` → find appropriate repo
5. `yuque_create_doc` → create document
6. Confirm with link and summary
