# LLM Wiki Vault

一个用于深度学术研究的知识库，核心理念是：LLM 应该**维护**知识，而不仅仅是检索它。深受 [Andrej Karpathy 的 LLM Wiki 模式](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)启发。

> [English version see README.md](README.md)

## 核心思想

大多数人与 LLM 协作处理文档的体验都像 RAG：上传文件，LLM 检索片段，生成答案。这能工作，但每次提问时 LLM 都在从零重新发现知识，没有积累。当你问一个需要综合五篇论文的微妙问题时，LLM 必须每次都去寻找并拼凑碎片。

这里的想法不同。不是仅在查询时从原始文档中检索，而是让 LLM**增量地构建并维护一个持久的 wiki**——一个结构化的、相互关联的 Markdown 文件集合，位于你和原始资料之间。当你添加一篇新论文时，LLM 不只是索引它。它会阅读、提取关键信息，并将其整合到现有 wiki 中——更新实体页面、修订概念摘要、标注矛盾之处、强化不断演进的综合。知识被编译一次，然后*保持最新*，而不是每次查询都重新推导。

这是关键区别：**wiki 是一个持久的、复合的工件。**交叉引用已经存在。矛盾已经被标记。综合已经反映了你所阅读的一切。每增加一个来源、每提出一个问题，wiki 都会变得更加丰富。

你从不（或很少）亲自写 wiki——LLM 通过 [Claude Skills](.claude/skills/) 完成所有撰写和维护工作。你负责寻找资料、探索和提出正确的问题。LLM 做所有繁重的工作——总结、交叉引用、归档和 bookkeeping。实践中，我一边是 LLM 助手，一边是 Obsidian。LLM 根据我们的对话进行编辑，我实时浏览结果——跟随链接、检查图谱视图、阅读更新后的页面。**Obsidian 是 IDE；LLM 是程序员；wiki 是代码库。**

这个特定的知识库聚焦于自动驾驶、逆强化学习（IRL）和运动规划，但该模式适用于任何需要长期积累知识的领域。

## 架构

三层架构：

### 原始资料（Raw Sources）

你精心整理的源文档集合——PDF 论文、网页文章、视频转录、会议笔记。这些是不可变的。LLM 读取它们但永不修改。这是你唯一的事实来源。

```
raw/
├── 01-articles/        # 网页剪藏文章
├── 02-papers/          # PDF 论文与学术文献
├── 03-transcripts/     # 视频转录文案
├── 04-meeting_notes/   # 会议笔记
└── 09-archive/         # 已处理归档文件（永不读取）
```

### 知识库（The Wiki）

LLM 生成的 Markdown 文件目录。摘要、实体页面、概念页面、综合笔记。LLM 完全拥有这一层。它创建页面、在新资料到达时更新它们、维护交叉引用、保持一切一致。你负责阅读；LLM 负责写作。

```
wiki/
├── concepts/           # 抽象概念、框架、方法
├── entities/           # 人物、公司、工具、产品
├── sources/            # 来源摘要与元数据
├── syntheses/          # L3 深度精读笔记
├── index.md            # 全局页面注册表
└── log.md              # 操作日志（仅追加）
```

### 模式文档（The Schema）

[`CLAUDE.md`](CLAUDE.md) 告诉 LLM wiki 如何结构化、约定是什么、摄取资料或维护 wiki 时应遵循什么流程。这是关键的配置文件——它让 LLM 成为一个有纪律的 wiki 维护者，而不是一个普通的聊天机器人。你和 LLM 会随着时间共同演化这份文档。

## 核心操作

### 摄取（Ingest）

将新资料放入 `raw/`，然后让 LLM 处理它：

```
/ingest
```

或摄取指定文件：

```
/ingest raw/02-papers/TreeIRL.pdf
```

LLM 阅读资料、讨论关键收获、在 `wiki/sources/` 中撰写摘要、更新 wiki 中相关的实体和概念页面、并向 `wiki/log.md` 追加条目。单个来源可能触及 10-15 个 wiki 页面。确认后，源文件被移动到 `raw/09-archive/`。

我倾向于一次摄取一个来源并保持参与——阅读摘要、检查更新、引导 LLM 该强调什么。

