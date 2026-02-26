---
name: yuque-personal-style-extract
description: Analyze the writing style of Yuque documents and extract style characteristics including structure, tone, vocabulary, and formatting patterns. For personal/individual use — analyzes your own documents.
license: Apache-2.0
compatibility: Requires yuque-mcp server connected to a Yuque account with personal Token
metadata:
  author: chen201724
  version: "2.0"
---

# Style Extract — Yuque Document Writing Style Analysis

Analyze one or more Yuque documents to extract writing style characteristics, helping users learn from excellent documents or maintain consistent writing style.

## When to Use

- User wants to analyze the writing style of a specific document
- User says "分析一下这篇文档的写作风格", "extract the style", "学习这篇文档的风格"
- User wants to maintain consistent style across documents
- User says "帮我总结一下我的写作特点", "我想保持和这篇一样的风格"

## Required MCP Tools

All tools are from the `yuque-mcp` server:

- `yuque_search` — Search documents by keyword to find target documents
- `yuque_get_doc` — Read full document content for style analysis
- `yuque_list_repos` — List personal knowledge bases to browse documents

## Workflow

### Step 1: Identify Target Documents

The user may provide:
- A specific document title or link
- A keyword to search for the document
- A request to analyze their overall writing style (multiple docs)

If a specific document is given:

```
Tool: yuque_get_doc
Parameters:
  repo_id: "<namespace>"
  doc_id: "<slug>"
```

If the user gives keywords, search first:

```
Tool: yuque_search
Parameters:
  query: "<keywords>"
  type: "doc"
```

For overall style analysis, read 3-5 recent documents from the user's repos.

### Step 2: Analyze Style Dimensions

Examine the document(s) across these dimensions:

| Dimension | What to Look For |
|-----------|-----------------|
| 📐 结构 (Structure) | Heading hierarchy, section organization, use of lists vs paragraphs |
| 🎯 语气 (Tone) | Formal/informal, technical/conversational, authoritative/collaborative |
| 📝 用词 (Vocabulary) | Technical depth, jargon usage, Chinese/English mixing patterns |
| 📏 篇幅 (Length) | Average section length, paragraph density, overall document length |
| 🎨 格式 (Formatting) | Use of tables, code blocks, images, callouts, emoji |
| 🔗 引用 (References) | How sources are cited, use of links, cross-references |
| 💡 表达 (Expression) | Use of examples, analogies, rhetorical questions, humor |

### Step 3: Extract Style Profile

Compose a style profile with concrete examples:

```markdown
## 📊 写作风格分析报告

### 文档信息
- **文档**：[标题](链接)
- **知识库**：「知识库名称」
- **字数**：约 X 字
- **更新时间**：YYYY-MM-DD

---

### 📐 结构特征

- **层级**：[如：使用 H2/H3 两级标题，不使用 H4]
- **组织方式**：[如：总分总结构，先给结论再展开]
- **段落长度**：[如：每段 2-4 句，简洁明了]

### 🎯 语气与风格

- **整体基调**：[如：专业但不刻板，偶尔使用口语化表达]
- **人称使用**：[如：多用"我们"，营造协作感]
- **典型句式**：[引用 1-2 个代表性句子]

### 📝 用词特点

- **术语密度**：[高/中/低]
- **中英混用**：[如：技术名词保留英文，其余用中文]
- **高频词汇**：[列出 5-8 个特征性词汇]

### 🎨 格式偏好

- **常用元素**：[如：大量使用表格、代码块较少、喜欢用 emoji 做标记]
- **视觉节奏**：[如：每 2-3 段插入一个列表或表格，避免大段纯文字]

### 💡 表达手法

- **举例方式**：[如：喜欢用实际场景举例，常用"比如说..."]
- **逻辑连接**：[如：善用"首先/其次/最后"，过渡自然]

---

### 🎯 风格摘要（一句话）

> [用一句话概括这个写作风格，如："专业严谨但不失亲和力的技术文档风格，善用结构化表达和实例说明。"]

### 📋 风格复用建议

如果你想模仿这个风格写作，注意以下要点：
1. [具体建议 1]
2. [具体建议 2]
3. [具体建议 3]
```

### Step 4: Compare Styles (Optional)

If the user provides multiple documents for comparison:

```markdown
## 📊 风格对比

| 维度 | 文档 A | 文档 B |
|------|--------|--------|
| 语气 | [特征] | [特征] |
| 结构 | [特征] | [特征] |
| 用词 | [特征] | [特征] |
| 格式 | [特征] | [特征] |

### 共同点
- [共同特征 1]
- [共同特征 2]

### 差异点
- [差异 1]
- [差异 2]
```

## Guidelines

- Always answer in the same language the user used (Chinese or English)
- Use concrete examples from the actual document — quote specific sentences or patterns
- Be objective and descriptive, not judgmental — "uses short paragraphs" not "paragraphs are too short"
- When analyzing multiple documents, identify both consistent patterns and variations
- The style profile should be actionable — someone should be able to write in a similar style after reading it
- This skill analyzes documents in personal repos — for team repos, use the corresponding skill in the `yuque-group` plugin

## Error Handling

| Situation | Action |
|-----------|--------|
| Document not found | Try alternative search keywords, then inform user |
| Document too short (<100 chars) | Inform user the document is too short for meaningful style analysis |
| `yuque_get_doc` fails (403) | Tell user they may lack permission to access this doc |
| API timeout | Retry once, then inform user of connectivity issue |
| User provides no specific document | List recent docs from their repos and ask which to analyze |
