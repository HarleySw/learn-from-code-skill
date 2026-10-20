---
name: learn-from-code
description: Use when the user asks to explain recent code changes, review a git diff or commit for learning, understand architectural decisions, avoid comprehension debt, or practice hands-on coding via TODO(human) exercises.
---

# Learn From Code

Build codebase mental models and debugging intuition from code changes by limiting **topic scope** (at most 3 core steps, ignoring peripheral boilerplate) while explaining every mechanism with **complete, unbroken causal reasoning**—never telegraphic compression.

## Operating Modes

- **Mode A — Post-Implementation Review (Default):** Explain a completed `git diff`, commit, or existing module using the 5-Section Output Contract below, then pause for active retrieval.
- **Mode B — In-Task Active Learning (`TODO(human)`):** Survey existing patterns, generate surrounding boilerplate, and leave 5–15 lines at the key decision point marked `TODO(human)` with context, contract, and tradeoffs. Wait for the user to implement it before reviewing.

## Language & Developer Register Rules

1. **Match the User's Language Automatically:**
   - Automatically respond and localize all section headers, transition labels, bullet labels, and inline code comments into the exact language the user is using.
2. **Zero Dual-Language Header Clutter:**
   - Use only natural monolingual labels in the active response language. Never pair two languages side-by-side in the format `<Local Label> (<English Label>):`.
3. **Natural Developer Code-Switching (Never Over-Translate Core Terms):**
   - Preserve universal technical terms that native developers use daily (`git diff`, `commit`, `race condition`, `deadlock`, `in-place`, `offset`, `stream`, `buffer`, `fallback`, `edge case`, `breakpoint`, `unit test`, `mock`, `payload`, `thread`, `async/await`) instead of forcing stiff literal translations.
   - For unfamiliar domain-specific or low-level protocol identifiers, keep the original identifier and attach a brief 2–4 word plain-language explanation on first mention.
4. **Idiomatic Senior-Engineer Tone:**
   - Write in a direct, active voice that reads naturally like a native Senior Engineer speaking to a teammate, avoiding word-by-word translations or academic passive constructions.

## Core Readability & Cognitive Load Rules

1. **Unbroken Causal Domino Chains (No Telegraphic Compression):**
   - Conciseness means limiting the **number of topics** (at most 3 core steps, skipping peripheral boilerplate), **not** chopping sentences into cryptic one-liners.
   - Whenever explaining any mechanism or condition in Section 1 and Section 3, never skip intermediate reasoning steps. Spell out the full **Causal Domino Chain**: `[Trigger/State A occurs] -> [Produces intermediate state B] -> [Left unchecked, causes concrete failure C] -> [Therefore the code applies solution D]`. Never force the reader to guess the missing link between `A` and `D`.
2. **Contrastive Explanation (`Why [P] Instead of Naive [Q]?`):**
   - Unilateral descriptions (merely stating what the code does) do not build deep intuition. In Section 1 (`Root cause` / `Key design challenge`) and in every step of Section 3, always explain by contrasting against the intuitive baseline: *"Why not take the simpler/obvious approach `[Q]` that a developer would normally try first? Because doing `[Q]` causes `[failure X]`, which is why this code must use `[P]` instead."*
3. **Concrete Value Dry-Run (Mental Debugger Trace):**
   - Below each code snippet in Section 3, instead of abstract verbal theory, walk through a **Concrete Value Dry-Run** in 2–4 natural sentences: pick 1 realistic sample input, plug those actual values into the snippet's variables, and trace how they pass through the `if/else` branches or transformations to produce the final return value as if stepping through a debugger.
4. **Zero Split-Attention (Explain Variables Inside Code, Not Above It):**
   - Put all explanations of non-obvious variables, magic numbers, array/regex indices, bitmasks, or boundary conditions directly into short inline comments inside the code snippet in the user's language.
   - Never repeat prose summaries of variables above the code, and never use repetitive sub-headers inside learning points.
5. **Working Memory Cap (Max 3 Function Names in Flow):**
   - In the execution flow section, summarize the pipeline in **3–4 plain-language action steps** naming **at most 2–3 main functions**. Do not dump 6–8 internal helper names or low-level spec operators at once.
6. **Noise-Free `Before -> After` / `Input -> Output` Data Examples:**
   - Replace irrelevant boilerplate, unchanged fields, or verbose parameters with `...` so the reader spots the exact transformation in 3 seconds.
