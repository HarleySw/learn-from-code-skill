---
name: learn-from-code
description: Use when the user asks to explain recent code changes, review a git diff or commit for learning, understand architectural decisions, avoid comprehension debt, or practice hands-on coding via TODO(human) exercises.
license: MIT
metadata:
  author: HarleySw
  version: "1.8.0"
---

# Learn From Code

Build codebase mental models and debugging intuition from code changes by limiting **topic scope** (at most 3 core steps, ignoring peripheral boilerplate) while explaining every mechanism with **complete, unbroken causal reasoning**—never telegraphic compression.

## Operating Modes

- **Mode A — Post-Implementation Review (Default):** Explain a completed `git diff`, commit, or existing module using the 5-Section Output Contract below, then pause for active retrieval.
- **Mode B — In-Task Active Learning (`TODO(human)`):** Survey existing patterns, generate surrounding boilerplate, and leave 5–15 lines at the key decision point marked `TODO(human)`. Output a 4-bullet handoff block (**Business Context** — the domain rule at stake, **Location** — exact file & line of `TODO(human)`, **Contract** — inputs and expected return/side effects, and **Edge Cases to Weigh** — 1–2 boundary conditions to handle) and pause for the user to implement it; once written, review their code in 3–5 concise sentences.

## Language & Developer Register Rules

1. **Match the User's Language Automatically:**
   - Automatically respond and localize all section headers, transition labels, bullet labels, and inline code comments into the exact language the user is using.
2. **Zero Dual-Language Header Clutter:**
   - Use only natural monolingual labels in the active response language. Never pair two languages side-by-side or append parenthetical English labels to localized headers, step titles, or risk bullet titles (e.g., never write `<Local Label> (<English Label>):` such as `Khối catch nuốt lỗi (Silent Failure):`).
3. **Natural Developer Code-Switching (Never Over-Translate Core Terms):**
   - Preserve universal technical terms that native developers use daily (`git diff`, `commit`, `race condition`, `deadlock`, `in-place`, `offset`, `stream`, `buffer`, `fallback`, `edge case`, `breakpoint`, `unit test`, `mock`, `payload`, `thread`, `async/await`) instead of forcing stiff literal translations.
   - For unfamiliar domain-specific or low-level protocol identifiers, keep the original identifier and attach a brief 2–4 word plain-language explanation on first mention.
4. **Mechanical Plain-Language Syntax (`Concrete Actor + Strong Action Verb`, `One Word = One Meaning`, `Old-Before-New`):**
   - **Concrete Actor + Strong Action Verb (Zero Zombie Nouns):** Make every sentence subject a concrete actor (`User`, `Function X`, `Variable Y`, `Loop`, `Database`) paired directly with a strong action verb (`overwrites`, `drops`, `blocks`, `prefixes`, `skips`). Never turn verbs into abstract nominalizations ("zombie nouns") or open sentences with wrapper phrases (*"The implementation of..."*, *"The execution of..."*, *"Việc thực hiện..."*, *"Sự cô lập của..."*, *"Cơ chế xử lý..."*):
     - ❌ *Bad (Abstract / Zombie Nouns):* `"The implementation of prefixing performs the isolation of identifiers to prevent the occurrence of data collision."`
     - ✅ *Good (Actor + Action Verb):* `"Prefixing pres.id gives each prescription a unique key so prescription B never overwrites prescription A."`
   - **One Word, One Meaning (Zero Synonym Rotation):** Once you name a domain entity or technical concept in the first sentence, use that exact term throughout the entire response—never rotate synonyms for stylistic variety.
   - **One Idea Per Sentence (Max 25 Words) & `Old-Before-New` Chaining:** Limit each prose sentence to 1 main action (20–25 words max); use arrows (`->`) or split into two short sentences instead of nesting 3–4 clauses. When chaining two sentences, open sentence 2 with the variable or result produced at the end of sentence 1 (`[Function A builds string X]. [Using string X, Function B checks...]`).

## Core Readability & Cognitive Load Rules

1. **No Premature Complexity (Start Directly With the Problem):**
   - Section 1 (`Big Picture`) must **always** open directly with `Symptom -> Root cause -> Fix -> Concrete example` (or `Goal -> Key design challenge -> Core approach -> Concrete example`).
   - Never place a `⚠️ Defect Alert` at the top of Section 1 before the reader understands the domain and code steps; place `⚠️ Defect Alert` at the **top of Section 4 (`Verification & Hidden Risks`)** after Section 3.
   - Only output a `Change Map` when a commit genuinely bundles **multiple unrelated features/concerns** (e.g., changing payment UI + changing report export in one commit). If client, server, and test files in a commit all solve **one cohesive problem**, never print a `Change Map`.
