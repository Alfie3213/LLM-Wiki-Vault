# Wiki Behavior Log

行为流水线：以 Grep-friendly 格式记录 ingest/query 历史。

## Format
```
[YYYY-MM-DD HH:MM:SS] [ACTION] [DESCRIPTION]
```

Actions: `INGEST`, `QUERY`, `LINT`, `ARCHIVE`, `INIT`

---

## [2026-05-06] ingest | 编译 CooHOI 论文

- **变更**: 新增 [[摘要-coohoi]]、[[CooHOI]]、[[CTDE]]、[[JiaweiGao]]; 更新 [[index.md]]
- **冲突**: 无
- **备注**: PDF 无法直接提取文本，通过 arXiv 元数据补全信息
