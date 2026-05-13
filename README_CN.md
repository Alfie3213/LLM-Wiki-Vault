# LLM Wiki Vault

一个基于 Obsidian 的学术文献阅读与研究笔记知识管理系统，聚焦自动驾驶、逆强化学习（IRL）与运动规划领域。

> [English version see README.md](README.md)

## 项目概述

本项目采用受 Zettelkasten 方法启发的分层知识架构，通过 Claude Skills 实现摄取、维护与查询的自动化。知识库针对深度论文阅读（L3 标准）优化，产出结构化的综合笔记。

## 目录结构

```
LLM-Wiki-Vault/
├── raw/                    # 待处理原始资料收件箱
│   ├── 01-articles/        # 网页剪藏文章
│   ├── 02-papers/          # PDF 论文与学术文献
│   ├── 03-transcripts/     # 视频转录文案
│   ├── 04-meeting_notes/   # 会议笔记
│   └── 09-archive/         # 已处理归档文件
├── wiki/                   # 编译后的知识库
│   ├── concepts/           # 抽象概念、框架、方法
│   ├── entities/           # 人物、公司、工具、产品
│   ├── sources/            # 来源摘要与元数据
│   ├── syntheses/          # L3 深度精读笔记
│   ├── index.md            # 全局页面注册表
│   └── log.md              # 操作日志（仅追加）
├── .claude/skills/         # Claude 技能定义
└── .obsidian/              # Obsidian 配置
```

## 快速开始

### 1. 添加原始资料

将论文、文章或转录稿放入 `raw/` 下的对应子目录。

### 2. 摄取到知识库

触发 ingest 技能将原始资料编译到 wiki：

```
/ingest
```

或处理指定文件：

```
/ingest raw/02-papers/my-paper.pdf
```

该操作将：
- 提取核心概念与实体
- 在 `wiki/sources/` 创建来源摘要
- 生成/更新概念与实体页面
- 更新 `wiki/index.md` 与 `wiki/log.md`
- 将源文件移动到 `raw/09-archive/`

### 3. 查询知识库

```
/query MaxEnt IRL 和 SMIRL 的关键区别是什么？
```

### 4. 检查健康度

```
/lint
```

## 可用技能

### 核心知识管理

| 技能 | 触发词 | 说明 |
|-------|---------|-------------|
| **ingest** | `/ingest` 或 "导入这篇论文" | 将原始资料编译到 wiki，提取实体/概念，并归档源文件 |
| **lint** | `/lint`、`/scan`、`/health` 或 "检查知识库" | 检测死链、孤儿页面、未同步索引和知识冲突 |
| **query** | `/query` 或自然语言提问 | 查询知识库并基于 wiki 内容回答 |
| **generate-mocs** | `/generate-mocs` 或 "更新索引" | 重新生成索引页与内容地图 |

### 可视化与图表

| 技能 | 触发词 | 说明 |
|-------|---------|-------------|
| **mermaid-visualizer** | "画流程图" 或 "可视化" | 生成 Mermaid 图表（流程图、思维导图、时序图等） |
| **excalidraw-diagram** | "Excalidraw"、"画图"、"流程图"、"standard excalidraw"、"animate" | 生成 Excalidraw 图表（Obsidian/.md、标准/.excalidraw、动画三种模式） |
| **obsidian-canvas-creator** | "创建 canvas"、"mind map"、"思维导图" | 创建 Obsidian Canvas 文件（思维导图或自由布局） |

### Obsidian 集成

| 技能 | 触发词 | 说明 |
|-------|---------|-------------|
| **obsidian-cli** | "obsidian" 命令 | 通过 CLI 与运行中的 Obsidian 交互（读取、创建、搜索笔记） |
| **obsidian-markdown** | 编辑 `.md` 文件时 | 创建和编辑 Obsidian 风格 Markdown（双链、标注、属性、嵌入） |
| **obsidian-bases** | 编辑 `.base` 文件时 | 创建类数据库视图，支持筛选、公式和汇总 |

### 工具

| 技能 | 触发词 | 说明 |
|-------|---------|-------------|
| **defuddle** | "提取网页" 或 "extract from URL" | 使用 Defuddle CLI 从网页提取干净 Markdown |
| **scholar-skill** | "读论文"、"L3 reading"、"精读" | 结构化论文阅读，支持 L1/L2/L3 三级标准与知识抽取 |

### 论文流水线（每日论文）

| 技能 | 触发词 | 说明 |
|-------|---------|-------------|
| **daily-papers-fetch** | "论文抓取" 或 "fetch papers" | 抓取并富化最新 arXiv/HuggingFace 论文 |
| **daily-papers-review** | "论文点评" 或 "review papers" | 生成带点评的精选推荐 |
| **daily-papers-notes** | "批量笔记" 或 "batch notes" | 为推荐论文生成结构化笔记 |
| **daily-papers** | "今日论文推荐" 或 "today's papers" | 端到端每日论文推荐流水线 |

## 使用示例

### 导入论文

```
请将 raw/02-papers/TreeIRL.pdf 这篇论文导入知识库
```

### 创建图表

```
画一个 TreeIRL 流程图
```

### 深度阅读

```
用 L3 标准精读 raw/02-papers/DIPP.pdf
```

### 可视化 Canvas

```
把 TreeIRL 的综合笔记转成思维导图 Canvas
```

### 检查知识库健康度

```
/scan
```

## 命名规范

- **实体**（人物、公司）：`TitleCase`（例：`ZhiyuHuang.md`、`Motional.md`）
- **概念**（方法、框架）：`TitleCase`（例：`TreeIRL.md`、`MaximumEntropyIRL.md`）
- **来源**（摘要）：`kebab-case`，前缀 `摘要-`（例：`摘要-treeirl-safe-urban-driving.md`）
- **综合**（精读笔记）：`YYYY-作者-标题-短横线连接.md`

## 关键依赖

- **Obsidian**：桌面端知识库应用
- **Obsidian Excalidraw 插件**：用于 `.md` 格式的 Excalidraw 图表
- **Defuddle CLI**：`npm install -g defuddle`（网页内容提取）
- **Obsidian CLI**：命令行仓库操作

## 注意事项

- 所有 wiki 内容使用简体中文编写
- `raw/09-archive/` 目录在摄取过程中**绝对禁止读取**
- 每个 wiki 页面必须包含 `## 关联连接` 区域，避免产生孤儿页面
- `wiki/index.md` 作为全局注册表，由 skills 自动更新
