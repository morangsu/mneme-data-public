# mneme-data-public · 开放基础知识库

本仓库是 [mneme](https://github.com/morangsu/mneme) 的**公共基础数据库**。只放"所有人都能共用"的内容：领域 schema（`*.toml`）与基础条目数据（`*.csv`）。

个性化调整、学习进度、复习记录、同步日志一律**不进这里**，它们属于个人的私有库（`mneme-data-private`）。

## 目录约定

```
*.toml          领域包定义（schema）
*.csv           基础条目数据（可被多个领域包引用）
manifest.json   清单：版本号 + 文件列表 + 校验值
```

保持**扁平结构**：`mneme` 的命令行用 `--domains <dir>` 直接扫描该目录下的 `*.toml`，克隆到本地即可用，不需要额外配置。

## 领域包

| 文件 | 领域 | 说明 |
| --- | --- | --- |
| `neijing.toml` | 内经 | 以篇章为基座，原文→篇名、主题→篇名、篇名→释义的反向提取 |
| `materia-medica.toml` | 本草 | 药材性味归经、功效、配伍禁忌 |
| `botany.toml` | 植物学 | 科属、形态、生境、鉴别 |
| `dietetics.toml` | 食疗学 | 食材、性味、宜忌、搭配 |

每个领域包声明实体（`[[entities]]`）与字段（`[[entities.fields]]`），字段上的几个关键标记决定了它怎么被使用：

- `indexable` — 进倒排索引，支持"反向提取"（给出答案反查问题）
- `reversible` — 可作为出题方向（原文 ⇄ 篇名）
- `immutable` — 定稿后不可改，改动必须新建条目
- `weight` — 出题权重

## 怎么使用

```bash
# 在 mneme 工程目录下，把公共库克隆成它的 domains/
# （github.com 的 git 传输若不通，可用 API 下载 tarball，见文末）
git clone https://github.com/morangsu/mneme-data-public domains

# 生成公共包（维护者）
mneme upstream-publish --domains domains --out data/upstream/public-bundle.json --rev <n>

# 用户侧：把公共库内容拉进自己的私有库
mneme upstream-adopt --public data/upstream/public-bundle.json
mneme upstream-pull  --public data/upstream/public-bundle.json   # 只出 staging 计划，不写库
mneme upstream-conflicts --plan <plan-id>                        # 逐项裁决
```

**拉取规则**：公共库 → 私有库时，纯新增自动合并；凡是碰到你**已经改过的字段**，一律停下来交你决定（保留我的 / 采用上游 / 两者都留），裁决结果写进私有库的 `sync/upstream-decisions.jsonl`，可回溯。

## 数据格式

CSV 列与领域包字段一一对应，`id` 为稳定主键（同一 `id` 视为同一条目）。

多值字段用 `|` 分隔（如 `发汗解表|宣肺平喘`），不是 `、`——避免中文标点在全角/半角混排时被误判。

`rev` 是乐观锁版本号，每次改 `+1`；`deleted` 用于软删除。

## 贡献

1. 新增领域包：放一个 `*.toml`，`id` 唯一，字段尽量带 `indexable`/`reversible` 以支持反查与出题
2. 新增数据：CSV 表头必须覆盖该领域包声明的所有 `required` 字段
3. 提交前跑 `mneme import --domains . --csv <你的文件> --dry-run` 验证

## 许可

数据部分建议遵循 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)，代码/模板部分 MIT。具体见 `LICENSE`。
