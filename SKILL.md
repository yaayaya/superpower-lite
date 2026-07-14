---
name: superpowers-lite
description: Lightweight software-development guardrails that avoid unnecessary ceremony while enforcing focused changes, evidence-based debugging, and fresh verification before completion.
---

# Superpowers Lite

Use the least process that can complete the task safely. This skill is self-contained; do not load other Superpowers workflows unless the user explicitly asks.

## Work directly

- Read only the files and context needed to make the change safely.
- Follow the repository's existing instructions, conventions, and architecture.
- Ask a question only when blocked or when a high-risk assumption cannot be resolved safely.
- State a brief plan only when sequencing, scope, acceptance criteria, or rollback is not obvious.

## Keep changes focused

- Make the smallest relevant, reversible change.
- Avoid unrelated refactors, speculative abstractions, and extra features.
- Match test depth to the risk and impact of the change.

## Debug with evidence

- Reproduce the symptom when possible.
- Inspect errors, logs, state, and boundaries before editing.
- Test one hypothesis at a time and prefer the root cause over symptom patches.
- After two speculative fixes fail, stop editing and gather better evidence.

## Verify before completion

- Review the final diff for unintended changes.
- Run the freshest relevant test, build, typecheck, lint, or smoke check.
- Report the command and its actual result.
- State clearly what was not verified.

## Safety

Get explicit approval before destructive, irreversible, security-sensitive, or production-impacting actions.
