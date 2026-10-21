---
name: learn-from-code
description: Use when the user asks to explain recent code changes, review a git diff or commit for learning, understand architectural decisions, avoid comprehension debt, or practice hands-on coding via TODO(human) exercises.
license: MIT
metadata:
  author: HarleySw
  version: "2.1.0"
---

# Learn From Code

Build codebase mental models and debugging intuition from code changes by prioritizing **completeness of core mechanics over arbitrary length caps**: filter out peripheral boilerplate, but explain every primary mechanism, guard condition, and dry-run step with **complete, unbroken causal reasoning**—never telegraphic compression or artificial truncation.

## Operating Modes

- **Mode A — Post-Implementation Review (Default):** Explain a completed `git diff`, commit, or existing module using the Two-Turn Output Contract below, pausing after Section 5 for active retrieval before delivering Sections 6–8 in Turn 2.
- **Mode B — In-Task Active Learning (`TODO(human)`):** Survey existing patterns, generate surrounding boilerplate, and leave the core decision block marked `TODO(human)`. Output a 4-bullet handoff block (**Business Context** — the domain rule at stake, **Location** — exact file & line of `TODO(human)`, **Contract** — inputs and expected return/side effects, and **Edge Cases to Weigh** — boundary conditions to handle) and pause for the user to implement it; once written, review their implementation thoroughly and clearly.

## Language & Developer Register Rules

1. **Match the User's Language Automatically:**
   - Automatically respond and localize all section headers, transition labels, bullet labels, and inline code comments into the exact language the user is using.
2. **Zero Dual-Language Header Clutter:**
   - Use only natural monolingual labels in the active response language. Never pair two languages side-by-side or append parenthetical English labels to localized headers, step titles, or risk bullet titles (e.g., never write `<Local Label> (<English Label>):`).
3. **Natural Developer Code-Switching (Never Over-Translate Core Terms):**
   - Preserve universal technical terms that native developers use daily (`git diff`, `commit`, `race condition`, `deadlock`, `in-place`, `offset`, `stream`, `buffer`, `fallback`, `edge case`, `breakpoint`, `unit test`, `mock`, `payload`, `thread`, `async/await`) instead of forcing stiff literal translations.
   - For unfamiliar domain-specific or low-level protocol identifiers, keep the original identifier and attach a brief plain-language explanation on first mention.
4. **Mechanical Plain-Language Syntax (`Concrete Actor + Strong Action Verb`, `One Word = One Meaning`, `Zero Translationese`):**
   - **Concrete Actor + Strong Action Verb (Zero Zombie Nouns):** Make every sentence subject a concrete actor (`User`, `Checkout screen`, `Search filter`, `Function X`, `Loop`) paired directly with a strong action verb (`overwrites`, `skips`, `blocks`, `prefixes`) following `Who/What + strong action verb + which specific item + when`. Never turn verbs into abstract nominalizations ("zombie nouns") or open sentences with wrapper phrases (*"The implementation of..."*, *"The execution of..."*):
     - ❌ *Bad (Abstract / Zombie Nouns):* `"The implementation of prefixing performs the isolation of identifiers to prevent the occurrence of data collision."`
     - ✅ *Good (Actor + Action Verb):* `"Prefixing groupId gives each group a unique key so Group B never overwrites Group A."`
   - **Ban "Translationese" and Literal Metaphor Calques (Use Everyday Spoken Developer Words):** When responding in any non-English language, write exactly as a native developer speaks aloud to a coworker at their desk. Never translate English technical metaphors (`drop`, `isolate`, `deduplicate`, `preserve`, `hydrate`, `swallow`, `plumbing`) word-by-word into unnatural literal verbs or stiff bureaucratic/academic nouns in the target language; always choose the natural everyday action phrase that native speakers use for that behavior (e.g., instead of literally translating *"drops items"*, *"isolates IDs"*, or *"preserves state"*, express it naturally as *"skips/misses items"*, *"gives each group its own separate key"*, or *"keeps the existing status"* in the target language).
   - **One Word, One Meaning (Zero Synonym Rotation):** Once you name a domain entity or technical concept in the first sentence, use that exact term throughout the entire response—never rotate synonyms for stylistic variety.
   - **One Clear Idea Per Sentence (Avoid Nested Multi-Clause Run-On Sentences), `Old-Before-New` Chaining & Zero `->` Run-On Prose:** Express one clear main action per sentence so every sentence reads naturally in a single breath without nested sub-clauses. In prose explanations (`Root cause`, `Key design challenge`, `Contrastive Why`, and `Assumptions & Risks`), NEVER chain multiple clauses together on a single line with `->` arrows as a substitute for periods. Instead, write **separate, complete sentences ending with a period (`.`)** and link them smoothly with `Old-Before-New` transitions—opening sentence 2 with the concept, variable, or result produced at the end of sentence 1 (*"First, the exporter assigns each word a fixed X coordinate. Because bold glyphs are wider than that estimate, the first word overflows into the space at X=55.2. As a result, the space disappears and the two words touch."*). Reserve `->` strictly for short state transitions at the end of a vertical dry-run line (e.g., `-> passes the first if guard`, `-> returns true`).

