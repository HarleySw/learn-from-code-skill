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

1. **No Premature Complexity (Start Directly With the Problem):**
   - Section 1 (`Big Picture`) must **always** open directly with `Symptom -> Root cause -> Fix -> Concrete example` (or `Goal -> Key design challenge -> Core approach -> Concrete example`).
   - Never place a `⚠️ Defect Alert` at the top of Section 1 before the reader understands the domain and code steps; place `⚠️ Defect Alert` at the **top of Section 4 (`Verification & Hidden Risks`)** after Section 3.
   - Only output a `Change Map` when a commit genuinely bundles **multiple unrelated features/concerns** (e.g., changing payment UI + changing report export in one commit). If client, server, and test files in a commit all solve **one cohesive problem**, never print a `Change Map`.
2. **No Wall-of-Prose — Contrastive Why + Visual Arrow (`->`) Dry-Run:**
   - Below each code snippet in Section 3, keep **only 2 items** (never add a separate 3rd `If omitted` bullet that repeats the failure):
     1. **Contrastive Why (Why not the naive approach / what breaks without it):** At most 2 concise sentences explaining why the simpler/naive approach `[Q]` (or omitting this check) fails (`[State A] -> [Intermediate state B] -> [Failure C]`) and why `[P]` prevents it.
     2. **Visual Dry-Run:** Format strictly as **2–3 sub-bullets using arrows (`->`)** with short, memorable sample values (never write a dense 100-word prose paragraph or use cryptic test IDs). Example shape:
        - Case 1 (`input = "Order_A"`, `id = "1"`) -> builds `scopedId = "Order_A_1"` -> Kept.
        - Case 2 (`input = "Order_B"`, `id = "1"`) -> builds `scopedId = "Order_B_1"` (distinct from `"Order_A_1"`) -> Both kept!
3. **Concrete Evaluated Examples Inside Inline Code Comments:**
   - Put all explanations of non-obvious variables, magic numbers, array/regex indices, bitmasks, or boundary conditions directly into short inline comments inside the code snippet in the user's language.
   - Whenever a code line contains a long string interpolation, complex regex, or multi-part expression, the inline comment right above that line **must include a concrete evaluated example string/value** so the reader grasps the result in 1 second without mentally parsing the expression.
   - Never repeat prose summaries of variables above the code.
4. **Working Memory Cap (Max 3 Function Names in Flow):**
   - In the execution flow section, summarize the pipeline in **3–4 plain-language action steps** naming **at most 2–3 main functions**. Do not dump 6–8 internal helper names or low-level spec operators at once.
5. **Noise-Free `Before -> After` / `Input -> Output` Data Examples:**
   - Replace irrelevant boilerplate, unchanged fields, or verbose parameters with `...` so the reader spots the exact transformation in 3 seconds.
6. **Chronological Story Pipeline (`Step 1 -> Step 2 -> Step 3`):**
   - Order the steps chronologically along the actual data or execution flow (`Step 1 -> Step 2 -> Step 3`) and title each step with the practical technical question that step resolves.
7. **Link Once Per Section:**
   - Link a file or symbol only on its **first appearance** in a section. Do not spam clickable links on every line.

## 5-Section Output Contract (Mode A Recipe)

Localize the 5 section titles and bullet labels below into the user's language (single-language labels only):

### 1. Big Picture (Context-Adaptive — Macro / System Behavior Level)
Explain the causal chain at the **Macro / System behavior level** (leaving Micro / variable- and line-level mechanics for Section 3). Always start directly with the 4 bullets below (only prepend a `Change Map` if the commit mixes unrelated features):
- **For Bug Fixes / Performance Fixes:**
  - **Symptom:** Describe the user-visible bug or bottleneck clearly.
  - **Root cause:** Open with a 1-sentence everyday analogy, then trace the macro **Causal Domino Chain** (`[Trigger/State A] -> [Intermediate system state B] -> [Concrete failure C]`) and contrast why the naive system assumption fails.
  - **Fix:** State the high-level architectural/logic mechanism changed (`[Solution D]`) and where.
  - **Concrete example (`Before -> After`):** A 2–4 line noise-free snippet showing the core transformation of the commit.
- **For New Features / Refactors / Existing Module Walkthroughs:**
  - **Goal:** State the capability or architectural outcome being built.
  - **Key design challenge:** Use a **Contrastive Explanation** and macro **Causal Domino Chain** (`[State A] -> [Intermediate system state B] -> [Failure C]`) to explain why the obvious/naive approach `[Q]` falls short (with a 1-sentence everyday analogy if helpful).
  - **Core approach:** Summarize how the chosen design `[P]` solves that challenge (`[Solution D]`).
  - **Concrete example (`Input -> Output`):** A 2–4 line noise-free snippet illustrating what enters and exits the module.

### 2. Main Execution Flow (3–4 steps)
- A numbered list of 3–4 plain-language steps tracing how data moves from the caller to the core change, naming **at most 2–3 primary functions** (or, for declarative code like SQL, Schema, Config, and CSS/UI without a sequential call stack, **3–4 Data Transformation Phases or Constraint Layers** such as *Step 1: Data sources & JOINs -> Step 2: Filtering & grouping -> Step 3: Output projections*).

