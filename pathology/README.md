# 病理学（pathology）

形态导向：镜下特征 → 诊断。**同病异变、同变异病**是核心难点。

## 文件

| 文件 | 说明 |
|---|---|
| `domain.toml` | 学科 schema（13 字段 / 2 出题方向） |
| `manifest.json` | 文件级 sha256 |
| `data/*.csv` | 基础条目数据 |

## 用到的 schema 能力

| 能力 | 用途 |
|---|---|
| `scope = "category"` | 多病变共享镜下词时先按类别限定 |
| `enumerate_if_ambiguous` | 特征不唯一时列举并说差异 |
| `differential` | 鉴别诊断（细胞水肿 vs 脂肪变性） |
| `immutable` | 镜下特征不可改 |
| `cloze = true` | 挖空特征性病变 |

## 实测

5 个真实病变产出 31 张卡。`hallmark:朗汉斯巨细胞` 正确命中干酪样坏死。

## 数据格式注意

`microscopy` 是 `kind = "tags"`，倒排按**整词**索引，查询需用完整词
（如 `胞质内空泡` 而非子串 `空泡`）。