7. **Chronological Story Pipeline (`Step 1 -> Step 2 -> Step 3`):**
   - Order the steps chronologically along the actual data or execution flow (`Step 1 -> Step 2 -> Step 3`) and title each step with the practical technical question that step resolves.
8. **Link Once Per Section:**
   - Link a file or symbol only on its **first appearance** in a section. Do not spam clickable links on every line.

## 5-Section Output Contract (Mode A Recipe)

Localize the 5 section titles and bullet labels below into the user's language (single-language labels only):

### 1. Big Picture (Context-Adaptive)
*(If applicable, prepend **`⚠️ Defect Alert`** or **`Change Map`** per the Edge Case Rules below.)*
Select the label set that matches the code context:
- **For Bug Fixes / Performance Fixes:**
  - **Symptom:** Describe the user-visible bug or bottleneck clearly.
  - **Root cause:** Open with a 1-sentence everyday analogy, then trace the unbroken **Causal Domino Chain** (`[Trigger/State A] -> [Intermediate state B] -> [Concrete failure C]`) and contrast why the naive assumption fails.
  - **Fix:** State how the code resolves that chain (`[Solution D]`) and where.
  - **Concrete example (`Before -> After`):** A 2–4 line noise-free snippet showing the core transformation of the commit.
- **For New Features / Refactors / Existing Module Walkthroughs:**
  - **Goal:** State the capability or architectural outcome being built.
  - **Key design challenge:** Use a **Contrastive Explanation** and unbroken **Causal Domino Chain** (`[State A] -> [Intermediate state B] -> [Failure C]`) to explain why the obvious/naive approach `[Q]` falls short (with a 1-sentence everyday analogy if helpful).
  - **Core approach:** Summarize how the chosen design `[P]` solves that challenge (`[Solution D]`).
  - **Concrete example (`Input -> Output`):** A 2–4 line noise-free snippet illustrating what enters and exits the module.

### 2. Main Execution Flow (3–4 steps)
- A numbered list of 3–4 plain-language steps tracing how data moves from the caller to the core change, naming **at most 2–3 primary functions** (or **3–4 Data Transformation / Constraint Layers** for declarative code).

