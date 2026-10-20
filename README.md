# learn-from-code

[![skills.sh](https://skills.sh/b/HarleySw/learn-from-code-skill)](https://skills.sh/HarleySw/learn-from-code-skill)
[![Version](https://img.shields.io/badge/version-2.0.0-blue.svg)](https://github.com/HarleySw/learn-from-code-skill)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Agent Skills](https://img.shields.io/badge/standard-agentskills.io-purple.svg)](https://agentskills.io/)

An open-source [Agent Skill](https://agentskills.io/) that forces AI coding agents to teach codebase mental models, domain invariants, and debugging intuition from git diffs—preventing **comprehension debt** without slowing delivery down into a boring lecture.

---

## The Problem: Comprehension Debt

When AI coding models (Claude 3.7 Sonnet, OpenAI o3/Codex, Gemini) write hundreds of lines across multiple files in seconds, typing speed is no longer the bottleneck. The bottleneck is **human comprehension**.

Passive explanations fail software teams:
* ⚠️ **Split-Attention Overload:** Long prose paragraphs above code, duplicated inline comments, and redundant bullets below code repeating the same variable explanations.
* ⚠️ **Working Memory Overflow:** Dumping 7–8 internal helper functions and raw AST mechanics at once.
* ⚠️ **The "Looks Good To Me" Trap:** Smooth explanations that feel clear while reading, but leave you with zero debugging intuition when production fails 2 days later.

`learn-from-code` fixes this by enforcing a strict **90–120 second reading budget (350–500 prose words)**, inline concrete evaluations, causal data-flow storytelling, and active retrieval checks.

---

## Operating Modes

| Mode | Trigger | Workflow |
| :--- | :--- | :--- |
| **Mode A: Post-Implementation Review** *(Default)* | Reviewing a `git diff`, inspecting a commit, or after generating a feature | Delivers a concise **5-Section Output Contract**, anchors code mechanics to real-world business triggers, maps unseen caller risks, and pauses with 1–2 debugging questions. |
| **Mode B: In-Task Active Learning (`TODO(human)`)** | When you want hands-on muscle memory during implementation | Analyzes surrounding patterns, generates necessary boilerplate, and leaves 5–15 lines at the core decision point marked `TODO(human)` with a 4-bullet typed contract for you to fill in. |

---

## Key Principles & Cognitive Rules

* **Zero Split-Attention:** Explanations of boundary conditions, magic numbers, and regex groups live **strictly inside short inline comments** in the code snippet—never duplicated in prose above the code.
* **Bridge Domain & Program Models (Zero Domain Hallucination):** Anchors code mechanics to the **real-world business event** (or **Runtime/System trigger** for pure infrastructure diffs) and verifies internal domain codes against repo docs/tests before explaining—explicitly labeling undocumented codes as *inferred hypotheses* rather than guessing.
* **Causal Domino Chain (`A -> B -> C`):** Explains *why* naive alternatives fail (`Trigger A -> Intermediate state B -> Concrete failure C`), not just what line changed.
* **Interleaved Timelines for Async/Concurrency:** Renders async lifecycles, abort controllers, locks, and race conditions as chronologically interleaved timelines (`t1 -> t2 -> t3`).
* **Unseen Caller, Persisted-Data, Unhappy-Path & Scale Audit:** Scans immediate callers **outside the `git diff`**, checks backward compatibility with existing persisted data (`localStorage`/DB/cache/URL) whenever key formats or schemas change, audits `catch`/`fallback` blocks for silent failures, and flags `O(N^2)` / `N+1` scale bottlenecks.
* **Delocalized Co-Change Warnings:** Explicitly calls out split or mirrored logic across files (client vs. server, producer vs. consumer) with clear synchronization rules to prevent state drift.
* **State Invariant Breakpoints:** Pairs suggested breakpoint lines with the exact runtime state invariant expression to inspect, isolating defects before state infection spreads.
* **Working Memory Cap:** Execution flows are summarized in 3–4 plain-language steps naming **at most 2–3 primary functions**.
* **Automatic Localization & Mechanical Plain-Language Syntax:** Automatically responds in your conversation language (English, Vietnamese, Japanese, etc.) while enforcing `Concrete Actor + Strong Action Verb` (zero zombie nouns), `One Word = One Meaning` (zero synonym rotation), `Old-Before-New` sentence chaining, and a 25-word sentence cap across all models.

---

## Installation

### Via Skills CLI (Recommended)

Install locally into your current project (supports Claude Code, Cursor, Antigravity, OpenCode, Codex, and 70+ agents):

```bash
npx skills add HarleySw/learn-from-code-skill
```

Install globally across all supported coding agents on your machine:

```bash
npx skills add -g HarleySw/learn-from-code-skill
```

To update to the latest version:

```bash
npx skills update -g learn-from-code
```

### Manual Installation

Copy [`skills/learn-from-code/SKILL.md`](skills/learn-from-code/SKILL.md) into your agent's configuration directory:
* **Claude Code:** `.claude/skills/learn-from-code/SKILL.md` (or `~/.claude/skills/learn-from-code/SKILL.md`)
* **Cursor:** `.cursor/rules/learn-from-code.mdc`
* **Google Antigravity:** `.agents/skills/learn-from-code/SKILL.md`
* **OpenCode / Codex / Cline:** In your respective workspace skills folder.

---

## Usage

Once installed, your agent activates the skill automatically when reviewing code changes or practicing hands-on coding. You can also trigger it explicitly:

### 1. Reviewing Git Diffs & Commits (Mode A)

```text
/learn-from-code explain commit 0bb8705
```

```text
Review our current git diff using learn-from-code so I understand the architectural decisions before committing.
```

### 2. Hands-On Practice (Mode B)

```text
Use learn-from-code in Mode B to implement the payment retry backoff logic.
```

---

## The 5-Section Output Contract (Mode A)

Every Mode A explanation follows a standardized, predictable recipe:

1. **Big Picture (100% Plain-Language Macro Story):** Symptom, everyday root-cause analogy with natural `->` cause-and-effect chain, plain-language fix summary (zero file/function/variable names), and a 3–6 line `Before -> After` snippet visually showing 2 items colliding or transforming.
2. **Main Execution Flow:** 3–4 steps opening with the real-world user/business trigger, naming at most 2–3 primary functions.
3. **Core Code Steps (`Step 1 -> Step 2 -> Step 3`):** Up to 3 chronological steps with inline code annotations, a 2-sentence *Contrastive Why* (why naive approach fails), and a *Visual Dry-Run* (`input -> output` or `t1 -> t2 -> t3` timeline).
4. **Verification & Hidden Risks:** High-priority `⚠️ Defect Alert` (if bugs/unseen caller breakages exist), followed by verified facts with a `breakpoint` (`file:line` + exact **state invariant** to inspect), unverified domain risks, and cross-file **co-change coupling** rules.
5. **Debugging Check:** 1–2 single-focus boundary questions targeting the shown snippets to test mental retention (plus an optional 5-bullet repo gotcha note for cross-file coupling), pausing for your response.

---

## Agent Compatibility Matrix

| AI Coding Agent / Tool | Support | Installation Scope |
| :--- | :---: | :--- |
| **Claude Code (CLI)** | Full | Global & Project (`npx skills add`) |
| **Cursor (Composer & Agent)** | Full | Global & Project (`.cursor/rules/` / `.agents/`) |
| **Google Antigravity (AGY)** | Full | Global & Project (`.agents/skills/`) |
| **OpenCode CLI** | Full | Global & Project (`.agents/skills/`) |
| **Codex CLI** | Full | Global & Project (`.agents/skills/`) |
| **GitHub Copilot** | Full | Project instructions (`.github/copilot-instructions.md`) |
| **Cline / Roo Code** | Full | Project (`.clinerules/` / `.agents/`) |
| **Windsurf** | Full | Project (`.windsurfrules` / `.agents/`) |

---

## Repository Structure

```text
learn-from-code-skill/
├── skills/
│   └── learn-from-code/
│       └── SKILL.md       # Core specification & prompt contract (v2.0.0)
├── LICENSE                # MIT License
└── README.md              # Documentation & quickstart guide
```

---

## Contributing

Issues and pull requests are welcome! If you have suggestions for refining cognitive load rules or adding specialized language examples, feel free to open an issue or PR on [GitHub](https://github.com/HarleySw/learn-from-code-skill).

---

## License

[MIT](LICENSE) © HarleySw
