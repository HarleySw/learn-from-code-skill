# learn-from-code

[![skills.sh](https://skills.sh/b/HarleySw/learn-from-code-skill)](https://skills.sh/HarleySw/learn-from-code-skill)
[![Version](https://img.shields.io/badge/version-2.1.0-blue.svg)](https://github.com/HarleySw/learn-from-code-skill)
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

`learn-from-code` fixes this by prioritizing **completeness of core mechanics over arbitrary length caps**: filtering out peripheral boilerplate while explaining every primary mechanism, guard condition, and vertical dry-run step with **complete, unbroken causal reasoning**, Subgoal-Labeled inline comments, and active retrieval checks.

---

## Operating Modes

| Mode | Trigger | Workflow |
| :--- | :--- | :--- |
| **Mode A: Post-Implementation Review** *(Default)* | Reviewing a `git diff`, inspecting a commit, or after generating a feature | Delivers a structured **Two-Turn Output Contract**, anchors code mechanics to real-world business triggers, maps unseen caller risks, pauses with dynamically scaled debugging questions (Turn 1), and consolidates transferable patterns and blast radius rules after you reply (Turn 2). |
| **Mode B: In-Task Active Learning (`TODO(human)`)** | When you want hands-on muscle memory during implementation | Analyzes surrounding patterns, generates necessary boilerplate, and leaves the core decision point marked `TODO(human)` with a 4-bullet typed contract for you to fill in. |

---

## Key Principles & Cognitive Rules

* **Completeness of Core Mechanics Over Arbitrary Length Caps:** Filters out peripheral boilerplate and repetitive property mappings, while explaining every primary pipeline function, engineering decision step, guard condition, and grounded risk completely—never truncating important logic to fit a fixed line or bullet limit.
* **Zero Split-Attention & Subgoal-Labeled Arithmetic (*Margulieux, Guzdial & Catrambone*):** Explanations of boundary conditions, magic numbers, regex groups, and **all index/offset/slice math (`Index + Length`, `Substring`, `slice`)** live strictly inside short inline **Subgoal Comments** illustrated with small concrete numbers—never left uncommented as "basic syntax" or duplicated in prose above the code.
* **1-Line Syntax Anatomy Legend (*Mayer's Pre-Training Principle & Hermans' LTM Support*):** Whenever a `Before -> After` example or code snippet contains unfamiliar format tokens, protocol commands, graphics/bytecode operators, or regex syntax, a 1-line Anatomy Legend comment at the top of the snippet decodes each token in plain words first.
* **Bridge Domain & Program Models (Zero Domain Hallucination):** Anchors code mechanics to the **real-world business event** (or **Runtime/System trigger** for pure infrastructure diffs) and verifies internal domain codes against repo docs/tests before explaining—explicitly labeling undocumented codes as *inferred hypotheses* rather than guessing.
* **One Clear Idea Per Sentence & Vertical Multi-Line Dry-Runs (*Mayer & Clark's Personalization Principle*):** Explains *why* naive alternatives fail using separate, single-idea sentences linked by `Old-Before-New` transitions (never run-on `->` chains), and breaks `Visual Dry-Run` scenarios vertically into indented sub-bullets (**Input**, **Step-by-step / Loop trace** matching numbered code comments, and **Result**).
* **Interleaved Timelines for Async/Concurrency:** Renders async lifecycles, abort controllers, locks, and race conditions as vertical chronologically interleaved timelines (`t1`, `t2`, `t3`).
* **Worked-Example Completeness (*Renkl, Atkinson & Derry*):** Every condition in a Section 3 code snippet must have its purpose explained and its downstream lines visible (never hidden behind `...`) before Section 5 tests how that rule applies to an edge case.
* **Unseen Caller, Persisted-Data, Unhappy-Path & Scale Audit:** Scans immediate callers **outside the `git diff`**, checks backward compatibility with existing persisted data (`localStorage`/DB/cache/URL) whenever key formats or schemas change, audits `catch`/`fallback` blocks for silent failures, and flags `O(N^2)` / `N+1` scale bottlenecks.
* **Delocalized Co-Change & Far-Transfer Consolidation (*Shute 2008; Gick & Holyoak 1983; Bohner & Arnold 1996*):** Once you answer the Section 5 check, Turn 2 delivers line-pinpointed **Formative Feedback (Section 6)**, extracts a reusable **Transferable Engineering Pattern (Section 7)** for future projects, and maps the **Maintenance Blast Radius & Missing Edge-Case Test (Section 8)**.
* **State Invariant Breakpoints:** Pairs suggested breakpoint lines with the exact runtime state invariant expression to inspect, isolating defects before state infection spreads.
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
1. **Big Picture (100% Plain-Language Macro Story):** Symptom, everyday root-cause analogy with separate single-idea sentences linked by `Old-Before-New` transitions, plain-language fix summary (zero file/function/variable names), and a visual `Before -> After` snippet showing items colliding or transforming (prefaced by a 1-line Syntax Anatomy Legend whenever unfamiliar format/protocol tokens appear).
2. **Main Execution Flow:** Chronological steps opening with the real-world user/business trigger and tracing data through the Primary Pipeline Functions.
3. **Core Code Steps (`Step 1 -> Step 2 -> ...`):** Steps scaled to the actual number of distinct engineering decisions, with complete code blocks, Subgoal-Labeled inline comments (with concrete small numbers for all index/slice math), a thorough *Contrastive Why* (why naive approaches fail for every guard), and a vertical multi-line *Visual Dry-Run* (**Input**, **Step-by-step / Loop trace**, and **Result**).
4. **Verification & Hidden Risks:** High-priority `⚠️ Defect Alert` (if bugs/unseen caller breakages exist), followed by verified facts with a `breakpoint` (`file:line` + exact **state invariant** to inspect), and all grounded runtime/persisted-data/scale risks.
5. **Debugging Check (Dynamic Question Selection):** Boundary questions dynamically scaled to the distinct mechanisms and edge cases covered in Sections 3 and 4, pausing for your response.

### Turn 2: Consolidate & Transfer (Sections 6–8, After You Reply)
6. **Answer Review & Diagnostic Feedback (*Shute's Formative Feedback*):** Confirms what you reasoned correctly and traces a concrete value through the exact line in Section 3 for any missed boundary.
7. **Transferable Engineering Pattern (*Gick & Holyoak's Schema Induction*):** Extracts the industry **Pattern Name & Mental Model**, **When to Reuse It in Other Projects**, and **When NOT to Use It (Tradeoff Limit)**.
8. **Maintenance Blast Radius & Next Action (*Bohner & Arnold's Change Impact Analysis*):** Maps **Co-Change Coupling** across files/APIs/stores and proposes the single highest-value **missing edge-case unit test** (or defect fix).

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
