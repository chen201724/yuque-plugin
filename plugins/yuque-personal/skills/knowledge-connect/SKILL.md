---
name: knowledge-connect
description: Discover connections between documents, build knowledge networks, and establish bidirectional links across your personal Yuque knowledge base. For personal/individual use — operates on your own docs.
license: Apache-2.0
compatibility: Requires yuque-mcp server connected to a Yuque account with personal Token
metadata:
  author: chen201724
  version: "1.0"
---

# Knowledge Connect — Discover Document Relationships & Build Knowledge Networks

Help the user discover hidden connections between their documents, find related content, and build a knowledge network with bidirectional links across their personal Yuque knowledge base.

## When to Use

- User wants to find documents related to a specific topic
- User says "有哪些相关文档", "find related docs", "帮我建立知识关联"
- User wants to build a knowledge map or graph for a topic
- User says "这个主题还有哪些相关的", "帮我串联一下知识", "构建知识图谱"

## Required MCP Tools

All tools are from the `yuque-mcp` server:

- `yuque_search` — Search for related documents by keyword
- `yuque_get_doc` — Read document content to analyze connections
- `yuque_list_repos` — List personal repos to scan
- `yuque_list_docs` — List documents in repos for broader discovery
- `yuque_update_doc` — Add cross-reference links to documents
- `yuque_create_doc` — Create knowledge map documents

## Workflow

### Step 1: Identify the Starting Point

The user may provide:
- A specific document to find connections for
- A topic or keyword to explore
- A request to map an entire knowledge area

If starting from a document:

```
Tool: yuque_get_doc
Parameters:
  repo_id: "<namespace>"
  doc_id: "<slug>"
```

Extract key concepts, terms, and themes from the document.

### Step 2: Discover Related Documents

Search for related content using extracted keywords:

```
Tool: yuque_search
Parameters:
  query: "<keyword 1>"
  type: "doc"
```

Repeat with different keywords to cast a wider net. Use:
- Direct topic keywords
- Synonyms and related terms
- Key people or project names mentioned
- Technical terms and concepts

Also scan repos for broader discovery:

```
Tool: yuque_list_docs
Parameters:
  namespace: "<repo_namespace>"
```

### Step 3: Read and Analyze Connections

For each potentially related document (top 5-10):

```
Tool: yuque_get_doc
Parameters:
  repo_id: "<namespace>"
  doc_id: "<slug>"
```

Analyze the relationship type:

| Relationship | Description | Example |
|-------------|-------------|---------|
| 🔗 直接相关 | Same topic, different angle | 两篇都讲微服务架构 |
| 🧩 互补 | Fills gaps in each other | 一篇讲设计，一篇讲实现 |
| 📚 前置/后续 | Sequential knowledge | 入门篇 → 进阶篇 |
| 🔀 交叉引用 | Shared concepts across topics | 都提到了 Redis 缓存策略 |
| ⚡ 矛盾/对比 | Conflicting viewpoints | 两篇对同一问题有不同方案 |

### Step 4: Build the Knowledge Map

Present the discovered connections:

```markdown
# 🗺️ 知识关联图：[主题/文档标题]

> 基于「[起始文档]」发现的知识网络
> 扫描范围：X 个知识库，XX 篇文档
> 生成时间：YYYY-MM-DD

---

## 🎯 中心节点

**[起始文档标题](链接)**
- 知识库：[库名]
- 核心概念：[概念1]、[概念2]、[概念3]

---

## 🔗 关联文档

### 直接相关

| 文档 | 知识库 | 关联类型 | 关联说明 |
|------|--------|----------|----------|
| [标题](链接) | [库名] | 🔗 直接相关 | [为什么相关] |
| [标题](链接) | [库名] | 🧩 互补 | [互补点说明] |

### 延伸阅读

| 文档 | 知识库 | 关联类型 | 关联说明 |
|------|--------|----------|----------|
| [标题](链接) | [库名] | 📚 前置知识 | [说明] |
| [标题](链接) | [库名] | 🔀 交叉引用 | [共同概念] |

---

## 🧠 知识网络

```
[中心文档]
├── 🔗 [直接相关文档 1]
│   └── 🔀 [交叉引用文档 A]
├── 🧩 [互补文档 2]
├── 📚 [前置文档 3]
│   └── 📚 [更前置文档 B]
└── ⚡ [对比文档 4]
```

---

## 💡 发现与建议

- **知识聚类**：[发现的知识聚类模式]
- **知识缺口**：[发现缺少的关联文档或主题]
- **建议行动**：
  1. [建议创建的文档或补充的内容]
  2. [建议建立的新关联]

---

> 本知识图谱由 AI 助手自动生成，关联关系基于内容分析。
```

### Step 5: (Optional) Add Cross-References

If the user agrees, add "相关文档" sections to the connected documents:

```
Tool: yuque_update_doc
Parameters:
  repo_id: "<namespace>"
  doc_id: "<slug>"
  body: "<original content>\n\n---\n\n## 🔗 相关文档\n\n- [相关文档 1](链接) — [关联说明]\n- [相关文档 2](链接) — [关联说明]\n"
```

Ask before modifying any existing document:
- "要在这些文档中添加相互引用链接吗？"

### Step 6: (Optional) Save Knowledge Map

```
Tool: yuque_create_doc
Parameters:
  repo_id: "<namespace>"
  title: "🗺️ 知识图谱：[主题]"
  body: "<knowledge map content>"
  format: "markdown"
```

### Step 7: Confirm

```markdown
✅ 知识关联分析完成！

🗺️ **发现 X 篇相关文档，建立了 X 个关联**

### 关联概览
- 🔗 直接相关：X 篇
- 🧩 互补文档：X 篇
- 📚 前置/后续：X 篇
- 🔀 交叉引用：X 篇

💡 建议：[最重要的一条建议]
```

## Guidelines

- Start broad, then narrow — search with multiple keywords to find unexpected connections
- Quality over quantity — 5 strong connections are better than 20 weak ones
- Explain why documents are related, not just that they are
- Always ask before modifying existing documents (adding cross-references)
- The knowledge map should be actionable — include specific suggestions for strengthening the knowledge network
- Identify knowledge gaps — what's missing is as valuable as what's connected
- For large knowledge bases, focus on one topic area at a time
- Default language is Chinese

## Error Handling

| Situation | Action |
|-----------|--------|
| `yuque_search` returns few results | Broaden keywords; try synonyms and related terms |
| Starting document has no clear connections | Suggest the document may be on a new topic; offer to search broader |
| Too many connections found (>15) | Prioritize by relevance strength; group into clusters |
| `yuque_update_doc` fails when adding links | Skip that document; note it in the report |
| User's knowledge base is very small | Acknowledge limited scope; suggest topics to write about to build the network |
