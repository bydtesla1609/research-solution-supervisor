# Codex Adapter

Research Solution Supervisor is a native Codex skill. The canonical skill package lives at:

```text
skill/research-solution-supervisor/
|-- SKILL.md
`-- agents/
    `-- openai.yaml
```

## Install

User-level install:

```bash
mkdir -p "$HOME/.agents/skills"
cp -R skill/research-solution-supervisor "$HOME/.agents/skills/"
```

Project-level install:

```bash
mkdir -p .agents/skills
cp -R skill/research-solution-supervisor .agents/skills/
```

Some Codex Desktop setups still use `$CODEX_HOME/skills` or `$HOME/.codex/skills` for personal skills. If your local Codex already discovers skills there, copy the same folder to that directory instead.

## Invoke

Use it explicitly when you want a supervision pass:

```text
使用 research-solution-supervisor，检查下面这个 AI 回答是否真正推进了我的研究方案。
```

Codex can also discover the skill automatically when the request is about supervising research progress, checking whether an AI answer solved the user's plan, or deciding the next effective research step.

## Expected input

Provide as much of this as possible:

- the research objective;
- the current stage of the research;
- the AI output or proposed solution to evaluate;
- any success criteria, evidence requirements, or next decision the user needs to make.

## Expected output

The skill should answer with:

- 推进判断;
- 关键缺口;
- 下一步动作;
- 当前状态.

It should not turn the supervision pass into a full research report unless the user asks for that separately.