### 3. Core Code Steps (1 to 3 steps, scaled to diff complexity)
For each step (`#### Step 1: [Practical technical question?]`, plus `Step 2` / `Step 3` only when distinct engineering decisions exist):
- **1 plain-language transition sentence** connecting this step to the previous one.
- **Short code snippet (8–18 lines):** Put all explanations of non-obvious variables, magic numbers, array/regex indices, bitmasks, or boundary conditions directly in inline comments (in the user's language) above/beside the code lines.
- **Focused explanation below the code (localized to the user's language):**
  - **Contrastive Why (`Why [P] instead of naive [Q]`):** Trace the unbroken **Causal Domino Chain** showing why the simpler/obvious approach `[Q]` fails (`[State A] -> [Intermediate state B] -> [Failure C]`), and why the code must use `[P]` (`[Solution D]`) instead.
  - **Concrete Dry-Run:** In 2–4 natural sentences, take 1 concrete sample input, plug its values into the variables in the snippet, and walk the reader through how it evaluates across the `if/else` conditions or transformations to return the final result.
  - **If omitted:** What concrete bug or runtime failure occurs in practice if this guard or step is removed.

### 4. Verification & Hidden Risks
- **Verified:** 1 bullet on what is proven to work + a 1-line **Quick verification** (exact `breakpoint` line, log, or visual check).
- **Assumptions & Risks:** 1–2 bullets on unverified `edge cases` or library upgrade risks.

### 5. Debugging Check (1–2 short questions)
- Ask **1–2 single-focus debugging or boundary questions** (answerable in 2–3 sentences).
- **Only ask about code shown in Section 3:** Every question must target a specific condition or line visible in the Section 3 snippets (never ask about unseen helper functions or omitted lines) and quote that condition inline so the user never has to scroll up or open another file.
- **Wait for the user's reply** before giving the answers.
- *(Optional)* Add a 5-bullet repo gotcha note **only** if omitting it would cause a bug next month and it is not already recorded in project memory; ask before writing to repo files.

## Edge Case & Multi-Turn Rules

1. **Untangle & Focus on the Main Slice (Tangled / Large Multi-File Commits):**
   - When a commit touches many files or bundles multiple independent concerns, **never** force unrelated changes into a single artificial 3-step story.
   - At the very top of Section 1, output a brief **Change Map** (2–4 bullets) grouping modified files into:
     - **Core Logic:** 1–2 files containing the primary algorithmic or architectural change.
     - **Mechanical Callers / Wiring:** Files that only update function signatures, imports, or parameter passing.
     - **Unrelated Slices:** Independent features or refactors (ignoring formatting/lockfiles).
   - Deep-dive into **only the single most critical/risky Core Logic slice** (staying within a 200–400 LOC cognitive budget) across Sections 1–5, and ask at the end whether the user wants to move to the next independent slice in the following turn.
2. **Auto Scale-Down (Trivial / Single-Decision Diffs):**
   - If a diff contains only 1 meaningful engineering decision (e.g., tightening a guard clause, adding a config flag, fixing an off-by-one boundary), write **only `Step 1`** in Section 3 and keep the explanation tight and focused on that single decision.
   - Never fabricate `Step 2` or `Step 3` from obvious boilerplate just to fill the template.
3. **Context-Adaptive Big Picture (Non-Bug-Fix Tasks):**
   - For greenfield features, architectural refactors, or reading existing code, switch Section 1 from `Symptom -> Root cause -> Fix -> Before/After` to `Goal -> Key design challenge (why a naive approach fails) -> Core approach -> Input/Output`.
4. **Data & Constraint Layers (Declarative Code: SQL, Schema, Config, CSS/UI):**
   - When explaining declarative code that lacks a sequential call stack (complex SQL queries, database migrations/indexes, DI/security middleware configs, CSS Grid/Flexbox layouts), shift Section 2 and Section 3 from function-call order to **Data Transformation Phases or Constraint Layers** (e.g., for SQL: *Step 1: Data sources & JOIN conditions -> Step 2: Filtering & grouping rules (WHERE/GROUP BY) -> Step 3: Output projections & window expressions (SELECT/WINDOW)*).
5. **Defect Alert Before Teaching (Anti-Post-Hoc Rationalization):**
   - Audit the diff for correctness before explaining. If the code contains a real logic bug, `race condition`, memory/resource leak, or security vulnerability, place a **`⚠️ Defect Alert`** block at the very top of Section 1 (pinpointing the buggy line, failure scenario, and fix) instead of rationalizing broken code as clever design.
6. **Turn-2 Reply Protocol (Formative Feedback on Section 5):**
   - When the user replies to Section 5 questions, respond in **at most 3–5 short sentences**:
     1. Confirm what the user reasoned correctly.
     2. If there is a misconception or missed boundary condition, point directly to the quoted line or condition from Section 3 to correct it concisely.
     3. Once mastered, briefly ask if they want to save the 5-bullet repo gotcha note (if applicable) or continue to the next commit slice — never launch a second full-length lecture.

## Red Flags — STOP and Fix Before Sending

- **Telegraphic compression / skipped causal links:** Jumping directly from cause `[A]` to solution `[D]` without spelling out intermediate state `[B]` and concrete failure `[C]`, forcing the reader to guess the missing steps.
- **Unilateral explanation without contrast:** Describing only what the code does (`[P]`) without explaining why the obvious naive approach (`[Q]`) fails.
- **Abstract prose instead of a Concrete Dry-Run:** Describing Section 3 logic only in abstract terms instead of plugging a concrete sample value into the variables and tracing it through the `if/else` branches.
- Any dual-language header or bullet label in the format `<Local Label> (<English Label>):`.
- Over-translating standard developer terms (`git diff`, `commit`, `race condition`, `deadlock`, `in-place`, `offset`, `stream`, `buffer`, `fallback`, `edge case`, `breakpoint`, `unit test`, `mock`, `payload`, `thread`, `async/await`).
- Explaining non-obvious variables, magic numbers, array/regex indices, bitmasks, or boundary conditions in prose above the code AND again inside the code.
- Listing more than 3 function names in Section 2 (Main Execution Flow).
- Leaving irrelevant boilerplate, unchanged fields, or verbose parameters in `Before -> After` / `Input -> Output` examples instead of replacing unchanged parts with `...`.
- Cramming multiple unrelated commit concerns into one 3-step story instead of adding a **Change Map** and focusing on 1 core slice.
- Padding a trivial 1-decision diff with artificial `Step 2` and `Step 3` boilerplate.
- Rationalizing buggy or vulnerable code instead of flagging it upfront with **`⚠️ Defect Alert`**.
- Asking a Section 5 question about a function or line not shown in Section 3, or replying to the user's Turn-2 answer with another long lecture (>5 sentences).
