---
name: scholar-skill
description: >
  基于 Obsidian 的 L3 级论文阅读与记忆抽取系统。
  将论文阅读转化为结构化笔记、可复用知识单元和不断演化的知识网络。
  支持三级阅读标准（L1 快速筛选 / L2 标准阅读 / L3 精读）、
  记忆抽取（Semantic / Episodic / Procedural）、
  知识关联与 MOC 更新、反思与确认机制。
  触发词："阅读论文"、"精读"、"L1/L2/L3 阅读"、"论文笔记"、"知识内化"。
user-invocable: true
---

# ScholarSkill - 学术论文阅读与知识内化

## 核心定位

ScholarSkill 是面向学术阅读的工作流技能。目标不是简单的摘要生成，而是：
- 将阅读转化为结构化笔记（而非一次性聊天回复）
- 将新论文连接到已有知识网络
- 在新证据出现时修订旧理解
- 鼓励比较、验证和批判性反思
- 让知识体系随时间不断变强

## 触发词

- "阅读论文"
- "精读"
- "L1/L2/L3 阅读"
- "论文笔记"
- "知识内化"
- "配置 ScholarSkill"

## 适用场景

- 学术论文阅读（ArXiv/会议/期刊）
- 技术博客深度阅读
- 文献综述整理
- 研究领域调研

## 核心能力

### 1. 三级阅读标准

#### L1 快速筛选 (5 分钟)
- 阅读标题、摘要、图表
- 输出：一句话概括 + 优先级评级 (P0/P1/P2)
- 适用：P2 论文筛选

#### L2 标准阅读 (45 分钟)
- 阅读引言、方法、实验、结论
- 输出：3-5KB 笔记 + 5-8 条记忆
- 适用：P1 论文（日常标准）

#### L3 精读 (2.5 小时)
- 完整方法细节、实验分析、补充材料
- 输出：10-15KB 笔记 + 知识升级 + 2-3 条程序规则
- 适用：P0 论文（核心相关）

### 2. 记忆抽取

**Semantic Memory**: 事实、概念、方法、结论
**Episodic Memory**: 疑问、误解、修正过程
**Procedural Memory**: 可复用的阅读规则和研究方法

### 3. 知识管理

- Obsidian 双向链接
- MOC 知识地图
- 原子化记忆存储
- 周巩固机制

### 4. 反思与确认机制

- **L1 反思**：每篇论文后快速检查理解、疑问和下一步行动
- **L2 反思**：周度回顾知识增长、缺失链接、研究方向和风险
- **L3 反思**：月度评估知识演化、方向调整和认知修正
- **人工确认**：新 MOC、核心论文、认知冲突、方向调整等关键事项进入 0-Inbox/

## 工作流程

```
1. 接收论文 (PDF/ArXiv URL/本地文件)
↓
2. 优先级评估 (P0/P1/P2)
↓
3. 选择阅读级别 (L1/L2/L3)
↓
4. 执行阅读 + 笔记生成
↓
5. 记忆抽取 (Semantic/Episodic/Procedural)
↓
6. 更新 2-Knowledge/（Concept / Insight / Method / Question / Person）
↓
7. 知识关联 (双向链接 + MOC 更新)
↓
8. 反思 / 确认 / 输出报告
```

## 目录结构（在 Obsidian Vault 中）

```
wiki/                         # 知识库根目录（与 Lingma 环境的实际结构对应）
├── sources/                  # 来源摘要（ingest 技能生成的原始资料摘要）
│   └── 摘要-{slug}.md
├── concepts/                 # 概念页面（框架、方法论、理论）
│   └── {ConceptName}.md
├── entities/                 # 实体页面（人物、公司、工具、产品）
│   └── {EntityName}.md
├── syntheses/                # 综合层（L2/L3 论文精读笔记、文献综述、深度分析）
│   └── {Year}-{Author}-{Title}.md
├── index.md                  # 全局内容字典
└── log.md                    # 操作日志（Append-only）

模板目录: .lingma/skills/scholar-skill/templates/
```

## 输出物

| 输出 | 位置 |
|------|------|
| L2/L3 论文精读笔记 | `wiki/syntheses/{Year}-{Author}-{Title}.md` |
| 来源摘要 | `wiki/sources/摘要-{slug}.md`（由 ingest 技能维护） |
| 概念卡片 | `wiki/concepts/{ConceptName}.md` |
| 实体卡片 | `wiki/entities/{EntityName}.md` |
| 全局索引 | `wiki/index.md` |
| 操作日志 | `wiki/log.md` |

### Frontmatter 规范

所有生成的 `.md` 文件必须包含统一的 YAML frontmatter，确保与 wiki 索引和排序工具兼容：

**L2/L3 论文精读笔记**:
```yaml
---
type: paper-note
level: L3
paper_title: ""
venue: ""
date: "YYYY-MM-DD"
authors: ""
fields: []
paper_type: ""
reading_date: "YYYY-MM-DD"
priority: P0|P1|P2
sources: [raw/09-archive/{Archived-Filename}.pdf]
last_updated: "YYYY-MM-DD"
---
```

**概念页面 (Concept)**:
```yaml
---
title: ""
type: concept
tags: []
sources: [raw/09-archive/{Archived-Filename}.pdf]
last_updated: "YYYY-MM-DD"
---
```

