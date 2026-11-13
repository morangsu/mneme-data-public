# mneme-data-public · 开放基础知识库

本仓库是 [mneme](https://github.com/morangsu/mneme) 的**公共基础数据库**，供医学生记忆辅助使用。
只放"所有人都能共用"的内容：各学科的领域定义（schema）与基础条目数据。

个性化调整、学习进度、复习记录、同步日志一律**不进这里**，它们属于个人的私有库。

## 目录结构（按学科分目录）

```
mneme-data-public/
├── index.json                 # 学科索引（id → 目录）
├── manifest.json              # 顶层清单（学科列表 + 版本）
├── LICENSE                    # MIT（代码/模板）+ CC BY-SA 4.0（数据）
├── materia-medica/            # 中药学
│   ├── README.md              # 本学科介绍
│   ├── domain.toml            # 学科 schema
│   ├── bundle.json            # 发布包
│   ├── manifest.json          # 文件级 sha256
│   └── data/sample-herbs.csv  # 基础数据
├── neijing/                   # 内经
├── botany/                    # 植物学
└── dietetics/                 # 食疗学
```

**每个学科目录是自包含单元**：领域包 + 发布包 + 校验清单 + 数据。新增学科只需新建一个目录，
不需要新仓库，也不需要改 mneme 的任何代码。

## 领域包字段标记

每个 `domain.toml` 声明实体（`[[entities]]`）与字段（`[[entities.fields]]`），关键标记：

- `indexable` — 进倒排索引，支持"反向提取"（给出答案反查问题）
- `reversible` — 可作为出题方向
- `immutable` — 定稿后不可改，改动必须新建条目（如中药的性味归经、内经原文）
- `weight` — 出题权重
- `required` — 必填

## 怎么使用

```bash
# 方式一：整个仓库克隆成本地公共库
git clone https://github.com/morangsu/mneme-data-public domains

# 方式二：只取某一学科（稀疏检出）
git clone --filter=blob:none --sparse https://github.com/morangsu/mneme-data-public domains
cd domains && git sparse-checkout set materia-medica

# 在 mneme 里按学科建库
mneme --data data --subject materia-medica init --domain domains/materia-medica/domain.toml

# 生成公共包（维护者）
mneme upstream-publish --out materia-medica/bundle.json --rev <n>

# 用户侧：把公共库内容拉进自己的私有库
mneme --data data --subject materia-medica upstream-adopt --public domains/materia-medica/bundle.json
mneme --data data --subject materia-medica upstream-pull  --public domains/materia-medica/bundle.json
mneme --data data --subject materia-medica upstream-conflicts --plan <plan-id>
```

**拉取规则**：公共库 → 私有库时，纯新增自动合并；凡是碰到你**已经改过的字段**，一律停下来交你决定
（保留我的 / 采用上游 / 两者都留），裁决结果写进私有库的 `sync/upstream-decisions.jsonl`，可回溯。

**学科护栏**：拉取时会校验 `bundle.schema_id` 与本地学科一致，拉错学科会直接报错，不会污染数据。

## 数据格式

CSV 列与领域包字段一一对应，`id` 为稳定主键（同一 `id` 视为同一条目）。

多值字段用 `|` 分隔（如 `发汗解表|宣肺平喘`），不是 `、`——避免中文标点在全角/半角混排时被误判。

`rev` 是乐观锁版本号，每次改 `+1`；`deleted` 用于软删除。

## 贡献

1. **新增学科**：新建目录 `<subject-id>/`，放 `domain.toml` + `README.md`，在 `index.json` 登记
2. **新增数据**：CSV 表头必须覆盖该领域包声明的所有 `required` 字段
3. **提交前验证**：`mneme --data /tmp/check --subject <id> init --domain <id>/domain.toml` 能成功加载
4. **更新发布包**：重跑 `upstream-publish` 并刷新该学科的 `manifest.json`

## 许可

代码/模板部分 MIT；数据部分 CC BY-SA 4.0。详见 `LICENSE`。

医学类内容仅为学习用途，不构成诊疗建议。