## Core Readability & Cognitive Load Rules

1. **No Premature Complexity or Code-Symbol Soup in Section 1 (Start With 100% Plain-Language Concepts):**
   - Section 1 (`Big Picture`) must **always** open directly with the 4 bullets `Symptom`, `Root cause`, `Fix`, and `Concrete example` (or `Goal`, `Key design challenge`, `Core approach`, and `Concrete example`) so the reader immediately grasps the problem and core idea.
   - **Zero File, Function, or Variable Names in Section 1 Prose:** Never mention file paths, function names, or local variable/property identifiers (e.g., raw variable names, helper function names, or internal file paths) in the 3 prose bullets (`Symptom`, `Root cause`, `Fix`). Explain them entirely in plain domain concepts (e.g., *"group identifier"*, *"item sequence number"*, *"cache key"*) and save all file, function, and variable names for Sections 2 and 3.
   - Never place a `⚠️ Defect Alert` at the top of Section 1 before the reader understands the domain and code steps; place `⚠️ Defect Alert` at the **top of Section 4 (`Verification & Hidden Risks`)** after Section 3.
   - Only output a `Change Map` when a commit genuinely bundles **multiple unrelated features/concerns** (e.g., changing payment UI + changing report export in one commit). If client, server, and test files in a commit all solve **one cohesive problem**, never print a `Change Map`.
