# learn-from-code

[![skills.sh](https://skills.sh/b/HarleySw/learn-from-code-skill)](https://skills.sh/HarleySw/learn-from-code-skill)
[![Version](https://img.shields.io/badge/version-2.1.3-blue.svg)](https://github.com/HarleySw/learn-from-code-skill)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Agent Skills](https://img.shields.io/badge/standard-agentskills.io-purple.svg)](https://agentskills.io/)

**English** | [Tiếng Việt](README.vi.md)

If you ask an AI to explain the code it just wrote and you walk away still confused, that is on the AI, not you. By default, models dump walls of jargon, skip the mental model, and throw unfamiliar APIs or library mechanisms at you without saying when they run or what they return.

`learn-from-code` is an [Agent Skill](https://agentskills.io/) that fixes how your coding agent explains `git diff`s and commits. When an agent writes a large change across several files, this skill makes it start with plain-language concepts, explain unfamiliar APIs and mechanisms before showing code, and walk through concrete test cases step by step.

## Quick start

Install with the [Skills CLI](https://skills.sh) (works with Claude Code, Cursor, Antigravity, OpenCode, Codex, Windsurf, Cline, and 70+ agents):

```bash
# Install in current project
npx skills add HarleySw/learn-from-code-skill

# Or install globally
npx skills add -g HarleySw/learn-from-code-skill
```

<details>
<summary>Manual installation</summary>

Copy [`skills/learn-from-code/SKILL.md`](skills/learn-from-code/SKILL.md) into your agent's skill or rules folder:

- **Claude Code:** `.claude/skills/learn-from-code/SKILL.md`
- **Cursor:** `.cursor/rules/learn-from-code.mdc`
- **Google Antigravity / OpenCode / Codex:** `.agents/skills/learn-from-code/SKILL.md`
</details>

## Usage

To review a commit, `git diff`, or existing module (Mode A, default):

```text
/learn-from-code
/learn-from-code explain commit 0bb8705
```

To practice writing the core logic yourself while the agent sets up the surrounding boilerplate (Mode B, `TODO(human)`):

```text
Use learn-from-code in Mode B to implement the retry backoff logic.
```

## How it works

In Mode A, the review runs in two turns so you can check your understanding before moving on.

**Turn 1: Understand the change**

1. **Big picture:** Describes what happens in practice, why the bug or design problem occurs using an everyday analogy, and a short `Before -> After` snippet (`❌` vs `✅`) without burying you in file or variable names upfront.
2. **Main execution flow:** Traces the path from the user action or system trigger through the main functions or entry points (at most one primary function or mechanism per step).
3. **Core code steps:** Explains unfamiliar APIs, library mechanisms, or parameters before showing the code block, adds inline comments and concrete examples above each branch, explains why simpler approaches fail, and walks through concrete cases (with `< 76-char` ASCII rulers when string slicing or offsets are involved).
4. **Verification & hidden risks:** Checks callers outside the `git diff`, backward compatibility with saved data, and edge cases. If the diff has a bug, it flags it under `⚠️ Defect Alert` with a `❌ Current -> ✅ Fix` snippet.
5. **Debugging check:** Ends with boundary scenarios and a small code slice right in place, so you can test your mental model without scrolling back up.

**Turn 2: Consolidate & transfer (after you reply)**

6. **Answer review:** Walks through your answers with exact `filename:line` references.
7. **Transferable patterns:** Pulls out the reusable engineering patterns from the diff and notes when they break down.
8. **Blast radius & next actions:** Lists coupled files that need to change together in the future and suggests missing edge-case unit tests.

## What makes it readable

The output is formatted to fit both 80-column CLI terminals and narrow IDE chat sidebars. Code comments and ASCII diagrams stay under 76 characters per line, explanations use short sub-bullets (`1 idea = 1 bullet`), and domain codes are checked against your repo's docs and tests before the agent explains them.

It also responds in whatever language you use (English, Vietnamese, Japanese, and others) while keeping everyday developer terms (`git diff`, `commit`, `race condition`, `fallback`, `callback`, `unit test`) intact.

## License

[MIT](LICENSE) © HarleySw
