# Barton AI Coding Baseline

> Karpathy 规则 + 跨会话上下文管理，融合为一体化方案。

## 这是什么

本项目将两个东西融合为一个完整的、可移植的框架：

| 组件 | 来源 | 解决什么问题 |
|------|------|-------------|
| 规则 1-4：先思考再编码、简单优先、手术式修改、目标驱动执行 | 基于 [Andrej Karpathy](https://github.com/karpathy) 对 LLM 编码缺陷的观察 | AI 在单次会话内如何编码 |
| 规则 5：基线上下文 + `BASELINE.md` | [Barton](https://github.com/bartonzzb) 原创贡献 | 两次会话之间如何保持上下文 |

Karpathy 指出 LLM 会做出错误假设、过度复杂化代码、顺手修改无关代码。现有方案（如 [andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills)）将他的 4 条规则打包成 CLAUDE.md——但只解决了一半问题。

**缺失的另一半**：新会话开始时，AI 什么都忘了。项目状态、范围边界、进行中的任务、禁止触碰的区域——全没了。你每次开新会话都要花 10 分钟重新解释。

本项目将 Karpathy 的 4 条规则与第 5 条原创规则（基线上下文）及 `BASELINE.md` 模板融合，一次性解决两个问题。

## 5 条规则

### 规则 1：先思考再编码 *(Karpathy)*

**不要假设。不要隐藏困惑。暴露权衡。**

实现之前：
- 明确陈述假设。不确定就问。
- 存在多种解释时，全部呈现——不要擅自选择。
- 存在更简单的方案时，说出来。该拒绝时就拒绝。
- 遇到不清楚的地方，停下来。指出困惑点，提问。

### 规则 2：简单优先 *(Karpathy)*

**最少代码解决问题。不做推测性功能。**

- 不做未被要求的功能。
- 不为一次性代码做抽象。
- 不做未被要求的"灵活性"或"可配置性"。
- 不为不可能的场景做错误处理。
- 如果 200 行能缩成 50 行，重写。

### 规则 3：手术式修改 *(Karpathy)*

**只碰必须碰的。只清理自己的烂摊子。**

编辑现有代码时：
- 不"顺便改进"相邻代码、注释或格式。
- 不重构没坏的东西。
- 匹配现有风格，即使你不同意。
- 发现无关的死代码时，提一下——不要删。

### 规则 4：目标驱动执行 *(Karpathy)*

**定义成功标准。循环直到验证通过。**

将命令转化为可验证的目标：
- "加验证" → "写无效输入的测试，然后让它们通过"
- "修 bug" → "写复现测试，然后让它们通过"
- "重构 X" → "确保重构前后测试都通过"

### 规则 5：基线上下文 *(Barton 原创)*

**先读再做。守住边界。永不丢失上下文。**

会话开始时：
- 如果项目根目录有 `BASELINE.md`，在读任何请求前先读它。
- 收到「基线恢复」指令时，先读 `BASELINE.md`，恢复完整上下文，再回应。

任何任务开始前：
- 读 `BASELINE.md` 确认当前状态和范围边界。
- 未经 owner 明确审批，不做范围外变更。
- 未经审批不得部署/发布。

任何任务完成后：
- 更新 `BASELINE.md`：已完成任务、当前版本、进行中状态。
- 会话结束前，提醒 owner 确认 `BASELINE.md` 是否需要更新。

## 快速开始

1. 把 `BASELINE.md` 复制到你的项目根目录
2. 填写字段（2 分钟）
3. 把对应工具的集成文件复制到指定位置：

| 工具 | 文件 | 位置 |
|------|------|------|
| Claude Code | `integrations/claude-code/CLAUDE.md` | 项目根目录或 `.claude/settings.json` |
| Cursor | `integrations/cursor/baseline-rules.mdc` | `.cursor/rules/` |
| Windsurf | `integrations/windsurf/.windsurfrules` | 项目根目录 |
| GitHub Copilot | `integrations/copilot/copilot-instructions.md` | `.github/copilot-instructions.md` |

## BASELINE.md 里写什么

| 字段 | 用途 | 示例 |
|------|------|------|
| 项目信息 | 这是什么项目 | "液冷泵控制器，v2.3" |
| 技术栈 | 核心技术 | "STM32 HAL, FreeRTOS, C99" |
| 进行中 | 正在做什么 | "T4: UART 驱动重构" |
| 已完成 | 做完了什么 | "T1: ADC 校准 - 2024-03-15" |
| 边界 | 不能碰什么 | "未经审批不得修改 safety_monitor.c" |
| 审批记录 | 变更控制日志 | "2024-03-20: 批准 T5 范围变更" |

## BASELINE.md 格式规则

1. **精简**：每个字段最多 1-3 行。不记流水账。
2. **只记录当前状态**：BASELINE 是快照，不是变更日志。
3. **已完成任务**：只写 ID + 名称 + 日期。不写细节。

## 为什么有效

Karpathy 的洞察："LLM 非常擅长在循环中直到满足特定目标才停下——不要告诉它做什么，给它成功标准，然后看它执行。"

本项目延伸了这个洞察：同样的 LLM 能力也适用于上下文管理——前提是上下文以结构化的、极简的形式存在于一个文件中，且 AI 在会话开始时读取它。一个 AI 实际会读的 15 行 `BASELINE.md`，比一个 AI 会忽略的 500 页文档有价值得多。

## 致谢

- 规则 1-4 基于 [Andrej Karpathy](https://github.com/karpathy) 对 LLM 编码行为的公开观察，由 [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) 推广普及。
- 规则 5（基线上下文）及 `BASELINE.md` 模板为原创贡献。

## 许可证

MIT

---

**Karpathy 的规则让 AI 写得更好。Barton 的基线让 AI 记得更久。融合为一体。**