**实体页面 (Entity)**:
```yaml
---
title: ""
type: entity
tags: []
sources: [raw/09-archive/{Archived-Filename}.pdf]
last_updated: "YYYY-MM-DD"
---
```

**关键规则**:
- `sources`: 指向 `raw/09-archive/` 中的归档 PDF（若由 ingest 技能归档），而非 `raw/02-papers/`
- `last_updated`: 文件内容发生任何修改时必须更新为当前日期
- `type`: 严格使用 `paper-note` | `concept` | `entity` | `source`

## 使用示例

### 示例 1: L2 标准阅读

```
用户：请用 L2 级别阅读这篇论文
附件：paper.pdf

智能体:
1. 评估优先级 → P1
2. 执行 L2 阅读（45 分钟标准）
3. 生成笔记（3-5KB）
4. 抽取 5-8 条记忆
5. 需要时创建 Concept / Insight / Question / Method 卡片
6. 更新知识关联与 MOC
7. 生成 L1 反思
8. 输出报告
```

### 示例 2: L3 精读

```
用户：请以 L3 级别精读 ArXiv:2407.19354

智能体:
1. 获取论文（ArXiv API 或本地 PDF）
2. 评估优先级 → P0（与核心方向直接相关）
3. 执行 L3 阅读（2.5 小时标准）
4. 生成深度笔记（10-15KB），存放于 `wiki/syntheses/`
5. 知识升级 + 旧知识修订
6. 提炼 2-3 条程序规则
7. 更新 `wiki/concepts/` 和 `wiki/entities/` 中的相关页面（增量合并新信息）
8. 更新 `wiki/index.md` 和 `wiki/log.md`
9. 若出现冲突/新方向，向用户报告并询问处理方式
10. 生成深度反思并推送详细报告
```

### 示例 3: 批量阅读

```
用户：这 10 篇论文请先 L1 筛选，然后对其中的 P0/P1 论文执行 L2 阅读

智能体:
1. 批量 L1 筛选（5 分钟/篇）
2. 评级分类：P0(2 篇) + P1(5 篇) + P2(3 篇)
3. 对 P0 论文执行 L3 阅读
4. 对 P1 论文执行 L2 阅读
5. P2 论文仅存档
6. 周末执行 L2 反思并整理概念 / MOC
7. 输出汇总报告
```

## 质量检查清单

### L2 检查
- [ ] 标准笔记完整（3-5KB）
- [ ] Semantic Memories 3-5 条
- [ ] Episodic Memories 1-2 条
- [ ] Procedural Memories 1 条候选
- [ ] 每条记忆有 action 字段
- [ ] 笔记有双向链接
- [ ] MOC 已更新

### L3 检查
- [ ] 深度笔记完整（10-15KB）
- [ ] Semantic Memories 5-8 条（含常驻记忆块）
- [ ] Episodic Memories 2-3 条
- [ ] Procedural Memories 2-3 条
- [ ] 知识升级记录（旧知识修订表）
- [ ] 批判性分析完整
- [ ] 行动建议具体

## 配置文件参考

```yaml
# Lingma 环境下的实际路径配置
wiki:
  vault_path: /Users/didi/Documents/LLM-Wiki-Vault
  sources_folder: wiki/sources          # 来源摘要（ingest 生成）
  concepts_folder: wiki/concepts        # 概念页面
  entities_folder: wiki/entities        # 实体页面
  syntheses_folder: wiki/syntheses      # L2/L3 精读笔记
  index_file: wiki/index.md
  log_file: wiki/log.md
  templates_folder: .lingma/skills/scholar-skill/templates

reading:
  default_level: L2
  enable_memory_extraction: true
  enable_knowledge_consolidation: true
```

## 依赖说明

本技能原为 OpenClaw 设计，部分功能在 Lingma 环境中的对应方案：

| 原依赖 | 用途 | Lingma 替代方案 |
|--------|------|----------------|
| obsidian-direct | Obsidian 文件操作 | 使用 obsidian-cli skill 或直接文件操作 |
| arxiv-watcher | ArXiv 论文搜索 | 使用 search_web 或 daily-papers-fetch skill |
| academic-research-hub | 多源学术搜索 | 使用 search_web |
| tavily | 网页内容提取 | 使用 fetch_content |
| pdf | PDF 文本提取 | 使用内置文件读取工具 |
| durable-task-runner | 长任务编排 | Lingma 会话内顺序执行 |

## 模板文件

所有模板位于 `templates/` 目录：
- Template-Paper-Note-L2.md — L2 标准阅读笔记模板
- Template-Paper-Note-L3.md — L3 精读笔记模板
- Template-Concept.md — 概念卡片模板
- Template-Insight.md — 洞察卡片模板
- Template-Method.md — 方法卡片模板
- Template-Question.md — 问题卡片模板
- Template-Person.md — 研究者卡片模板
- Template-MOC.md — 知识地图模板
- Template-Reflection-L1.md — L1 单篇反思模板
- Template-Reflection-L2.md — L2 周度反思模板
- Template-Reflection-L3.md — L3 月度反思模板
- Template-Confirmation-Request.md — 人工确认请求模板
- Procedure-人工确认流程.md — 人工确认流程说明

## 参考文档

- docs/CONFIGURATION.md — 配置指南
- docs/DURABLE-INTEGRATION.md — 长任务编排集成说明
- docs/INTEGRATION-SUMMARY.md — 集成总结
