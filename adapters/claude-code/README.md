# Claude Code Adapter

Claude Code supports skills with a `SKILL.md` entrypoint. Research Solution Supervisor can be used directly as a Claude Code skill by copying the canonical package into a Claude skills directory.

## Install

Project-level install:

```bash
mkdir -p .claude/skills
cp -R skill/research-solution-supervisor .claude/skills/
```

User-level install:

```bash
mkdir -p "$HOME/.claude/skills"
cp -R skill/research-solution-supervisor "$HOME/.claude/skills/"
```

The project-level install is useful when a repository has a repeatable research workflow and every Claude Code session should supervise AI outputs against that workflow. The user-level install is better when you want the same supervision behavior across many projects.

## Invoke

Explicit invocation:

```text
Use the research-solution-supervisor skill to audit whether this AI answer actually advances my research plan.
```

Chinese invocation:

```text
使用 research-solution-supervisor，判断这个 AI 回答是否真的解决了我当前研究方案的问题，并给出下一步动作。
```

## Claude-specific notes

- Keep the user's research objective and the AI output in the same message when possible.
- Ask Claude to preserve the skill's four-part response shape if you need comparable evaluations across multiple research passes.
- Do not rely on the skill as source verification by itself. If a claim needs current sources, ask Claude Code to run the appropriate research or verification step after the supervision pass identifies the gap.

## Minimal prompt template

```text
Use research-solution-supervisor.

Research objective:
...

Current stage:
...

AI output to evaluate:
...

Please classify the progress, identify only material gaps, and give the smallest next action that will move the research forward.
```
