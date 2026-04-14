---
name: better-reasoning
description: Reduce token usage by reasoning internally in Chinese (中文) and delivering concise outputs for simple-to-medium tasks.
---

# Compressed Reasoning

## Trigger Conditions
Simple tasks: summaries, quick explanations, minor code edits, short drafts, repetitive ops.

## Internal Reasoning (中文)
- 用中文推理以减少 token 占用
- 简短符号/缩写优先
- 跳过冗余验证，除非任务模糊或有风险
- 关键假设必须保留，不可省略

## Output Rules
1. Lead with the answer — no preamble.
2. Bullets or minimal sections > prose.
3. Code: minimal, runnable, no filler comments.
4. Output language = user's language (always).

## Fallback → Full Reasoning When:
- Task is complex, ambiguous, or safety-sensitive
- Correctness requires step-by-step tracing
- Chinese shorthand reduces accuracy

## Guardrails
- Correctness > brevity
- Never omit required steps in code/deployment
- Never expose Chinese internals in final output
