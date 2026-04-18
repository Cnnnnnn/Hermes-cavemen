# Hermite Caveman — Terse Mode

> 🪨 why use many token when few token do trick

**English | [中文](#中文)**

---

## What is this?

Adaptation of [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) for **Hermes Agent / OpenClaw**. Cuts output tokens ~75% while keeping full technical accuracy.

**Terse Mode is ON by default.** It compresses every reply automatically. No setup needed beyond copying the rules.

---

## Before / After

**Normal (87 tokens):**
> "当然！我很高兴帮你解决这个问题。你遇到的问题很可能是由于认证中间件没有正确验证 token 过期时间导致的。让我看一下并建议一个修复方案。"

**Terse - full (24 tokens):**
> "认证中间件 bug。Token 过期检查用了 < 而不是 <=。修："

**Same answer. 75% less word. Brain still big.**

---

## Two Ways to Install / 两种安装方式

### Option A — Copy the Terse Mode section (recommended) / 推荐方式
Copy only the `## Terse Mode` section from `SOUL.md` and append it to your existing `SOUL.md` file.

This keeps your existing SOUL.md content intact and only adds the Terse Mode rules.

**Steps:**
1. Open [`SOUL.md`](SOUL.md) in this repo
2. Find the `## Terse Mode` section (lines 25–63)
3. Copy that section into your Hermes `SOUL.md` file

---

### Option B — Replace your SOUL.md entirely / 完全覆盖方式
If your `SOUL.md` is empty or you want everything in one file, copy the entire [`SOUL.md`](SOUL.md) from this repo and replace your current one.

> ⚠️ This will replace all existing content in your SOUL.md.

**Steps:**
1. Backup your current `SOUL.md` first
2. Copy the entire content of [`SOUL.md`](SOUL.md)
3. Replace your file

---

## Levels / 级别

| Level | Description | 示例 |
|-------|-------------|------|
| `lite` | Drop filler/hedging only. Professional but tight. | "Your component re-renders because you create a new object reference each render. Wrap in useMemo." |
| `full` | Classic caveman. Fragments OK. Short synonyms. **← Default** | "New object ref each render. Inline object prop = new ref = re-render. useMemo." |
| `ultra` | Max compression. Abbreviations + arrows. | "Inline obj prop → new ref → re-render. useMemo." |
| `wenyan` | 文言文风格 Classical Chinese. | "物出新參照，致重繪。useMemo Wrap之。" |

**Switch level:** `/terse lite|full|ultra|wenyan`

**Exit:** `normal mode` / `正常模式`

---

## Auto-Clarity

Terse mode pauses automatically for / Terse 模式在这些情况下自动暂停：

- Security warnings / 安全警告
- Irreversible action confirmations / 不可逆操作确认
- User asks for detail: `解释一下` / `详细点` / `展开`
- Multi-step sequences at risk of misread / 多步骤指令可能产生误解时

Terse resumes after the clear part is done. / 明确部分完成后恢复 terse。

---

## How it works / 原理

The rules are written in `SOUL.md`. Hermes Agent reads SOUL.md on every session start, so the rules are always active — no skill loading needed.

Terse Mode is **persistent by default**. It stays on across all turns until you explicitly exit with `正常模式` or `normal mode`.

---

## Project Structure / 项目结构

```
hermite-caveman/
├── README.md       ← This file (bilingual)
├── SOUL.md         ← Full Terse Mode rules (copy section or entire file)
├── SKILL.md        ← Skill format version (optional, for skill-based loading)
└── LICENSE         ← MIT
```

---

## Credits

Based on [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) — 37.6k stars, MIT licensed.

---

## License

MIT License. See [`LICENSE`](LICENSE).

---

# 中文

# Hermite Caveman — Terse Mode

> 🪨 why use many token when few token do trick

**Hermes Agent / OpenClaw 的极度精简输出模式。将回复压缩约 75%，同时保留全部技术内容。**

**Terse Mode 默认开启。** 安装后自动生效，无需额外操作。

---

## 安装方式（二选一）

### 方式 A — 只复制 Terse Mode 部分（推荐）

只复制 `SOUL.md` 中的 `## Terse Mode` 章节（从第 25 行到第 63 行），追加到你现有的 `SOUL.md` 文件末尾。

保留你原有的 SOUL.md 内容，只新增 terse 规则。

**步骤：**
1. 打开本仓库的 [`SOUL.md`](SOUL.md)
2. 找到 `## Terse Mode` 部分
3. 将该部分复制到你的 Hermes `SOUL.md` 文件末尾

---

### 方式 B — 完全覆盖 SOUL.md

如果你的 `SOUL.md` 是空的，或者想要完整版本，直接用本仓库的 [`SOUL.md`](SOUL.md) 整体覆盖。

> ⚠️ 这会替换掉你现有的所有 SOUL.md 内容，请先备份。

**步骤：**
1. 先备份你当前的 `SOUL.md`
2. 复制本仓库 [`SOUL.md`](SOUL.md) 的全部内容
3. 整体替换你的文件

---

## 级别

| 级别 | 说明 | 示例 |
|------|------|------|
| `lite` | 仅删除 filler/hedging。保留冠词和完整句子。专业简洁。 | "Your component re-renders because you create a new object reference each render. Wrap in useMemo." |
| `full` | 经典 caveman 风格。允许碎片化句子。**← 默认** | "New object ref each render. Inline object prop = new ref = re-render. useMemo." |
| `ultra` | 极致压缩。缩写 + 箭头表示因果。 | "Inline obj prop → new ref → re-render. useMemo." |
| `wenyan` | 文言文风格。 | "物出新參照，致重繪。useMemo Wrap之。" |

**切换级别：** `/terse lite|full|ultra|wenyan`
**退出：** `正常模式` / `normal mode`

---

## 自动退出情况

以下情况 Terse Mode 自动暂停，恢复正常输出：

- 安全警告 / 不可逆操作确认
- 用户要求详细解释：`解释一下` / `详细点` / `展开`
- 多步骤指令可能有误解风险

明确部分完成后自动恢复 Terse Mode。

---

## 原版对比

| | 原版 caveman | Hermite Caveman |
|---|---|---|
| 目标平台 | Claude Code | Hermes / OpenClaw |
| 激活方式 | 项目配置文件自动扫描 | SOUL.md 内置规则 |
| 安装方式 | 插件安装 | 复制 SOUL.md |
| 分级 | lite/full/ultra/wenyan | lite/full/ultra/wenyan ✅ |

---

## 致谢

基于 [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman)（37.6k stars，MIT License）。

---

## 开源协议

MIT License。见 [`LICENSE`](LICENSE)。
