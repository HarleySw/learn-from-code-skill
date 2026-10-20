# Learn From Code — Agent Skill

An [Agent Skill](https://agentskills.io/) that helps software engineers build deep codebase mental models, debugging intuition, and architectural judgment from AI-generated code or `git diff` changes—without turning fast delivery into a slow, overwhelming lecture.

[![skills.sh](https://skills.sh/b/HarleySw/learn-from-code-skill)](https://skills.sh/HarleySw/learn-from-code-skill)

## Why This Skill?

When AI generates code faster than humans can read it, teams accumulate **comprehension debt**. Passive code explanations often backfire due to:
- **Split-Attention Overload:** Prose above code, inline comments, and bullets below code repeating the same variable explanations.
- **Working Memory Overflow:** Dumping 7–8 internal helper names and low-level protocol operators at once.
- **Illusion of Competence:** Smooth explanations that feel easy to read but fail to build real debugging intuition.

`learn-from-code` solves this by enforcing a **90–120 second reading budget (350–500 prose words)**, inline code annotations, chronological data-flow storytelling, and active debugging checks.

## Operating Modes

| Mode | When It Runs | How It Works |
| :--- | :--- | :--- |
| **Mode A: Post-Implementation Review** *(Default)* | After generating code, reviewing a `git diff`, or inspecting a commit | Explains changes using a strict **5-Section Output Contract**, separates verified facts from hypotheses, and pauses with 1–2 self-contained debugging questions. |
| **Mode B: In-Task Active Learning (`TODO(human)`)** | During feature implementation when you want hands-on practice | Surveys existing repo patterns, generates surrounding boilerplate, and leaves 5–15 lines at the core decision point marked `TODO(human)` for you to implement and get reviewed. |

## Key Features

- **Zero Split-Attention:** Explanations of variables, boundary conditions, and Regex groups live directly inside short inline comments in the code snippet—never duplicated in prose above the code.
- **Working Memory Cap:** The execution flow summarizes the pipeline in 3–4 plain-language steps naming at most 2–3 primary functions.
- **Noise-Free `Before -> After` Examples:** Strips irrelevant hex bytes and boilerplate into `...` so you can spot the exact data transformation in 3 seconds.
- **Language-Agnostic & Natural Developer Register:** Automatically responds in your conversation language (English, Vietnamese, Japanese, Korean, etc.) with idiomatic senior-engineer phrasing, zero bilingual header clutter, and natural developer code-switching (keeping universal terms like `race condition`, `deadlock`, `in-place`, `offset`, `fallback`, `breakpoint`).
- **Active Retrieval & Debugging Focus:** Ends with 1–2 single-focus boundary or failure-mode questions grounded strictly in the shown code snippets.

## Installation

Install using the [Skills CLI](https://skills.sh/):

```bash
npx skills add HarleySw/learn-from-code-skill
```

Install globally for all supported agents (Claude Code, Cursor, Antigravity, Gemini CLI, Codex, OpenCode, GitHub Copilot):

```bash
npx skills add -g HarleySw/learn-from-code-skill
```

Update to the latest version:

```bash
npx skills update
```

## Usage

Once installed, your AI coding agent will automatically activate `learn-from-code` when you ask to review changes or practice hands-on coding. You can also invoke it explicitly:

### Post-Implementation Review (Mode A)

```text
/learn-from-code explain commit f123dec5e5c
```

```text
Review our current git diff using learn-from-code so I understand the edge cases before committing.
```

### Hands-On Active Learning (Mode B)

```text
Use learn-from-code in Mode B (TODO(human)) to implement the retry and backoff logic in PaymentService.
```

## 5-Section Output Contract (Mode A)

1. **Big Picture (Problem & Core Idea):** Symptom, plain-language root-cause analogy, fix summary, and a noise-free `Before -> After` data snippet.
2. **Main Execution Flow (3–4 steps):** High-level trace naming at most 2–3 primary functions.
3. **Core Code Steps (`Step 1 -> Step 2 -> Step 3`):** Up to 3 chronological steps titled as practical questions, with inline code comments and 2 crisp bullets below each snippet (*Why this design* & *If omitted*).
4. **Verification & Hidden Risks:** Proven facts (with a 1-line quick verification step) separated from unverified assumptions and upgrade risks.
5. **Debugging Check:** 1–2 self-contained boundary/failure questions that pause for your response.

## Repository Structure

```text
learn-from-code-skill/
├── skills/
│   └── learn-from-code/
│       └── SKILL.md       # Agent skill instructions and output contract
├── LICENSE                # MIT License
└── README.md              # Documentation
```

## License

MIT
