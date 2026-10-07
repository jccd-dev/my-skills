---
name: learn-skill
description: Explain completed coding work in a concise learning-focused way only when the user explicitly mentions this skill by name, such as "$learn-skill", "learn-skill", or "use learn-skill". Use after finishing a coding task, code review fix, debugging session, refactor, or implementation when the user wants to learn what changed, what the code is for, why it is needed, how the logic works, and the flow. Do not use automatically for ordinary coding tasks unless the user explicitly invokes this skill.
---

# Learn Skill

## Purpose

Use this skill after the requested coding work is complete to teach the user what was done. Keep the explanation practical, short, and tied directly to the actual code changes.

## Workflow

1. Finish the requested coding task first.
2. Review the relevant changed files, commands run, and behavior verified.
3. Read `references/output-format.md`.
4. Add a compact learning section to the final response using that format.

## Explanation Rules

- Explain only the files, functions, components, logic, or commands that matter to the completed task.
- Prefer simple language over jargon. If a term is necessary, define it in one short phrase.
- Say why a change was needed before explaining how it works.
- Describe the execution flow in order, from user action or entry point to final result.
- Mention important tradeoffs, assumptions, or limitations only when they affect the user.
- Avoid broad tutorials, unrelated background, repeated code dumps, and long line-by-line narration.
- Keep code excerpts tiny: only include the exact lines or shapes needed to explain the point.

## Output

Follow `references/output-format.md` for the final teaching format. If the task was very small, compress the format to the relevant sections only.
