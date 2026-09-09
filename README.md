# Research Solution Supervisor

Research Solution Supervisor 是一个面向研究型工作的 AI 监督 skill。它不负责把研究重新做一遍，而是判断 AI 的回答、研究 pass 或方案建议是否真正推进了用户的研究目标，并把结论转成下一步可执行动作。

它适合用在这样的时刻：

- 你已经有一个研究目标、方案或阶段性计划；
- AI 给出了一版回答、调研、推导、方案或结论；
- 你需要判断这版输出是否足够可靠，能不能进入下一步；
- 你希望整个研究流程稳步推进，而不是停留在“看起来有很多内容”的状态。

## 核心功能

- **推进判断**：把当前 AI 输出归类为“已较好解决 / 部分解决 / 尚未解决 / 需要先补证据 / 需要用户决策”。
- **缺口暴露**：指出真正影响研究推进的缺口，而不是泛泛修改文风。
- **证据监督**：区分已确认事实、AI 声称、推断和仍需验证的信息。
- **下一步动作**：给出最小但有效的后续行动，例如下一轮提示词、证据清单、实验任务、比较表或用户决策问题。
- **流程控速**：帮助用户判断该继续、修订、验证还是暂停依赖当前结果。

## 工作方式

这个 skill 借鉴了 Intent Debugger 的思路：先把目标和判断标准说清楚，再评估当前输出是否满足这些标准。不同点在于，Intent Debugger 更偏“把模糊意图变成可确认需求”，Research Solution Supervisor 更偏“监督 AI 是否让研究方案真的向前走”。

一次典型监督会输出四块内容：

1. **推进判断**：当前 AI 工作是否解决了本阶段问题。
2. **关键缺口**：哪些缺失、冲突或证据不足会影响继续推进。
3. **下一步动作**：下一轮最该做什么，必要时给出可直接发送的 prompt。
4. **当前状态**：哪些可以暂时视为成立，哪些仍然不能依赖。

## 仓库结构

```text
research-solution-supervisor/
|-- README.md
|-- skill/
|   `-- research-solution-supervisor/
|       |-- SKILL.md
|       `-- agents/
|           `-- openai.yaml
`-- adapters/
    |-- codex/
    |-- claude-code/
    `-- deepseek/
```

## 快速开始

### OpenAI Codex

把 `skill/research-solution-supervisor` 复制到 Codex 可发现的 skills 目录中：

```bash
mkdir -p "$HOME/.agents/skills"
cp -R skill/research-solution-supervisor "$HOME/.agents/skills/"
```

在部分本地 Codex Desktop 环境里，skills 目录可能是 `$CODEX_HOME/skills` 或 `$HOME/.codex/skills`。如果你的 Codex 已经在这些目录发现个人 skill，也可以复制到对应目录。

更多说明见 [adapters/codex](adapters/codex/)。

### Claude Code

Claude Code 支持从用户级或项目级 skills 目录加载 skill。把同一个 skill 文件夹复制到 `.claude/skills` 或用户级 skills 目录即可：

```bash
mkdir -p .claude/skills
cp -R skill/research-solution-supervisor .claude/skills/
```

更多说明见 [adapters/claude-code](adapters/claude-code/)。

### DeepSeek

DeepSeek API 本身不会自动读取本地 `SKILL.md` 目录。适配方式是把 `SKILL.md` 的说明作为 system/developer prompt 的一部分注入到对话里，再把用户的研究方案和 AI 输出作为待监督材料传入。

更多说明见 [adapters/deepseek](adapters/deepseek/)。

## 使用示例

```text
使用 research-solution-supervisor。

我的研究目标是：比较几种数学建模思路，找出最适合比赛论文的方案。

当前 AI 给出的方案如下：
...

请判断它是否真正推进了我的研究流程，指出关键缺口，并给出下一步最有效动作。
```

## 适用与不适用

适用：

- 研究方案监督
- AI 输出质量审查
- 阶段性研究推进判断
- 证据、推理和下一步动作检查
- 人机协作研究流程控速

不适用：

- 普通代码 review
- 单纯润色最终文档
- 没有研究目标或方案背景的泛泛评价
- 已经明确要执行的实现任务

## 参考

- [OpenAI Codex skills documentation](https://developers.openai.com/codex/build-skills)
- [Claude Code skills documentation](https://docs.anthropic.com/en/docs/claude-code/skills)
- [DeepSeek Chat Completion API](https://api-docs.deepseek.com/api/create-chat-completion)
