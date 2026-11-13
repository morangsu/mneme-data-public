# 方剂学（formula）

类方做基座，组成/功用/主治互为线索；剂量与配伍属危险区。

## 文件

| 文件 | 说明 |
|---|---|
| `domain.toml` | 学科 schema |
| `manifest.json` | 文件级 sha256 清单 |
| `data/*.csv` | 基础条目数据 |

## 数据规模（当前为验证用样例）

1 个 CSV 文件。

## 已知 schema 缺口（见 docs/ROADMAP.md §3b）

- 关系带属性（君臣佐使角色、单药剂量）需要 `[[relations]]`（P2-5）
- 分级字段（不良反应 I–IV 级）需要 `level` 类型（P2-9）
