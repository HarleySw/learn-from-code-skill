# learn-from-code

[![skills.sh](https://skills.sh/b/HarleySw/learn-from-code-skill)](https://skills.sh/HarleySw/learn-from-code-skill)
[![Version](https://img.shields.io/badge/version-2.1.1-blue.svg)](https://github.com/HarleySw/learn-from-code-skill)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Agent Skills](https://img.shields.io/badge/standard-agentskills.io-purple.svg)](https://agentskills.io/)

An open-source [Agent Skill](https://agentskills.io/) that forces AI coding agents to teach codebase mental models, domain invariants, and debugging intuition from git diffs—preventing **comprehension debt** without slowing delivery down into a boring lecture.

---

## The Problem: Comprehension Debt

When AI coding models (Claude 3.7 Sonnet, OpenAI o3/Codex, Gemini) write hundreds of lines across multiple files in seconds, typing speed is no longer the bottleneck. The bottleneck is **human comprehension**.

Passive explanations fail software teams:
* ⚠️ **Split-Attention Overload:** Long prose paragraphs above code, duplicated inline comments, and redundant bullets below code repeating the same variable explanations.
* ⚠️ **Working Memory Overflow:** Dumping raw AST mechanics and minor utility helpers without explaining the primary causal pipeline.
* ⚠️ **The "Looks Good To Me" Trap:** Smooth explanations that feel clear while reading, but leave you with zero debugging intuition when production fails 2 days later.

`learn-from-code` fixes this by prioritizing **completeness of core mechanics over arbitrary length caps** and **3-second visual comprehension**: filtering out peripheral boilerplate while explaining every primary mechanism, guard condition, and dry-run step with **ASCII ruler diagrams, compact Markdown state trace tables, semantic traffic-light icons (`✅`, `❌`, `⚠️`), Subgoal-Labeled inline comments, and active retrieval checks**.

---

## Operating Modes

| Mode | Trigger | Workflow |
| :--- | :--- | :--- |
| **Mode A: Post-Implementation Review** *(Default)* | Reviewing a `git diff`, inspecting a commit, or after generating a feature | Delivers a structured **Two-Turn Output Contract**, anchors code mechanics to real-world business triggers, maps unseen caller risks, pauses with dynamically scaled debugging questions (Turn 1), and consolidates transferable patterns and blast radius rules after you reply (Turn 2). |
| **Mode B: In-Task Active Learning (`TODO(human)`)** | When you want hands-on muscle memory during implementation | Analyzes surrounding patterns, generates necessary boilerplate, and leaves the core decision point marked `TODO(human)` with a 4-bullet typed contract for you to fill in. |

---

## Key Principles & Cognitive Rules

* **Completeness of Core Mechanics Over Arbitrary Length Caps:** Filters out peripheral boilerplate and repetitive property mappings, while explaining every primary pipeline function, engineering decision step, guard condition, and grounded risk completely—never truncating important logic to fit a fixed line or bullet limit.
* **2–4 Line ASCII Ruler Diagrams & Subgoal-Labeled Arithmetic (*CHI 2024 ASCII Diagramming & Margulieux's Subgoal Labeling*):** Whenever a step involves string slicing (`Index + Length`, `Substring`, `slice`), byte offsets/padding, coordinate geometry (`X, Y`), or pointer ranges, includes a `< 76-char` **2–4 line ASCII Ruler Diagram** and inline **Subgoal Comments** with concrete small numbers so you can *see* the boundaries visually instead of doing mental arithmetic.
* **Compact Markdown State Trace Tables for Loops & Multi-Case Branches (*Python Tutor Model*):** Whenever tracing a loop (`for`, `while`, `.reduce`, `.find`) building an accumulator or evaluating multiple conditions across cases in `Visual Dry-Run`, renders a compact **4-column Markdown State Trace Table** (`Step / Case` | `Input / Current Token` | `Check Evaluated (✅ / ❌)` | `Resulting State / Output`) so your eyes can scan down a single column to see variables change step by step.
* **Semantic Traffic-Light Icons Only (`✅`, `❌`, `⚠️` — *Mayer's Signaling Principle*):** Uses strictly `❌` (broken states, overwritten data, rejected checks), `✅` (valid states, passed checks, working fix), and `⚠️` (`Defect Alert` in Section 4)—banning decorative emojis (`🚀`, `💡`, `✨`, `🔥`) that create visual noise.
* **Cross-Surface CLI & GUI Rendering Discipline:** Enforces a **76-character width cap** on all ASCII diagrams and inline code comments (preventing horizontal wrapping in 80-column CLI/TUI terminals like OpenCode/Claude Code and narrow ~400px IDE chat sidebars like Cursor/VS Code), keeps Markdown tables to at most 4 concise columns with zero unescaped `|` pipes, includes `filename:line` in visible link text, and bans raw HTML tags or `mermaid` blocks.
* **1-Line Syntax Anatomy Legend (*Mayer's Pre-Training Principle & Hermans' LTM Support*):** Whenever a `Before -> After` example or code snippet contains unfamiliar format tokens, protocol commands, graphics/bytecode operators, or regex syntax, a `< 76-char` Anatomy Legend comment at the top of the snippet decodes each token in plain words first.
* **Bridge Domain & Program Models (Zero Domain Hallucination):** Anchors code mechanics to the **real-world business event** (or **Runtime/System trigger** for pure infrastructure diffs) and verifies internal domain codes against repo docs/tests before explaining—explicitly labeling undocumented codes as *inferred hypotheses* rather than guessing.
* **Worked-Example Completeness (*Renkl, Atkinson & Derry*):** Every condition in a Section 3 code snippet must have its purpose explained and its downstream lines visible (never hidden behind `...`) before Section 5 tests how that rule applies to an edge case.
* **Unseen Caller, Persisted-Data, Unhappy-Path & Scale Audit:** Scans immediate callers **outside the `git diff`**, checks backward compatibility with existing persisted data (`localStorage`/DB/cache/URL) whenever key formats or schemas change, audits `catch`/`fallback` blocks for silent failures, and flags `O(N^2)` / `N+1` scale bottlenecks.
* **Delocalized Co-Change & Far-Transfer Consolidation (*Shute 2008; Gick & Holyoak 1983; Bohner & Arnold 1996*):** Once you answer the Section 5 check, Turn 2 delivers line-pinpointed **Formative Feedback (Section 6)**, extracts a reusable **Transferable Engineering Pattern (Section 7)** for future projects, and maps the **Maintenance Blast Radius & Missing Edge-Case Test (Section 8)**.
* **Automatic Localization & Mechanical Plain-Language Syntax:** Automatically responds in your conversation language (English, Vietnamese, Japanese, etc.) while enforcing `Concrete Actor + Strong Action Verb` (zero zombie nouns), `One Word = One Meaning` (zero synonym rotation), and `Old-Before-New` sentence chaining across all models.

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

## The Two-Turn Output Contract (Mode A)

Every Mode A review follows a structured two-turn learning loop:

### Turn 1: Understand the Current Change (Sections 1–5)
1. **Big Picture (100% Plain-Language Macro Story):** Symptom, everyday root-cause analogy with separate single-idea sentences linked by `Old-Before-New` transitions, plain-language fix summary (zero file/function/variable names), and a visual `Before -> After` snippet (`❌` vs `✅`) showing items colliding or transforming (prefaced by a `< 76-char` Syntax Anatomy Legend whenever unfamiliar format/protocol tokens appear).
2. **Main Execution Flow:** Chronological steps opening with the real-world user/business trigger and tracing data through the Primary Pipeline Functions (with `filename:line` in visible link labels).
3. **Core Code Steps (`Step 1 -> Step 2 -> ...`):** Steps scaled to the actual number of distinct engineering decisions, with complete code blocks, `< 76-char` Subgoal-Labeled inline comments and **2–4 line ASCII Ruler Diagrams** for index/slice/coordinate math, a thorough *Contrastive Why* (why naive approaches fail for every guard), and a **compact 4-column Markdown State Trace Table** (or vertical multi-line walkthrough) in *Visual Dry-Run*.
4. **Verification & Hidden Risks:** High-priority `⚠️ Defect Alert` (if bugs/unseen caller breakages exist), followed by verified facts with a `breakpoint` (`filename:line` + exact **state invariant** to inspect), and all grounded runtime/persisted-data/scale risks.
5. **Debugging Check (Dynamic Question Selection):** Boundary questions dynamically scaled to the distinct mechanisms and edge cases covered in Sections 3 and 4, pausing for your response.

### Turn 2: Consolidate & Transfer (Sections 6–8, After You Reply — No Arbitrary Length or Item Caps)
6. **Answer Review & Diagnostic Feedback (*Shute's Formative Feedback*):** Thoroughly reviews every question and every part of your reasoning (`✅`), tracing concrete values through the exact `filename:line` in Section 3 (with a State Trace Table, ASCII Ruler, or step-by-step walkthrough) for any missed boundary.
7. **Transferable Engineering Patterns (*Gick & Holyoak's Schema Induction*):** Extracts **all distinct reusable engineering patterns** in the change (covering **Pattern Name & Mental Model**, **When to Reuse It in Other Projects**, and **When NOT to Use It (Tradeoff Limit)** for each pattern).
8. **Maintenance Blast Radius & Next Actions (*Bohner & Arnold's Change Impact Analysis*):** Maps all **Co-Change Couplings** across files/APIs/stores (`filename:line`) and lists all valuable **missing edge-case unit tests** and defect fixes.

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
│       └── SKILL.md       # Core specification & prompt contract (v2.1.1)
├── LICENSE                # MIT License
└── README.md              # Documentation & quickstart guide
```

---

## Contributing

Issues and pull requests are welcome! If you have suggestions for refining cognitive load rules or adding specialized language examples, feel free to open an issue or PR on [GitHub](https://github.com/HarleySw/learn-from-code-skill).

---

## License

[MIT](LICENSE) © HarleySw