2. **No Wall-of-Prose — Complete Contrastive Why + Vertical Multi-Line Walkthrough (Mayer & Clark's Personalization Principle):**
   - Below each code snippet in Section 3, keep **only 2 items** (never add a separate 3rd `If omitted` bullet that repeats the failure):
     1. **Contrastive Why (Why not the naive approach / what breaks without it):** Explain **the full reason behind every important guard, condition, or branch present in the quoted snippet** (if the snippet contains multiple distinct guards, explain why each one is needed and what concrete bug occurs if omitted), written as clear, separate single-idea sentences ending with periods (`.`)—never chained into a long run-on line with `->`.
     2. **Visual Dry-Run (Vertical Multi-Line Walkthrough With No Length Restriction):** Do not restrict the length of `Visual Dry-Run`, and NEVER cram Input, multiple checks or loop iterations (`i = 0, 1, 2`), and Output onto a single horizontal line or format it as a robotic variable-assignment dump (`varA = 1, varB = 2 -> cond1 && cond2 -> true`). For each scenario, break the walkthrough **vertically onto separate indented sub-bullets** that match the numbered comments (`// 1.`, `// 2.`, `// 3.`) or loop iterations in the code snippet, using natural human phrases (never cluttering sentences with parenthetical boolean dumps like `(sameLineY = true)` or `(noFontChange = true)`):
        - **Input:** State the concrete sample items entering the snippet in plain words with simple numbers.
        - **Step-by-step / Loop trace (1 vertical line per check or loop iteration):** Show what happens at each numbered step or each loop pass (`Iteration 1 (i = 0): ...`, `Iteration 2 (i = 1): ...`), explicitly displaying how a mutable intermediate variable or buffer looks after that exact step.
        - **Result:** Show the final return value or output string.
        - ❌ *Bad (Robotic variable log dump or horizontal 1-line chain):*
          `- Case 1: prevBlock = "(Hello)" (Y=14.5, prevX=57.3), between = "\r\n", curBlock = "(world)" (Y=14.5, curX=86.1) -> IsNullOrWhiteSpace(between) == true && sameLineY && 86.1 > 57.3 -> returns true.`
        - ✅ *Good (Vertical step-by-step walkthrough matching numbered code comments):*
          - **Scenario 1 — Two words inside the same table cell (Merged):**
            - **Input:** Previous word is `"(Hello)"` (`Y = 14.5, X = 57`), next word is `"(world)"` (`Y = 14.5, X = 86`).
            - **Step 1–3 (Check gap `between`):** Slicing the text between pos 120 and pos 122 returns only a blank newline `"\r\n"` (no table-border commands) -> passes the first `if` guard.
            - **Step 4–6 (Check line, font, and X order):** Both sit on line `Y = 14.5`, the second word has no font-change command (`Groups[1]` is empty), and `86 > 57` -> all checks pass.
            - **Result:** Returns `true` so the two words merge into one phrase.
        - *Async, concurrency, lock, or lifecycle cleanup logic (`async/await`, `race condition`, `useEffect` cleanup, `AbortController`, debounce):* Format as a **Vertical Interleaved Timeline** with one indented sub-bullet per timestamp (`- t1: ...`, `- t2: ...`, `- t3: ...`) showing overlapping events in plain words rather than cramming `t1 -> t2 -> t3` onto one horizontal line.
3. **Subgoal-Labeled Inline Comments with Simple Numbers for All Index, Offset & Slice Math (Margulieux, Guzdial & Catrambone's Subgoal Labeling):**
   - Put all explanations of non-obvious variables, magic numbers, array/regex indices, bitmasks, or boundary conditions directly into short inline comments inside the code snippet in the user's language. Never repeat prose summaries of variables above the code.
   - **Comment All Index, Offset, Pointer, and Slice Arithmetic with Simple Numbers:** Never leave index arithmetic, string slicing (`Index + Length`, `Substring`, `slice`), coordinate math, bit shifts, or pointer offsets uncommented by assuming they are "obvious syntax"—pointer and index arithmetic is where readers expend the most working memory. Above every such line, write a short **Subgoal Comment** stating *what boundary or gap is being calculated* illustrated with simple concrete numbers, and never put raw, undecoded protocol gibberish inside explanatory comments:
     - ❌ *Bad (Uncommented index math + raw gibberish in comment):*
       ```csharp
       int previousEnd = previousBlock.Index + previousBlock.Length;
       // Get substring between blocks (e.g. "0.5 w 10 20 50 15 re S\r\n")
       string between = text.Substring(previousEnd, currentBlock.Index - previousEnd);
       ```
     - ✅ *Good (Subgoal label + simple numbers + plain-language meaning):*
       ```csharp
       // 1. Find the character position right after the previous word ends (e.g., starts at pos 100 + length 20 = ends at pos 120)
       int previousEnd = previousBlock.Index + previousBlock.Length;
       // 2. Slice out whatever sits between the previous word (pos 120) and the next word (e.g., pos 125)
       string between = text.Substring(previousEnd, currentBlock.Index - previousEnd);
       // 3. If that gap contains table-border drawing commands instead of blank whitespace, the two words belong to different cells -> do not merge!
       if (!string.IsNullOrWhiteSpace(between)) return false;
       ```
   - Whenever a code line contains a long string interpolation, complex regex, or multi-part expression, the inline comment right above that line **must include a concrete evaluated example string/value** so the reader immediately sees the result without mentally parsing the expression.
4. **Bridge Domain Model & Program Model (Real-World Triggers, Grounded Business Rules & Persisted-Data / Runtime Risks):**
   - Code mechanics (`Program Model`) are only half of comprehension; always anchor them to the project's real-world workflow and business invariants (`Domain / Situation Model`) without hallucinating:
     - **Ground Domain Terms in Repo First (Zero Domain Hallucination):** Before explaining internal domain codes, status numbers, or abbreviated identifiers (e.g., `FormA1`, `statusCode = 3`, `tier = "P2"`), cross-check repo documentation (`CLAUDE.md`, `GEMINI.md`, `README.md`), comments, enums/models, or unit test descriptions. If a domain meaning has no evidence in the repo and is only inferred from a variable name, explicitly label it as an **inferred hypothesis** rather than stating it as fact.
     - **In Section 2 (Real-World User / Business Triggers, or Runtime / System Triggers for Pure Infrastructure):** Never list functions as dry data processors. Start each step with the **real-world user action or business event** that triggers that function and the user goal it serves—or, if the commit is pure infrastructure, build/config, or a low-level utility with no direct end-user action, open with the **runtime or caller system event** that invokes it instead of forcing an artificial user story.
     - **In Section 3 (Business Rules & Divergent Domain Policies):** In the opening transition of each Step, state the **Business Rule / Domain Invariant** being enforced. When two modules in the same commit handle the same domain entity differently (e.g., one module deduplicates items by name while another keeps every item per parent container), explicitly explain the **business reason** behind that difference.
     - **In Section 4 (Domain Invariant, Persisted-Data, Unhappy-Path & Scale Risks):** State the **concrete real-world business/user impact** if an invariant fails, and always check: (a) **persisted-data backward compatibility** whenever an ID/Key formula or schema stored in `localStorage`/DB/cache/session/URL changes, (b) **unhappy-path & silent failures** in `catch` blocks, `?.`, or default `fallback` branches, and (c) **production scale bottlenecks** (`O(N^2)` nested loops like `.find()` inside `for`, `N+1` queries, or memory growth) that look harmless on 2 dry-run items.
5. **Primary Pipeline Trace in Section 2 (Filter Out Trivial Utilities, Keep Every Primary Pipeline Step):**
   - In the execution flow section, trace the actual path data takes through the **Primary Pipeline Functions** from entry trigger to final output, skipping only minor formatting/utility helpers so the reader grasps the complete end-to-end call chain without drowning in low-level utility names.
6. **Noise-Free `Before -> After` Examples & 1-Line Syntax Anatomy Legend Before Unfamiliar Formats (Mayer's Pre-Training Principle & Hermans' LTM Syntax Support):**
   - Replace repetitive property mappings or unchanged fields with `...` so the reader immediately spots the exact transformation.
   - **1-Line Syntax Anatomy Legend Before Unfamiliar Formats:** Whenever a `Before -> After` snippet, code block, or code comment includes unfamiliar format tokens, protocol commands, graphics/bytecode operators, regex patterns, or DSL syntax, **ALWAYS place a 1-line Anatomy Legend comment at the very top of that snippet** decoding every token in plain words so the reader never has to guess unfamiliar syntax. Never put raw, undecoded protocol commands inside a comment:
     - ❌ *Bad (Unexplained tokens):*
       `CMD ... 28.8 14.5 POS (ItemA) DRAW END`
     - ✅ *Good (With 1-line Anatomy Legend at the top):*
       `// Command anatomy: CMD..END (text block) | 28.8 (horizontal X) | 14.5 (vertical Y) | (ItemA) DRAW (render text)`
       `CMD ... 28.8 14.5 POS (ItemA) DRAW END`
7. **Chronological Story Pipeline (`Step 1 -> Step 2 -> ...`):**
   - Order the steps chronologically along the actual data or execution flow and title each step with the practical technical question that step resolves.
8. **Link Once Per Section:**
   - Link a file or symbol only on its **first appearance** in a section. Do not spam clickable links on every line.

## Two-Turn Output Contract (Mode A Recipe)

Localize the section titles and bullet labels below into the user's language (single-language labels only):

### 1. Big Picture (Context-Adaptive — 100% Plain-Language Macro Story)
Explain the causal chain at the **Macro / System behavior level** using **100% plain-language domain concepts**—never mention file names, function names, or local variable/property names in the 3 prose bullets below (leave all code symbols for Sections 2 and 3). Always start directly with the 4 bullets below (only prepend a `Change Map` if the commit mixes unrelated features):
- **For Bug Fixes / Performance Fixes:**
  - **Symptom:** Describe the **concrete scene the user actually sees on screen** using specific sample names/times—never use compressed abstract noun phrases (like *"loses overlapping slots"* or *"drops subsequent items"*). If a bug causes 2 symptoms on 2 different screens/features, split them into **2 separate sub-bullets** (never cram both into one sentence with *"simultaneously / and"*):
    - *Example:* Instead of *"The search drops subsequent items and the cart loses overlapping entries"*, split into two concrete screen behaviors: (1) *In Search results:* the filter reads only Category A and skips all items in Category B; (2) *On the Checkout screen:* if two orders share item number `#1`, the screen displays only 1 item and the other disappears.
  - **Root cause:** Open with a 1-sentence everyday analogy, then explain the complete macro causal chain in **separate, single-idea sentences ending with a period (`.`)** linked smoothly with `Old-Before-New` transitions (**never chain multiple clauses together on one line with `->` arrows**).
  - **Fix:** State the high-level conceptual solution in everyday spoken words without listing file, function, or variable names.
  - **Concrete example (`Before -> After`):** A visual snippet that **clearly shows the collision, overwrite, or transformation happening** in `Before` and how data is kept intact in `After` across concrete items with vivid sample names (never write a single abstract variable assignment). **If the example contains any domain-specific format tokens, protocol commands, regex syntax, or DSL operators, place a 1-line Syntax Anatomy Legend comment at the very top of the snippet decoding each token in plain words:**
    ```ts
    // Before: Both orders use item number "1", so Order B overwrites Order A
    items["1"] = "Laptop (Order A)";
    items["1"] = "Mouse (Order B)"; // <- Wipes out Laptop!

    // After: Prefixing the order ID keeps both items in separate slots
    items["OrderA_1"] = "Laptop (Order A)";
    items["OrderB_1"] = "Mouse (Order B)"; // <- Both kept!
    ```
- **For New Features / Refactors / Existing Module Walkthroughs:**
  - **Goal:** State the capability or architectural outcome being built in plain words.
  - **Key design challenge:** Use a **Contrastive Explanation** written as **separate, single-idea sentences ending with a period (`.`)** and `Old-Before-New` transitions (**never chain long prose clauses with `->` arrows**) to explain why the obvious/naive approach falls short (with a 1-sentence everyday analogy if helpful).
  - **Core approach:** Summarize how the chosen design solves that challenge in plain words (zero file, function, or variable names).
  - **Concrete example (`Input -> Output`):** A noise-free visual snippet with vivid sample values illustrating what enters and exits the system (starting with a **1-line Syntax Anatomy Legend comment** at the top if any unfamiliar format/protocol/DSL tokens appear).

### 2. Main Execution Flow (Anchored to Real-World User / Business or Runtime System Triggers)
- A numbered list of plain-language steps tracing the actual path data takes from entry to output through the **Primary Pipeline Functions** (or through the **Data Transformation Phases / Constraint Layers** for declarative SQL, Schema, Config, and CSS/UI), where each step opens with the **real-world user action or business scenario** that triggers it (or, for pure infrastructure/plumbing/low-level utilities without direct user interaction, the **runtime or caller system event** that invokes it), skipping only minor utility helpers.

### 3. Core Code Steps (Scaled to the Actual Number of Engineering Decisions — Micro / Code Level)
Divide into steps corresponding to the **actual number of distinct engineering decisions or guards in the execution flow** (`#### Step 1: <Practical technical question?>`, `#### Step 2: ...`—filter out peripheral boilerplate, but never omit an important logic step just to fit an arbitrary step limit):
- **Plain-language transition opening:** Connect this step to the previous one and state the **Business Rule / Domain Invariant** this step protects (and, if another module in the commit treats the same entity differently, why the business domain requires that difference).
- **Complete step code snippet (with full Subgoal Comments & Syntax Anatomy Legend):**
  - Quote the **complete code block for that step** accompanied by all required Subgoal Comments and the 1-line Syntax Anatomy Legend, replacing with `...` only repetitive property assignments that contain zero control-flow logic.
  - If the snippet manipulates unfamiliar format tokens, protocol operators, or regex capture groups, start the snippet with a **1-line Syntax Anatomy Legend comment** decoding each token/group in plain words.
  - Above every index, offset, pointer, or string-slice calculation (`Index + Length`, `Substring`, `slice`), write a **Subgoal Comment with small concrete numbers** (e.g., `// 1. Find where previous word ends (starts at pos 100 + length 20 = ends at pos 120)`), and never dump raw, undecoded protocol gibberish into comments.
  - For long string interpolations, regexes, or complex expressions, include a **concrete evaluated example** inside the comment (e.g., `// Match full key (e.g. k === "OrderA_1_Morning_0730")`).
  - **Worked-Example Completeness (Renkl's Principle):** Every condition present in a Section 3 code snippet (such as secondary boolean guards or loop boundary checks like `i + 1 < count`) MUST have its purpose explained clearly in Section 3 (in the inline comment or Contrastive Why), and any downstream code line that depends on that condition must be explicitly visible in the snippet—never hidden behind `...`.
- **Focused explanation below the code (2 items only, localized to the user's language):**
  - **Contrastive Why (Why not the naive approach / what breaks if omitted):** Explain **the full reason behind every important guard, condition, or branch in the snippet**, written as separate, single-idea sentences ending with a period (`.`) and linked by `Old-Before-New` transitions (**never chain multiple clauses together with `->` arrows**).
  - **Visual Dry-Run (Vertical Multi-Line Walkthrough With No Length Restriction):** Never cram Input, multiple checks or loop iterations (`i = 0, 1, 2`), and Output onto a single horizontal line. For each concrete scenario, break the trace **vertically onto separate indented sub-bullets** matching the numbered comments (`// 1.`, `// 2.`, `// 3.`) or loop passes in the snippet, using natural human phrases without parenthetical boolean dumps (`(sameLineY = true)`):
    - **Input:** State the concrete sample items entering the snippet.
    - **Step-by-step / Loop trace (1 vertical sub-bullet per check or loop iteration):** Show what happens at each numbered step or each loop pass (`Iteration 1 (i = 0): ...`, `Iteration 2 (i = 1): ...`), explicitly showing how any mutable variable or buffer looks after that step (using `->` only at the end of a step line for a short outcome like `-> passes the first if guard`).
    - **Result:** Show the final return value or output string.

### 4. Verification & Hidden Risks
- **`⚠️ Defect Alert` *(Only if a bug/flaw exists in the diff or its unseen callers)*:** Before writing Section 4, audit immediate callers/consumers **outside the `git diff`**, error-handling branches (`catch`, `?.`, default `fallback`—checking whether they degrade gracefully or silently swallow errors and corrupt domain state without logging), and production-scale complexity (`O(N^2)` nested `.find()`/`.includes()` loops or `N+1` queries that look harmless in a 2-item dry-run). If any logic bug, silent failure, `race condition`, leak, scale trap, or contract break exists, highlight it right here at the top of Section 4 (pinpointing the exact line, failure scenario, real-world domain impact, and fix in clear, separate sentences ending with periods `.`)—and never praise broken/silent-failure code as "defensive" in Section 3.
- **Verified:** State what is proven to work + a **Quick verification** specifying an exact `breakpoint` (`file:line`) accompanied by the concrete **State Invariant / expression** to inspect to verify state is not infected/corrupted (e.g., `breakpoint at file.ts:131: verify Object.keys(items) all carry ${groupId}_ prefix`), plus any relevant test command or visual check.
- **Assumptions & Risks:** List **all grounded, evidence-backed risks found in the code and its callers** (never invent generic boilerplate risks, and never omit a real risk to fit an arbitrary bullet cap)—covering remaining unverified `edge cases`, unseen caller assumptions, **persisted-data backward compatibility** (whenever an ID/Key format, state shape, or schema in `localStorage`/DB/cache/session/URL changes, whether existing saved user data will fail or need migration), and **scale/unhappy-path risks**. Explain both the **technical trigger** and the **real-world business/user consequence** in **separate, complete sentences ending with a period (`.`)** linked with `Old-Before-New` transitions (**never chain multiple clauses together with `->` arrows**).

### 5. Debugging Check (Dynamic Question Selection)
- **Dynamic Question Selection (Scale to the Change's Complexity):** Do not cap the check at a fixed number of questions. Decide how many questions to ask based on the actual number of distinct mechanisms, boundary conditions, and domain risks covered in Sections 3 and 4 (for example, 1 question for a simple single-step change, or 3–4 questions when Section 3 covers multiple distinct steps and edge cases). Each question must target a different decision point, boundary guard, or failure scenario—never ask two questions that test the same concept, and never pad with trivial syntax questions.
- **Complete Prerequisite Rule (Never Quiz on Unexplained Mechanics or Omitted Code Lines — Renkl's Worked-Example Completeness Principle):** Every condition or mechanic you ask about in Section 5 MUST have had its underlying reason explained in Section 3 first, and all code lines needed to deduce the answer must be explicitly visible in the Section 3 snippets (never hidden behind `...`). Section 5 questions must test applying a rule that was *already clearly taught* in Section 3 to a new edge-case input, quoting the exact condition inline so the user never has to scroll up or guess a hidden mechanic:
  - ❌ *Bad (Quizzing on an unexplained guard or code hidden behind `...`):* Asking in Section 5 what happens in Step 2 when two items have different font codes if Step 1 only mentioned `group[1] == ""` in passing without explaining *why* font changes prevent merging, or if Step 2 hid the line `sharedFont = firstItem.Groups[1]` behind `...`.
  - ✅ *Good (Testing application of an already-explained rule):* Explaining in Section 3 *why* Step 1 checks `noFontChange` and explicitly showing Step 2's `sharedFont = firstItem.Groups[1]` line, then asking in Section 5 what visual bug would appear on screen if `noFontChange` were removed when a bold word sits next to a regular word.
- **Wait for the user's reply** before giving the answers or moving to the Turn-2 Consolidation & Transfer Contract (Sections 6, 7, and 8).

## Edge Case & Multi-Turn Rules

1. **Smart Target Resolution (When No Commit or File Is Specified):**
   - If the user invokes the skill without specifying a commit or file: (a) check uncommitted changes (`git diff` and `git diff --staged`) first; (b) if the working tree is clean, inspect `HEAD`; (c) if `HEAD` only touches docs, tooling/skill configs, formatting, or demo scripts, scan `git log` to select the **most recent core application/domain code commit** and add a 1-line note at the top identifying the chosen commit.
2. **Untangle Multi-Concern Commits (Only When Truly Tangled):**
   - Only output a brief **Change Map** at the top of Section 1 when a commit bundles **multiple unrelated features or concerns**. Never print a `Change Map` when client, server, and test files all implement a single cohesive fix or feature.
   - When a commit is genuinely tangled, deep-dive into **one cohesive Core Logic concern at a time** across Sections 1–5, and ask at the end whether the user wants to move to the next slice in the following turn.
3. **Auto Scale-Down (Trivial / Single-Decision Diffs):**
   - If a diff contains only 1 meaningful engineering decision (e.g., tightening a guard clause, adding a config flag, fixing an off-by-one boundary), write **only `Step 1`** in Section 3 and keep the explanation tight and focused on that single decision. Never fabricate `Step 2` or `Step 3` from obvious boilerplate.
4. **Deletion-Heavy / Simplification Commits (*Chesterton's Fence*):**
   - When a commit primarily deletes legacy code, removes a `fallback` branch, or simplifies a module, quote the key removed block (or a compact `Before -> After` pair) in Section 3 and explain: (a) what original `edge case` or business rule the removed code protected, (b) what new mechanism replaces it so deletion is safe, and (c) whether existing persisted user data (`localStorage` / database) could fail once that branch is gone.
5. **Defect & Unseen-Caller Audit in Section 4 (Anti-Post-Hoc Rationalization):**
   - Audit both the diff and its immediate callers/consumers outside the diff before explaining. Never rationalize broken code as clever design—after walking through Section 3, flag any real logic bug, `race condition`, leak, or broken downstream caller with **`⚠️ Defect Alert`** at the top of Section 4.
6. **Turn-2 Consolidation & Transfer Contract (Sections 6, 7, 8):**
   - **If the user asks a clarifying or *"What-if"* question before answering Section 5:** Answer their question thoroughly using a clear **Contrastive Why + Vertical Step-by-Step Walkthrough**, then lightly invite them to answer the Section 5 check when ready.
   - **Once the user answers the Section 5 questions, structure the Turn-2 response into Sections 6, 7, and 8 (localized into the user's language, single-language headers only):**
     - **### 6. Answer Review & Diagnostic Feedback (Shute's Formative Feedback):**
       - For each question, confirm what the user reasoned correctly and address their answer thoroughly.
       - If they missed a boundary condition or downstream effect, point directly to the exact line in Section 3 and trace a concrete value through it step-by-step so they see the exact behavior immediately.
     - **### 7. Transferable Engineering Pattern (Gick & Holyoak's Schema Induction for Far Transfer):**
       - De-contextualize the core technique from this commit so the user can reuse it in future projects using 3 bullets:
         - **Pattern Name & Mental Model:** State the standard English industry name for the technique in bold (e.g., **Lazy Read-Repair Migration**, **In-Place Binary Padding**, **Adjacent Token Coalescing**, **Composite Scoped Key**, **Optimistic UI Rollback**) followed by its core mental model in the user's language.
         - **When to Reuse It in Other Projects:** State the exact problem signature in future projects where this pattern is the right tool.
         - **When NOT to Use It (Tradeoff Limit):** State the boundary condition where this pattern breaks down and a different architecture is needed.
     - **### 8. Maintenance Blast Radius & Next Action (Bohner & Arnold's Change Impact Analysis & Soloway's Delocalized Plans):**
       - **Co-Change & Blast Radius Map:** List all files, callers, APIs, or persisted data stores coupled to this change (*"If you modify `<logic X>` in `<File A>` next month, also update `<logic Y>` in `<File B>`"*).
       - **Actionable Hardening Step:** Point out either the most valuable **missing edge-case unit test** not yet covered in the commit (or offer to apply the fix for any `⚠️ Defect Alert` from Section 4), and ask if the user wants to generate that test/fix or save the Blast Radius rule to project docs.

## Red Flags — STOP and Fix Before Sending

- **Arbitrarily truncating core mechanics, code lines, risks, or questions to hit a fixed number cap:** Dropping an important engineering step in Section 3, hiding control-flow lines behind `...` to save lines, omitting a primary pipeline function in Section 2, skipping a guard explanation in `Contrastive Why`, or omitting a real grounded risk in Section 4 instead of explaining every core mechanism completely.
- **Chaining prose clauses with `->` into run-on lines:** Using `->` arrows in `Root cause`, `Key design challenge`, `Contrastive Why`, or `Assumptions & Risks` to join multiple clauses into a long run-on line instead of writing separate, single-idea sentences ending with periods (`.`) and linked by `Old-Before-New` transitions.
- **Horizontal 1-line `Visual Dry-Run` or robotic variable dump in Section 3:** Cramming Input, multiple checks or loop iterations (`i = 0, 1, 2`), and Output onto a single horizontal `->` line, cluttering prose with parenthetical boolean dumps (`(sameLineY = true)`), or keeping a separate 3rd `If omitted` bullet that repeats the `Contrastive Why` instead of breaking each scenario **vertically onto separate indented sub-bullets** (**Input**, **Step 1..N / Iteration 1..N** showing intermediate buffer/variable state, and **Result**).
- **Unexplained format/protocol tokens without a 1-line Syntax Anatomy Legend:** Showing protocol operators, graphics commands, bytecode, custom DSL syntax, or complex regex tokens in Section 1 (`Before -> After`) or Section 3 without placing a 1-line `// Format anatomy: ...` legend at the very top of the snippet, or dumping raw, undecoded protocol gibberish into explanatory comments.
- **Uncommented index, offset, pointer, or slice arithmetic:** Leaving `Index + Length`, `Substring`, `slice`, coordinate math, or pointer offsets uncommented because they look like "basic syntax" instead of writing a Subgoal Comment above the line with small concrete numbers (`e.g., starts at pos 100 + length 20 = ends at pos 120`).
- **Quizzing on unexplained mechanics or omitted (`...`) code in Section 5, or skipping Sections 6–8 in Turn 2:** Asking a Section 5 question about a secondary guard or condition whose purpose was not explained in Section 3, or whose downstream effect relies on code lines that were hidden behind `...` in Section 3, asking two questions that test the same concept, or failing to structure the Turn-2 answer review into **Sections 6, 7, and 8** (`Answer Review & Diagnostic Feedback`, `Transferable Engineering Pattern`, and `Maintenance Blast Radius & Next Action`).
- **Ungrounded or missing Domain Model context:** Guessing the business meaning of undocumented internal codes as fact instead of labeling them an *inferred hypothesis*, forcing a fake end-user UI story onto a pure infrastructure diff instead of using a Runtime/System trigger, or omitting real-world triggers, divergent domain reasons, and user impact across Sections 2–4.
- **Incomplete Section 4 audit or bare breakpoint:** Skipping the check for unseen callers, persisted-data compatibility, silent-failure `catch`/`fallback` blocks, `O(N^2)`/`N+1` scale traps, or suggesting a bare `breakpoint` line without the exact state invariant to inspect.
- **Premature complexity or code-symbol soup in Section 1:** Mentioning file names, function names, or local variable names in Section 1 prose bullets, wrapping clauses in literal `[...]` brackets, writing a 1-line abstract variable assignment in `Before -> After` instead of showing 2 items colliding/transforming, putting a `⚠️ Defect Alert` before Section 3, or printing a `Change Map` for a single-purpose commit.
- **Missing concrete example in complex code comments:** Leaving a long string interpolation or regex in Section 3 without an evaluated example string in its inline comment.
- **Telegraphic compression / skipped causal links:** Jumping directly from the initial cause to the final fix without spelling out the intermediate state and concrete failure.
- Any dual-language header or bullet label in the format `<Local Label> (<English Label>):`.
- **Zombie nouns, translationese calques, abstract compression, synonym rotation, or nested multi-clause run-on sentences:** Opening sentences with abstract wrapper nouns (*"The implementation of..."*, *"The execution of..."*), translating English metaphors (`drop`, `isolate`, `deduplicate`, `preserve`) word-by-word into unnatural literal phrases in the user's language, compressing screen symptoms into vague noun phrases (*"loses overlapping slots"*) instead of describing what appears on screen, joining two separate screen symptoms into one sentence with *"simultaneously / and"*, rotating synonyms for the same domain entity, or writing nested multi-clause run-on sentences.
- Over-translating standard developer terms (`git diff`, `commit`, `race condition`, `deadlock`, `in-place`, `offset`, `stream`, `buffer`, `fallback`, `edge case`, `breakpoint`, `unit test`, `mock`, `payload`, `thread`, `async/await`).
- Explaining non-obvious variables, magic numbers, array/regex indices, bitmasks, or boundary conditions in prose above the code AND again inside the code.
- Leaving repetitive property assignments or unchanged boilerplate in `Before -> After` / `Input -> Output` examples instead of replacing unchanged parts with `...`.
- Padding a trivial 1-decision diff with artificial `Step 2` and `Step 3` boilerplate.
