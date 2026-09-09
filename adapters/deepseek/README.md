# DeepSeek Adapter

DeepSeek is adapted through prompt injection rather than local skill discovery. The API receives the content of `skill/research-solution-supervisor/SKILL.md` as the system instruction, then receives the user's research objective and the AI output as the task input.

## Basic pattern

1. Read `skill/research-solution-supervisor/SKILL.md`.
2. Put that content in the `system` message.
3. Put the research objective, current stage, AI output, and requested supervision task in the `user` message.
4. Ask the model to preserve the four-section response contract.

## Python example

```python
from pathlib import Path
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_DEEPSEEK_API_KEY",
    base_url="https://api.deepseek.com",
)

skill = Path("skill/research-solution-supervisor/SKILL.md").read_text(
    encoding="utf-8"
)

user_input = """
我的研究目标：
...

当前阶段：
...

待监督的 AI 输出：
...

请判断它是否真正推进了研究方案，指出关键缺口，并给出下一步动作。
"""

response = client.chat.completions.create(
    model="deepseek-chat",
    messages=[
        {"role": "system", "content": skill},
        {"role": "user", "content": user_input},
    ],
)

print(response.choices[0].message.content)
```

## Minimal prompt-only version

If you are using DeepSeek in a chat UI rather than the API, paste this before the research material:

```text
你现在使用 research-solution-supervisor 这个监督规则。
目标不是重新做完整研究，而是判断当前 AI 输出是否真正推进了我的研究方案。
请按照“推进判断 / 关键缺口 / 下一步动作 / 当前状态”输出。
```

Then paste:

```text
研究目标：
...

当前阶段：
...

AI 输出：
...
```

## Boundaries

- DeepSeek adapter does not install a persistent local skill by itself.
- Do not send private research material to the DeepSeek API unless the user has approved that destination.
- Treat the supervision result as a progress-control judgment. If it says a claim needs evidence, run or request a separate verification step.
