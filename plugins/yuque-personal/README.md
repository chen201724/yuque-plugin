# 语雀个人版 Plugin / Yuque Personal Plugin

个人知识库 AI 集成 — 25 MCP Tools + 8 Skills。

语雀 = 第二大脑，Skills = AI 认知能力。

## Skills

| Skill | 类型 | 描述 |
|-------|------|------|
| `smart-search` | 知识检索 | 自然语言搜索个人语雀文档，智能摘要回答 |
| `smart-summary` | 知识输出 | 对任意文档/知识库生成不同粒度的摘要（一句话、要点、详细） |
| `reading-digest` | 知识输入 | 阅读文章后自动提取核心观点、金句、行动项，生成结构化阅读笔记 |
| `note-refine` | 知识加工 | 把粗糙笔记打磨成高质量文档，补充结构、优化表达、改善排版 |
| `meeting-notes` | 知识记录 | 从会议内容自动生成结构化会议纪要，保存到个人知识库 |
| `weekly-report` | 知识汇总 | 汇总一周文档活动，生成个人周报 |
| `tech-design` | 知识创作 | 根据需求生成技术方案文档，保存到个人知识库 |
| `stale-detector` | 知识维护 | 扫描知识库发现过期文档，生成健康报告，建议更新或归档 |

## 配置

需要设置 `YUQUE_PERSONAL_TOKEN` 环境变量：

```bash
export YUQUE_PERSONAL_TOKEN="your-personal-token"
```

获取方式：登录 [语雀](https://www.yuque.com) → 个人设置 → Token → 新建
