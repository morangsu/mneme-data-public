# 针灸学（acupuncture）

**schema 能力密度最高**的学科：经络层级 + 腧穴归属关系 + 体表方位 + 图示标注 四者叠加。

## 文件

| 文件 | 说明 |
|---|---|
| `domain.toml` | 学科 schema（15 字段 / 5 出题方向） |
| `manifest.json` | 文件级 sha256 |
| `data/*.csv` | 基础条目数据 |
| `images/` | 穴位图（用户自备，仓库不内置） |

## 同时用到的 schema 能力

| 能力 | 在本学科的用途 |
|---|---|
| `group_kind = "meridian"` | 按经络分组视图 |
| `[[entities.relations]]` + `order` | 经络循行次序 |
| `spatial = true` + `spatial` 列 | 「中府在锁骨的下方」体表方位 |
| `kind = "image"` + `marks` 列 | 穴位在人体图上的位置标注 |
| `kind = "level"` + `levels` | **危险度分级**：安全 / 慎刺 / 禁深刺 / 禁针 |
| `cloze = true` | 挖空「定位」让人背 |
| `differential` | 邻近穴位鉴别（风池 vs 风府） |
| `immutable` | 定位、刺灸法、国际编号不可改 |

## 五种题型

实测同一批数据同时产出：定位推穴名、主治推穴位、背定位（挖空）、
图示指认（`atlas_locate.image`）、体表方位（`spatial_rel.spatial`）、
邻近穴鉴别（`differential.pair`）。

## 医学安全设计

- **危险穴**用 `danger` 分级标注（风池「禁深刺」，深刺伤延髓）；
- 定位与刺法设为 `immutable`，改一个字就可能扎错；
- 与 OCR 置信度透传联动：低置信识别到的定位会标红并要求人工核对。
