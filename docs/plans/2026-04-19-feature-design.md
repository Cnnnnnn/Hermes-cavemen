# hermes-cavemen 功能增强设计

Date: 2026-04-19
Status: Draft
Author: hermes-cavemen contributor

---

## B. 自适应压缩强度（Adaptive Compression）

### 目标

AI 根据上下文自动判断并切换到最合适的压缩级别，无需用户手动 `/terse`。用户显式设置永远优先。

### 信号体系

每次回复前，AI 评估三个信号：

| 信号 | 取值 | 说明 |
|------|------|------|
| **任务类型** | code / explain / chat / creative / analysis / destructive | destructive 触发 Auto-Clarity；code 输出天然 ultra |
| **用户语言** | zh / en / mixed | 中文场景 wenyan 可用；英文场景 full/ultra |
| **预估输出长度** | short (<100字) / medium / long (>500字) | 长输出自动 ultra 省 token；短输出 lite 减少机械感 |

### 决策树

```
IF  destructive:
        → Auto-Clarity（临时停 terse）
ELIF  task == code:
        → ultra
ELIF  预估长度 == long AND 语言 == en:
        → ultra
ELIF  语言 == zh AND 用户指定 wenyan:
        → wenyan / wenyan-lite / wenyan-full
ELIF  预估长度 == short:
        → lite
ELIF  语言 == en:
        → full
ELSE:
        → full（默认）
```

### Auto-Escalation（自动升级）

触发条件：单次输出超过 800 字，且当前级别为 full/lite。
触发时机：在下一个段落开始处切换到 ultra，只升一次，不降级。
目的：长文末尾也保持压缩率，不虎头蛇尾。

### 优先级

用户显式 `/terse xxx` 命令 > 自动判断。所有自动逻辑都服从手动设置。

### 实现位置

SOUL.md 新增 `### Adaptive Logic` 章节，写入决策树和信号说明，作为 AI 的内置判断规则。

---

## D. 压缩率实测报告（Compression Verification）

### 目标

verify.sh 输出客观的压缩率数字，让用户知道 Terse Mode 实际省了多少 token。

### 方案 D2 — 字数估算

- 中文：1 字 ≈ 1 token
- 英文：1 词 ≈ 1.3 token（tiktoken 近似系数）
- 压缩率 = (原文 token - 压缩后 token) / 原文 token × 100%

verify.sh 内置基准原文和各级别预期压缩率，不依赖外部 API。

### 方案 D3 — 模拟测试

verify.sh 携带固定测试用例：

```
原文: "当然！我很高兴帮你解决这个问题。你遇到的问题很可能是由于认证中间件没有正确验证 token 过期时间导致的。"
原文字符数: 62 字
预期压缩后（full）: "认证中间件 bug。Token 过期检查用了 < 而不是 <=。修："
压缩后字符数: 32 字
压缩率: 48%
```

用户也可以传入自定义文本进行测试。

### Pass/Fail 标准

| 级别 | 最低压缩率 |
|------|-----------|
| lite | 20% |
| full | 40% |
| ultra | 60% |
| wenyan | 70% |

---

## E. 多语言上下文感知（Multilingual Awareness）

### 目标

中英混合输出场景下，各自独立压缩，不破坏语言自然感。

### 方案 E1 — 主语言优先

1. AI 判断回复主体语言（zh / en）
2. 主语言段落走 full/ultra
3. 另一语言的独立段落、术语，保持该语言但压缩比率跟随主语言
4. 用户明确指定语言时，服从指定

### 方案 E2 — 各自独立压缩

- 中文段落：按 full/ultra/wenyan 规则压缩（删 filler、删冠词、碎片化）
- 英文段落：同样按 full/ultra 规则压缩（Drop articles、short synonyms）
- 术语、代码块：保持原样

### 具体行为示例

| 场景 | 行为 |
|------|------|
| 用户说中文，夹杂英文术语 | 英文术语保持英文，整体走 full/ultra |
| 用户说"用英文解释" | 整段按英文 full 处理 |
| 用户说中文、无特殊要求 | 默认 full/wenyan |
| 中英文交替出现 | 各段独立判断，术语保持原语言 |

### 实现位置

SOUL.md 修改 `### Rules`，新增多语言感知行为说明，作为 AI 的内置感知规则。

---

## 实施顺序

1. **D**（verify.sh 增强）→ 独立脚本，不影响核心规则，最安全
2. **E**（多语言感知）→ 修改 SOUL.md Rules，增量改动
3. **B**（自适应压缩）→ 修改 SOUL.md，新增 Adaptive Logic 章节，改动最大

每个功能独立 commit，可单独回滚。