### 3. Core Code Steps (1 to 3 steps, scaled to diff complexity — Micro / Code Level)
For each step (`#### Step 1: [Practical technical question?]`, plus `Step 2` / `Step 3` only when distinct engineering decisions exist):
- **1 plain-language transition sentence** connecting this step to the previous one.
- **Short code snippet (8–18 lines):** Put all explanations of non-obvious variables, magic numbers, array/regex indices, bitmasks, or boundary conditions directly in inline comments (in the user's language) above/beside the code lines. For long string interpolations, regexes, or complex expressions, include a **concrete evaluated example** inside the comment (e.g., `// Match full key (e.g. k === "OrderA_1_Morning_0730")`).
- **Focused explanation below the code (2 items only, localized to the user's language):**
  - **Contrastive Why (Why not naive `[Q]` / what breaks if omitted):** At most 2 concise sentences explaining why the simpler/naive approach `[Q]` (or omitting this step) fails and what concrete bug occurs in practice.
  - **Visual Dry-Run:** 2–3 sub-bullets using arrows (`->`) and short, memorable sample values tracing how the input evaluates across the variables and branches to the final result.

### 4. Verification & Hidden Risks
- **`⚠️ Defect Alert` *(Only if a bug/flaw exists in the diff)*:** If the code introduces a logic bug, `race condition`, memory/resource leak, or security flaw, highlight it right here at the top of Section 4 (pinpointing the exact line from Section 3, failure scenario with `->` steps, and fix)—and do not repeat it in the bullets below.
- **Verified:** 1 bullet on what is proven to work + a 1-line **Quick verification** (exact `breakpoint` line, test command, or visual check).
- **Assumptions & Risks:** 1–2 bullets on remaining unverified `edge cases` or library upgrade risks.

### 5. Debugging Check (1–2 short questions)
- Ask **1–2 single-focus debugging or boundary questions** (answerable in 2–3 sentences).
- **Only ask about code shown in Section 3:** Every question must target a specific condition or line visible in the Section 3 snippets (never ask about unseen helper functions or omitted lines) and quote that condition inline so the user never has to scroll up or open another file.
- **Wait for the user's reply** before giving the answers.
- *(Optional)* Add a 5-bullet repo gotcha note **only** if omitting it would cause a bug next month and it is not already recorded in project memory; ask before writing to repo files.

## Edge Case & Multi-Turn Rules

1. **Untangle Multi-Concern Commits (Only When Truly Tangled):**
   - Only output a brief **Change Map** at the top of Section 1 when a commit bundles **multiple unrelated features or concerns**. Never print a `Change Map` when client, server, and test files all implement a single cohesive fix or feature.
   - When a commit is genuinely tangled, deep-dive into **only the single most critical Core Logic slice** (200–400 LOC budget) across Sections 1–5, and ask at the end whether the user wants to move to the next slice in the following turn.
2. **Auto Scale-Down (Trivial / Single-Decision Diffs):**
   - If a diff contains only 1 meaningful engineering decision (e.g., tightening a guard clause, adding a config flag, fixing an off-by-one boundary), write **only `Step 1`** in Section 3 and keep the explanation tight and focused on that single decision.
   - Never fabricate `Step 2` or `Step 3` from obvious boilerplate just to fill the template.
3. **Defect Alert in Section 4 (Anti-Post-Hoc Rationalization):**
   - Audit the diff for correctness before explaining. Never rationalize broken code as clever design—after walking through the code in Section 3, flag any real logic bug, `race condition`, leak, or vulnerability with **`⚠️ Defect Alert`** at the top of Section 4.
4. **Turn-2 Reply Protocol (Formative Feedback on Section 5):**
   - When the user replies to Section 5 questions, respond in **at most 3–5 short sentences**:
     1. Confirm what the user reasoned correctly.
     2. If there is a misconception or missed boundary condition, point directly to the quoted line or condition from Section 3 to correct it concisely.
     3. Once mastered, briefly ask if they want to save the 5-bullet repo gotcha note (if applicable) or continue to the next commit slice — never launch a second full-length lecture.

## Red Flags — STOP and Fix Before Sending

- **Premature complexity at the top of Section 1:** Putting a `⚠️ Defect Alert` before Section 3, or printing a `Change Map` for a single-purpose commit where client/server/test files all solve the same problem.
- **Wall-of-prose Dry-Run or 3rd `If omitted` bullet in Section 3:** Writing a dense paragraph for the dry-run instead of 2–3 arrow (`->`) bullets with short, memorable sample names, or keeping a separate `If omitted` bullet that repeats the `Contrastive Why`.
- **Missing concrete example in complex code comments:** Leaving a long string interpolation or regex in Section 3 without an evaluated example string in its inline comment.
- **Telegraphic compression / skipped causal links:** Jumping directly from cause `[A]` to solution `[D]` without spelling out intermediate state `[B]` and concrete failure `[C]`.
- Any dual-language header or bullet label in the format `<Local Label> (<English Label>):`.
- Over-translating standard developer terms (`git diff`, `commit`, `race condition`, `deadlock`, `in-place`, `offset`, `stream`, `buffer`, `fallback`, `edge case`, `breakpoint`, `unit test`, `mock`, `payload`, `thread`, `async/await`).
- Explaining non-obvious variables, magic numbers, array/regex indices, bitmasks, or boundary conditions in prose above the code AND again inside the code.
- Listing more than 3 function names in Section 2 (Main Execution Flow).
- Leaving irrelevant boilerplate, unchanged fields, or verbose parameters in `Before -> After` / `Input -> Output` examples instead of replacing unchanged parts with `...`.
- Padding a trivial 1-decision diff with artificial `Step 2` and `Step 3` boilerplate.
- Asking a Section 5 question about a function or line not shown in Section 3, or replying to the user's Turn-2 answer with another long lecture (>5 sentences).