2. **No Wall-of-Prose — Contrastive Why + Visual Arrow (`->`) Dry-Run:**
   - Below each code snippet in Section 3, keep **only 2 items** (never add a separate 3rd `If omitted` bullet that repeats the failure):
     1. **Contrastive Why (Why not the naive approach / what breaks without it):** At most 2 concise sentences explaining why the simpler/naive approach `[Q]` (or omitting this check) fails (`[State A] -> [Intermediate state B] -> [Failure C]`) and why `[P]` prevents it.
     2. **Visual Dry-Run:** Format strictly as **2–3 sub-bullets using arrows (`->`)** with short, memorable sample values (never write a dense 100-word prose paragraph or use cryptic test IDs):
        - *Standard synchronous logic:* Trace 1–2 concrete inputs (`Case 1: input = "Order_A", id = "1" -> builds scopedId = "Order_A_1" -> Kept`).
        - *Async, concurrency, lock, or lifecycle cleanup logic (`async/await`, `race condition`, `useEffect` cleanup, `AbortController`, debounce):* Format as an **Interleaved Timeline (`t1 -> t2 -> t3`)** showing overlapping events (e.g., `t1: Request A starts -> t2: User switches tab, cleanup sets cancelled = true -> t3: Stale Request A resolves, hits cancelled guard, and exits without overwriting state`).
3. **Concrete Evaluated Examples Inside Inline Code Comments:**
   - Put all explanations of non-obvious variables, magic numbers, array/regex indices, bitmasks, or boundary conditions directly into short inline comments inside the code snippet in the user's language.
   - Whenever a code line contains a long string interpolation, complex regex, or multi-part expression, the inline comment right above that line **must include a concrete evaluated example string/value** so the reader grasps the result in 1 second without mentally parsing the expression.
   - Never repeat prose summaries of variables above the code.
4. **Bridge Domain Model & Program Model (Real-World Triggers, Grounded Business Rules & Persisted-Data / Runtime Risks):**
   - Code mechanics (`Program Model`) are only half of comprehension; always anchor them to the project's real-world workflow and business invariants (`Domain / Situation Model`) without hallucinating:
     - **Ground Domain Terms in Repo First (Zero Domain Hallucination):** Before explaining internal domain codes, status numbers, or abbreviated identifiers (e.g., `FormA1`, `statusCode = 3`, `tier = "P2"`), cross-check repo documentation (`CLAUDE.md`, `GEMINI.md`, `README.md`), comments, enums/models, or unit test descriptions. If a domain meaning has no evidence in the repo and is only inferred from a variable name, explicitly label it as an **inferred hypothesis** rather than stating it as fact.
     - **In Section 2 (Real-World User / Business Triggers, or Runtime / System Triggers for Pure Infrastructure):** Never list functions as dry data processors. Start each step with the **real-world user action or business event** that triggers that function and the user goal it serves—or, if the commit is pure infrastructure, build/config, or a low-level utility with no direct end-user action, open with the **runtime or caller system event** that invokes it instead of forcing an artificial user story.
     - **In Section 3 (Business Rules & Divergent Domain Policies):** In the opening transition of each Step, state the **Business Rule / Domain Invariant** being enforced. When two modules in the same commit handle the same domain entity differently (e.g., one module deduplicates items by name while another keeps every item per parent container), explicitly explain the **business reason** behind that difference.
     - **In Section 4 (Domain Invariant, Persisted-Data, Unhappy-Path & Scale Risks):** State the **concrete real-world business/user impact** if an invariant fails, and always check: (a) **persisted-data backward compatibility** whenever an ID/Key formula or schema stored in `localStorage`/DB/cache/session/URL changes, (b) **unhappy-path & silent failures** in `catch` blocks, `?.`, or default `fallback` branches, and (c) **production scale bottlenecks** (`O(N^2)` nested loops like `.find()` inside `for`, `N+1` queries, or memory growth) that look harmless on 2 dry-run items.
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

### 2. Main Execution Flow (3–4 steps anchored to Real-World User / Business or Runtime System Triggers)
- A numbered list of 3–4 plain-language steps where each step opens with the **real-world user action or business scenario** that triggers it (or, for pure infrastructure/plumbing/low-level utilities without direct user interaction, the **runtime or caller system event** that invokes it), tracing how data moves to the core change and naming **at most 2–3 primary functions** (or, for declarative code like SQL, Schema, Config, and CSS/UI without a sequential call stack, **3–4 Data Transformation Phases or Constraint Layers**).

