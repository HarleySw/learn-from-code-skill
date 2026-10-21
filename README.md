# learn-from-code

[![skills.sh](https://skills.sh/b/HarleySw/learn-from-code-skill)](https://skills.sh/HarleySw/learn-from-code-skill)
[![Version](https://img.shields.io/badge/version-2.1.2-blue.svg)](https://github.com/HarleySw/learn-from-code-skill)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Agent Skills](https://img.shields.io/badge/standard-agentskills.io-purple.svg)](https://agentskills.io/)

**English** | [Tiếng Việt](README.vi.md)

An [Agent Skill](https://agentskills.io/) that helps you actually understand AI-generated code and `git diff`s—turning complex changes into clear mental models, visual state traces, and debugging intuition instead of walls of text.

## Quick Start

Install via the [Skills CLI](https://skills.sh) (supports Claude Code, Cursor, Antigravity, OpenCode, Codex, Windsurf, Cline, and 70+ agents):

```bash
# Install in current project
npx skills add HarleySw/learn-from-code-skill

# Or install globally
npx skills add -g HarleySw/learn-from-code-skill
```

<details>
<summary>Manual installation</summary>

Copy [`skills/learn-from-code/SKILL.md`](skills/learn-from-code/SKILL.md) into your agent's skill/rules folder:

- **Claude Code:** `.claude/skills/learn-from-code/SKILL.md`
- **Cursor:** `.cursor/rules/learn-from-code.mdc`
- **Google Antigravity / OpenCode / Codex:** `.agents/skills/learn-from-code/SKILL.md`
</details>

## Usage

**Mode A — Understand a commit, `git diff`, or module (Default):**

```text
/learn-from-code
/learn-from-code explain commit 0bb8705
```

**Mode B — Practice hands-on coding (`TODO(human)`):**

```text
Use learn-from-code in Mode B to implement the retry backoff logic.
```

## How It Works

When reviewing code (Mode A), the agent follows a structured **2-turn workflow**:

**Turn 1 — Understand the Change:**

1. **Big Picture:** Plain-language symptom, everyday analogy, root cause, and a `Before -> After` snippet (`❌` vs `✅`).
2. **Main Execution Flow:** Chronological steps anchored to real-world user or system triggers (max 1 primary function per step).
3. **Core Code Steps:** Focused snippets with inline comments, concrete examples, *Contrastive Why* sub-bullets, and a **4-column Markdown State Trace Table** (plus **ASCII Rulers** for slice/offset math).
4. **Verification & Hidden Risks:** Audits unseen callers, backward compatibility (`localStorage`/DB), and edge cases, flagging real bugs with `⚠️ Defect Alert`.
5. **Debugging Check:** Targeted boundary questions dynamically scaled to the change to test your mental model before moving on.

**Turn 2 — Consolidate & Transfer *(after you reply)*:**

6. **Answer Review:** Step-by-step feedback on your answers pinpointed to exact `filename:line` locations.
7. **Transferable Patterns:** Reusable engineering patterns extracted from the diff (when to use vs. when not to use).
8. **Blast Radius & Next Actions:** Cross-file co-change rules and missing edge-case tests worth adding.

## Features

- **Beginner-Friendly & Senior-Deep:** Explains the big picture in plain everyday words before diving into code mechanics, with zero arbitrary length caps.
- **Visual Breathing Room:** Uses scannable sub-bullets, 4-column state trace tables, and `< 76-char` ASCII rulers that render cleanly in both 80-col CLI terminals and narrow IDE sidebars.
- **Anti-Hallucination & Caller Audit:** Grounds domain terms in repo docs/tests first and checks callers outside the `git diff` for regressions.
- **Auto-Localized:** Automatically responds in your language (English, Vietnamese, Japanese, etc.) using natural developer phrasing rather than stiff machine translation.

## License

[MIT](LICENSE) © HarleySw