### 查询（Query）

向 wiki 提问。LLM 搜索相关页面、阅读它们、综合出带引用的答案：

```
/query MaxEnt IRL 和 SMIRL 的关键区别是什么？
```

好的答案可以作为新页面归档回 wiki——一个比较、一个分析、一个你发现的联系。这样你的探索就像摄取的资料一样，在知识库中不断复合积累。

### 检查（Lint）

定期对 wiki 进行健康检查：

```
/lint
```

LLM 扫描死链、孤儿页面、未注册页面和知识冲突。这能让 wiki 在增长过程中保持健康。

## Claude Skills

Skills 是可复用的提示词，教会 LLM 如何执行特定任务。本知识库包含：

| 类别 | 技能 | 功能 |
|----------|-------|--------------|
| 核心 | **ingest** | 将原始资料编译到 wiki，提取实体/概念，归档 |
| 核心 | **query** | 通过阅读 wiki 页面并综合回答来回答问题 |
| 核心 | **lint** | 对 wiki 进行健康检查（死链、孤儿、冲突） |
| 核心 | **generate-mocs** | 重新生成索引页和内容地图 |
| 阅读 | **scholar-skill** | 结构化论文阅读，支持 L1/L2/L3 三级深度标准 |
| 可视化 | **mermaid-visualizer** | 从文本生成 Mermaid 图表 |
| 可视化 | **excalidraw-diagram** | 生成 Excalidraw 图表（Obsidian/标准/动画三种模式） |
| 可视化 | **obsidian-canvas-creator** | 生成 Obsidian Canvas 文件（思维导图或自由布局） |
| Obsidian | **obsidian-cli** | 通过命令行与运行中的 Obsidian 实例交互 |
| Obsidian | **obsidian-markdown** | Obsidian 风格 Markdown 语法指导 |
| Obsidian | **obsidian-bases** | 类数据库视图，支持筛选、公式、汇总 |
| 工具 | **defuddle** | 从网页提取干净 Markdown |
| 流水线 | **daily-papers** | 端到端每日 arXiv 论文抓取、点评与笔记生成 |

通过 `/skillname` 或自然语言触发技能（例如"导入这篇论文"、"画个流程图"）。

## 索引与日志

两个特殊文件帮助在 wiki 增长时进行导航：

**[`wiki/index.md`](wiki/index.md)** 是内容导向的——每个页面的目录，附一句话摘要，按类别组织（概念、实体、来源、综合）。LLM 在每次摄取时更新它。回答查询时，LLM 先读索引找到相关页面，再深入阅读。

**[`wiki/log.md`](wiki/log.md)** 是时间线导向的——摄取、查询、检查的仅追加记录。每个条目以统一前缀开头（例如 `## [2026-04-02] ingest | 文章标题`），使其可以用简单工具解析。

## 命名规范

- **实体**（人物、公司）：`TitleCase` — `ZhiyuHuang.md`、`Motional.md`
- **概念**（方法、框架）：`TitleCase` — `TreeIRL.md`、`MaximumEntropyIRL.md`
- **来源**（摘要）：`kebab-case`，前缀 `摘要-` — `摘要-treeirl-safe-urban-driving.md`
- **综合**（精读笔记）：`YYYY-作者-标题-短横线连接.md`

## 技巧与建议

- **Obsidian Web Clipper** 是一个浏览器扩展，可将网页文章转换为 Markdown。非常适合快速将来源放入 `raw/01-articles/`。
- **Obsidian 的图谱视图** 是观察 wiki 形状的最佳方式——什么与什么相连、哪些页面是枢纽、哪些是孤儿。
- **scholar-skill** 支持三种阅读深度：L1（快速筛选）、L2（标准阅读）、L3（深度阅读，含完整方法推导、批判性反思和知识抽取）。我对聚焦领域的论文默认使用 L3。
- 所有 wiki 内容使用简体中文编写，即使触发词和技能可能接受英文。
- `raw/09-archive/` 目录在摄取过程中永不读取——它是一个仅写入的归档。
- 每个 wiki 页面必须包含 `## 关联连接` 区域，避免产生孤儿页面。