### 3. Core Code Steps (1 to 3 steps, scaled to diff complexity — Micro / Code Level)
For each step (`#### Step 1: [Practical technical question?]`, plus `Step 2` / `Step 3` only when distinct engineering decisions exist):
- **1–2 plain-language transition sentences** connecting this step to the previous one, stating the **Business Rule / Domain Invariant** this step protects (and, if another module in the commit treats the same entity differently, why the business domain requires that difference).
- **Short code snippet (8–18 lines):** Put all explanations of non-obvious variables, magic numbers, array/regex indices, bitmasks, or boundary conditions directly in inline comments (in the user's language) above/beside the code lines. For long string interpolations, regexes, or complex expressions, include a **concrete evaluated example** inside the comment (e.g., `// Match full key (e.g. k === "OrderA_1_Morning_0730")`).
- **Focused explanation below the code (2 items only, localized to the user's language):**
  - **Contrastive Why (Why not naive `[Q]` / what breaks if omitted):** At most 2 concise sentences explaining why the simpler/naive approach `[Q]` (or omitting this step) fails and what concrete bug occurs in practice.
  - **Visual Dry-Run:** 2–3 sub-bullets using arrows (`->`) and short, memorable sample values tracing how the input evaluates across the variables and branches to the final result (or an **Interleaved Timeline `t1 -> t2 -> t3`** for async/concurrency/cleanup code).

### 4. Verification & Hidden Risks
- **`⚠️ Defect Alert` *(Only if a bug/flaw exists in the diff or its unseen callers)*:** Before writing Section 4, audit immediate callers/consumers **outside the `git diff`**, error-handling branches (`catch`, `?.`, default `fallback`—checking whether they degrade gracefully or silently swallow errors and corrupt domain state without logging), and production-scale complexity (`O(N^2)` nested `.find()`/`.includes()` loops or `N+1` queries that look harmless in a 2-item dry-run). If any logic bug, silent failure, `race condition`, leak, scale trap, or contract break exists, highlight it right here at the top of Section 4 (pinpointing the exact line, failure scenario with `->` steps, real-world domain impact, and fix)—and never praise broken/silent-failure code as "defensive" in Section 3.
- **Verified:** 1 bullet on what is proven to work + a 1-line **Quick verification** specifying an exact `breakpoint` (`file:line`) accompanied by the concrete **State Invariant / expression** to inspect to verify state is not infected/corrupted (e.g., `breakpoint at file.ts:131: verify Object.keys(doses) all carry ${prescriptionId}_ prefix`), plus any relevant test command or visual check.
- **Assumptions & Risks:** 1–2 bullets on remaining unverified `edge cases`, unseen caller assumptions, **persisted-data backward compatibility** (whenever an ID/Key format, state shape, or schema in `localStorage`/DB/cache/session/URL changes, whether existing saved user data will fail or need migration), or **scale/unhappy-path risks**, explicitly stating both the **technical trigger** and the **real-world business/user consequence** if violated. If twin logic, shared key formats, or producer-consumer contracts span multiple files (e.g., Client API vs. Server Service), explicitly mandate the cross-file sync rule: *"If modifying logic [X] in [File A] later, [File B] must be updated synchronously to prevent state drift."*

### 5. Debugging Check (1–2 short questions)
- Ask **1–2 single-focus debugging or boundary questions** (answerable in 2–3 sentences).
- **Only ask about code shown in Section 3:** Every question must target a specific condition or line visible in the Section 3 snippets (never ask about unseen helper functions or omitted lines) and quote that condition inline so the user never has to scroll up or open another file.
- **Wait for the user's reply** before giving the answers.
- *(Optional)* Add a 5-bullet repo gotcha note **only** if omitting it would cause a bug next month (especially cross-file synchronization rules: *"If modifying logic [X] in file [A] later, you must synchronously update file [B] to prevent state divergence"*) and it is not already recorded in project memory; ask before writing to repo files.

## Edge Case & Multi-Turn Rules

1. **Smart Target Resolution (When No Commit or File Is Specified):**
   - If the user invokes the skill without specifying a commit or file: (a) check uncommitted changes (`git diff` and `git diff --staged`) first; (b) if the working tree is clean, inspect `HEAD`; (c) if `HEAD` only touches docs, tooling/skill configs, formatting, or demo scripts, scan `git log` to select the **most recent core application/domain code commit** and add a 1-line note at the top identifying the chosen commit.
2. **Untangle Multi-Concern Commits (Only When Truly Tangled):**
   - Only output a brief **Change Map** at the top of Section 1 when a commit bundles **multiple unrelated features or concerns**. Never print a `Change Map` when client, server, and test files all implement a single cohesive fix or feature.
   - When a commit is genuinely tangled, deep-dive into **only the single most critical Core Logic slice** (200–400 LOC budget) across Sections 1–5, and ask at the end whether the user wants to move to the next slice in the following turn.
3. **Auto Scale-Down (Trivial / Single-Decision Diffs):**
   - If a diff contains only 1 meaningful engineering decision (e.g., tightening a guard clause, adding a config flag, fixing an off-by-one boundary), write **only `Step 1`** in Section 3 and keep the explanation tight and focused on that single decision. Never fabricate `Step 2` or `Step 3` from obvious boilerplate.
4. **Deletion-Heavy / Simplification Commits (*Chesterton's Fence*):**
   - When a commit primarily deletes legacy code, removes a `fallback` branch, or simplifies a module, quote the key removed block (or a compact `Before -> After` pair) in Section 3 and explain: (a) what original `edge case` or business rule the removed code protected, (b) what new mechanism replaces it so deletion is safe, and (c) whether existing persisted user data (`localStorage` / database) could fail once that branch is gone.
5. **Defect & Unseen-Caller Audit in Section 4 (Anti-Post-Hoc Rationalization):**
   - Audit both the diff and its immediate callers/consumers outside the diff before explaining. Never rationalize broken code as clever design—after walking through Section 3, flag any real logic bug, `race condition`, leak, or broken downstream caller with **`⚠️ Defect Alert`** at the top of Section 4.
6. **Turn-2 Reply Protocol (Formative Feedback & "What-If" Deep-Dives):**
   - When the user replies in Turn 2, respond in **at most 3–5 short sentences**:
     1. **If the user answers the Section 5 questions:** Confirm what they reasoned correctly; if there is a misconception or missed boundary condition, point directly to the quoted line or condition from Section 3 to correct it concisely; once mastered, briefly ask if they want to save the 5-bullet repo gotcha note (if applicable) or continue to the next commit slice.
     2. **If the user instead asks a hypothetical *"What-if"* or code-modification question (*"What if we change line X to Y?"*):** Answer their question directly first using a concise **Contrastive Why + Visual Arrow (`->`) Trace**, and then lightly ask if they still want to try the Section 5 question—never launch a second full-length lecture.

## Red Flags — STOP and Fix Before Sending

- **Ungrounded or missing Domain Model context:** Guessing the business meaning of undocumented internal codes as fact instead of labeling them an *inferred hypothesis*, forcing a fake end-user UI story onto a pure infrastructure diff instead of using a Runtime/System trigger, or omitting real-world triggers, divergent domain reasons, and user impact across Sections 2–4.
- **Incomplete Section 4 audit or bare breakpoint:** Skipping the check for unseen callers, persisted-data compatibility, silent-failure `catch`/`fallback` blocks, `O(N^2)`/`N+1` scale traps, cross-file co-change rules, or suggesting a bare `breakpoint` line without the exact state invariant to inspect.
- **Premature complexity at the top of Section 1:** Putting a `⚠️ Defect Alert` before Section 3, or printing a `Change Map` for a single-purpose commit where client/server/test files all solve the same problem.
- **Wall-of-prose Dry-Run or 3rd `If omitted` bullet in Section 3:** Writing a dense paragraph for the dry-run instead of 2–3 arrow (`->`) bullets with short, memorable sample names (or `t1 -> t2 -> t3` for async/concurrency), or keeping a separate `If omitted` bullet that repeats the `Contrastive Why`.
- **Missing concrete example in complex code comments:** Leaving a long string interpolation or regex in Section 3 without an evaluated example string in its inline comment.
- **Telegraphic compression / skipped causal links:** Jumping directly from cause `[A]` to solution `[D]` without spelling out intermediate state `[B]` and concrete failure `[C]`.
- Any dual-language header or bullet label in the format `<Local Label> (<English Label>):`.
- **Zombie nouns, synonym rotation, or >25-word multi-clause prose sentences:** Opening sentences with abstract wrapper nouns (*"The implementation of..."*, *"Việc thực hiện..."*) instead of `Concrete Actor + Action Verb`, rotating synonyms for the same domain entity, or writing 40-word nested sentences instead of splitting or using `->`.
- Over-translating standard developer terms (`git diff`, `commit`, `race condition`, `deadlock`, `in-place`, `offset`, `stream`, `buffer`, `fallback`, `edge case`, `breakpoint`, `unit test`, `mock`, `payload`, `thread`, `async/await`).
- Explaining non-obvious variables, magic numbers, array/regex indices, bitmasks, or boundary conditions in prose above the code AND again inside the code.
- Listing more than 3 function names in Section 2 (Main Execution Flow).
- Leaving irrelevant boilerplate, unchanged fields, or verbose parameters in `Before -> After` / `Input -> Output` examples instead of replacing unchanged parts with `...`.
- Padding a trivial 1-decision diff with artificial `Step 2` and `Step 3` boilerplate.
- Asking a Section 5 question about a function or line not shown in Section 3, or replying to the user's Turn-2 answer with another long lecture (>5 sentences).
