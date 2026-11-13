# 内经（neijing）

本目录是一门学科的**自包含单元**：领域包 + 发布包 + 校验清单 + 数据。

## 文件

| 文件 | 说明 |
|---|---|
| `domain.toml` | 学科 schema：实体、字段、出题方向、视图、约束 |
| `bundle.json` | 发布包（`mneme upstream-publish` 产物），供 `upstream-adopt` / `upstream-pull` 消费 |
| `manifest.json` | 本学科文件级 sha256 清单 |
| `data/` | 基础条目数据（CSV，可选） |

## 使用

```bash
# 作为公共库拉取（在 mneme 工程下）
mneme --data data --subject neijing upstream-pull --public <本目录>/bundle.json

# 或直接把本目录 clone 成 domains/neijing/
git clone <repo> domains   # 然后指向 domains/neijing/domain.toml
```

## 贡献

1. 改 `domain.toml` 的字段/方向/视图；
2. 改 `data/*.csv` 的条目（`id` 是稳定主键）；
3. 重跑 `mneme upstream-publish --out bundle.json --rev <n>` 并更新 `manifest.json`。
