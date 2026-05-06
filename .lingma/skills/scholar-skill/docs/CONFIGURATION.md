# ScholarSkill 配置指南

版本: v1.0.0
最后更新: 2026-03-18

## 配置流程总览

```
1. 决定 Vault 路径
↓
2. 创建目录结构
↓
3. 同步模板到模板目录
↓
4. 开始使用
```

## 目录结构

在 Obsidian Vault 中创建以下目录：

```
Vault/
├── 0-Inbox/                  # 确认请求、待处理事项
├── 1-Papers/                 # 论文笔记
│   ├── By-Topic/
│   ├── By-Year/
│   └── To-Process/
├── 2-Knowledge/              # 原子化知识
│   ├── Concepts/             # 概念卡片
│   ├── Insights/             # 洞察卡片
│   ├── Methods/              # 方法卡片
│   ├── Questions/            # 问题卡片
│   └── People/               # 研究者卡片
├── 3-MOCs/                   # 知识地图
├── 4-Outputs/                # 反思、确认记录、草稿
│   ├── Reflections/
│   │   ├── L1/
│   │   ├── L2/
│   │   └── L3/
│   ├── Confirmation-Records/
│   ├── Drafts/
│   └── Literature-Reviews/
├── 9-Templates/              # 模板目录
│   └── zh-CN/                # 中文版模板
└── memory/                   # 长期记忆
    ├── semantic/
    ├── episodic/
    └── inbox/
```

### 手动创建命令

```bash
cd /your/obsidian/vault
mkdir -p 0-Inbox
mkdir -p 1-Papers/By-Topic
mkdir -p 1-Papers/By-Year
mkdir -p 1-Papers/To-Process
mkdir -p 2-Knowledge/Concepts
mkdir -p 2-Knowledge/Insights
mkdir -p 2-Knowledge/Methods
mkdir -p 2-Knowledge/Questions
mkdir -p 2-Knowledge/People
mkdir -p 3-MOCs
mkdir -p 4-Outputs/Reflections/L1
mkdir -p 4-Outputs/Reflections/L2
mkdir -p 4-Outputs/Reflections/L3
mkdir -p 4-Outputs/Confirmation-Records
mkdir -p 9-Templates/zh-CN
mkdir -p memory/semantic
mkdir -p memory/episodic
mkdir -p memory/inbox
```

## 配置文件说明

配置文件路径（可选，如不使用可跳过）：

```yaml
obsidian:
  vault_path: /your/Obsidian/Vault
  inbox_folder: 0-Inbox
  paper_notes_folder: 1-Papers
  knowledge_folder: 2-Knowledge
  concepts_folder: 2-Knowledge/Concepts
  insights_folder: 2-Knowledge/Insights
  methods_folder: 2-Knowledge/Methods
  questions_folder: 2-Knowledge/Questions
  people_folder: 2-Knowledge/People
  moc_folder: 3-MOCs
  outputs_folder: 4-Outputs
  reflections_folder: 4-Outputs/Reflections
  confirmation_records_folder: 4-Outputs/Confirmation-Records
  templates_folder: 9-Templates/zh-CN
  memory_folder: memory

reading:
  default_level: L2
  enable_memory_extraction: true
  enable_knowledge_consolidation: true

priority:
  p0_keywords:  # 核心方向（L3 精读）
    - "CAD Generation"
    - "Physical AI"
    - "World Model"
  p1_keywords:  # 相关方向（L2 标准）
    - "LLM Agent"
    - "Reasoning"
    - "3D Generation"
  p2_keywords:  # 边缘方向（L1 筛选）
    - "其他方向"

reflection:
  enable_reflection: true
  l1_after_each_paper: true
  l2_schedule: weekly
  l3_schedule: monthly

confirmation:
  enable_human_confirmation: true
  trigger_on_new_moc: true
  trigger_on_core_paper: true
  trigger_on_cognitive_conflict: true
```

## 关键配置项

### 1. Obsidian 路径（必填）

```yaml
obsidian:
  vault_path: /Users/your-username/ObsidianVault
```

如何找到你的 Obsidian 路径：
1. 打开 Obsidian
2. 点击左下角 "Open another vault"
3. 右键当前仓库 → "Copy path"

### 2. 阅读级别（可选）

```yaml
reading:
  default_level: L2  # L1(快速) / L2(标准) / L3(精读)
```

### 3. 优先级关键词（推荐自定义）

根据你的研究方向修改这些关键词！

```yaml
priority:
  p0_keywords:
    - "你的核心方向1"
    - "你的核心方向2"
  p1_keywords:
    - "相关方向1"
    - "相关方向2"
```

### 4. 反思与确认（推荐保留默认）

```yaml
reflection:
  enable_reflection: true
  l1_after_each_paper: true
  l2_schedule: weekly
  l3_schedule: monthly

confirmation:
  enable_human_confirmation: true
  trigger_on_new_moc: true
  trigger_on_core_paper: true
  trigger_on_cognitive_conflict: true
```

## 模板同步

将 skill 自带模板复制到 Obsidian 仓库的 `9-Templates/zh-CN/`：

- Template-Paper-Note-L2.md
- Template-Paper-Note-L3.md
- Template-Reflection-L1.md
- Template-Reflection-L2.md
- Template-Reflection-L3.md
- Template-Confirmation-Request.md
- Template-Concept.md
- Template-Insight.md
- Template-Method.md
- Template-Question.md
- Template-Person.md
- Template-MOC.md
- Procedure-人工确认流程.md

## 故障排除

### 问题 1: 找不到 Obsidian 仓库

解决:
1. 手动输入完整路径
2. 如路径不存在，先创建目录：mkdir -p /path/to/vault

### 问题 2: 模板同步失败

解决:
手动复制模板文件到 `9-Templates/zh-CN/`

### 问题 3: 想重新配置

解决:
删除配置文件后重新设置，或直接修改目录结构。
