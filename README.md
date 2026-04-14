# better-reasoning

A Claude skill that compresses internal reasoning to reduce token usage — while keeping all outputs concise, accurate, and in the user's language.

---

## How It Works

When activated, the skill instructs Claude to:

1. **Reason internally in Chinese (中文)** — a compact internal shorthand that reduces token consumption during the thinking process
2. **Output in the user's language** — always, regardless of internal reasoning language
3. **Fall back to full reasoning** when the task is complex, ambiguous, or safety-sensitive

The goal is fewer tokens spent on internal monologue, without sacrificing output quality.

---

## When It Activates

The skill triggers on simple-to-medium complexity tasks:

- Summarization
- Quick code fixes or edits
- Short drafts, docs, or READMEs
- Quick factual explanations
- Repetitive or operational tasks

---

## When It Falls Back to Full Reasoning

The skill disengages and Claude uses full reasoning when:

- The task requires multi-step or complex reasoning
- The request is ambiguous and needs careful interpretation
- The task is safety-sensitive
- Chinese shorthand would reduce accuracy for the specific task

---

## Installation

1. Copy the skill file into your Claude agent's skill directory.
2. Reference it in your agent configuration.

```bash
# Example: add to your skills folder
cp compressed-reasoning.md ./skills/compressed-reasoning.md
```

Then activate it in your system prompt or agent config as you would any other skill.

---

## Testing

Before deploying, validate the skill with the following steps.

### Smoke Tests (Manual)

Run these prompts and inspect the outputs:

| Prompt | Expected Behavior |
|---|---|
| `"Summarize this in 5 bullets: [text]"` | Concise bullets, no preamble |
| `"Fix this function: [broken code]"` | Minimal, working code |
| `"What is the capital of France?"` | Direct answer, no over-reasoning |
| `"Explain quantum entanglement"` | Should fallback to full reasoning |
| `"Should I take this medication with alcohol?"` | Must fallback — safety-sensitive |

Check: Is the output concise? Is Chinese leaking into responses?

### Token Comparison

Run identical prompts **with** and **without** the skill. Compare via the API response:

```json
{
  "usage": {
    "input_tokens": 120,
    "output_tokens": 85
  }
}
```

Only ship if there's a measurable reduction. If savings are negligible, the added complexity isn't worth it.

### Edge Case Stress Tests

- Ambiguous request → does it expand or compress incorrectly?
- Safety-adjacent question → does fallback trigger reliably?
- Non-English user input → does output stay in the user's language?
- Mixed-language input → does it handle gracefully?

---

## Known Limitations

| Area | Status |
|---|---|
| Chinese reasoning effectiveness | ⚠️ Prompt-guided, not a guaranteed internal mode |
| Token savings | ⚠️ Not universally validated — measure per use case |
| Output language safety | ⚠️ Depends on model compliance |
| Fallback reliability | ⚠️ Requires testing per deployment context |

> **Note:** This skill works best as a soft optimization hint. It is not a deterministic switch — Claude's internal reasoning is not directly observable or controllable.

---

## Roadmap

- [ ] Eval suite with baseline token comparisons
- [ ] Benchmark results across task categories
- [ ] Multi-language reasoning variants (beyond Chinese)
- [ ] Automated fallback detection tests
- [ ] CI integration for regression testing

---

## Contributing

PRs and issues welcome. If you run token benchmarks with this skill, please share results — empirical data on savings across different task types is the biggest open question.

---

## License

GPLv3
