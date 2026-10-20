---
name: learn-from-code
description: Use when the user asks to explain recent code changes, review a git diff or commit for learning, understand architectural decisions, avoid comprehension debt, or practice hands-on coding via TODO(human) exercises.
---

# Learn From Code

Build codebase mental models and debugging intuition from code changes in 90–120 seconds of reading (**350–500 prose words total** outside code blocks).

## Operating Modes

- **Mode A — Post-Implementation Review (Default):** Explain a completed `git diff` or commit using the 5-Section Output Contract below, then pause for active retrieval.
- **Mode B — In-Task Active Learning (`TODO(human)`):** Survey existing patterns, generate surrounding boilerplate, and leave 5–15 lines at the key decision point marked `TODO(human)` with context, contract, and tradeoffs. Wait for the user to implement it before reviewing.

## Language & Developer Register Rules

1. **Match the User's Language Automatically:**
   - Always respond in the exact language used by the user in the conversation or prompt (e.g., Vietnamese, English, Japanese, Korean), unless explicitly asked otherwise.
   - Localize all section headers, step labels (`Before -> After` / `Trước -> Sau` / `変更前 -> 変更後`, `Step 1 -> Step 2` / `Bước 1 -> Bước 2` / `ステップ1 -> ステップ2`), bullet labels, and inline explanatory comments inside quoted code snippets into the user's language.
2. **Zero Dual-Language Header Clutter:**
   - Never output mechanical bilingual headers or labels that pair two languages together (e.g., never write `Problem (Triệu chứng/Vấn đề):`, `Mô hình trực quan (Mental model):`, `Đã kiểm chứng (Verified):`, or `Why & Tradeoffs (Tại sao & Đánh đổi):`).
   - Use a single, natural label in the active response language.
3. **Natural Developer Code-Switching (Never Over-Translate Core Terms):**
   - Keep universal engineering terms that native developers use daily in their standard English/Katakana form (`git diff`, `commit`, `race condition`, `deadlock`, `in-place`, `offset`, `stream`, `buffer`, `fallback`, `edge case`, `breakpoint`, `unit test`, `mock`, `payload`, `thread`, `async/await`). Never force awkward literal translations (such as "điều kiện đua", "khóa chết", or "điểm ngắt").
   - For unfamiliar domain or binary/protocol tokens (`xref`, `FlateDecode`, `CIDFont`), keep the original token and attach a brief 2–4 word plain-language appositive on first mention (e.g., "bảng mục lục `xref`", "the `xref` byte-index table", "`xref`（バイト位置テーブル）").
4. **Idiomatic Senior-Engineer Tone:**
   - **Vietnamese:** Concise, direct, active voice, using everyday Vietnamese developer phrasing rather than stiff word-by-word passive translations from English.
   - **English:** Crisp, active-voice technical prose.
   - **Japanese:** Clear polite technical style (`です/ます`調) with natural industry Katakana/English terminology (`フォールバック`, `エッジケース`, `インプレース`, `オフセット`, `ブレークポイント`).

## Core Readability & Cognitive Load Rules

1. **Zero Split-Attention (Explain Inside Code, Not Around It):**
   - Put all variable meanings, boundary checks, and Regex group/index labels (`Groups[3]`) directly into short inline comments inside the code snippet in the user's language.
   - Never repeat prose summaries of variables above the code, and never use repetitive sub-headers inside learning points.
   - Below each code snippet, write **at most 2 bullets** (localized to the user's language):
     - **Why it works this way:** The design reason in 1–2 plain sentences.
     - **What breaks if removed:** The exact bug or visual/runtime symptom that happens in practice.
2. **Working Memory Cap (Max 3 Function Names in Flow):**
   - In the execution flow section, summarize the pipeline in **3–4 plain-language action steps** naming **at most 2–3 main functions**. Do not dump 6–8 internal helper names or low-level spec operators at once.
3. **Noise-Free `Before -> After` Data Examples:**
   - Replace irrelevant boilerplate, hex bytes, or unchanged operators with `...` so the reader spots the exact transformation in 3 seconds.
4. **Chronological Story Pipeline (`Step 1 -> Step 2 -> Step 3`):**
   - Order learning points sequentially along the actual data flow (e.g., *1. When is merging allowed? -> 2. How are tokens merged and aligned? -> 3. How is the file overwritten safely?*).
   - Title each point with the practical question that step answers.
5. **Link Once Per Section:**
   - Link a file or symbol only on its **first appearance** in a section. Do not spam clickable links on every line.

## 5-Section Output Contract (Mode A Recipe)

Localize the 5 section titles and bullet labels below into the user's language (single-language labels only):

### 1. Big Picture (Problem & Core Idea)
- **Symptom:** 1 sentence describing the user-visible bug or limitation.
- **Root cause:** 1–2 sentences explaining why it happens (open with a 1-sentence everyday analogy before naming technical specs).
- **Fix:** 1 sentence stating what changed and where.
- **Concrete example (`Before -> After`):** A 2–4 line noise-free snippet showing the core transformation of the entire commit right here so the reader grasps the goal immediately.

### 2. Main Execution Flow (3–4 steps)
- A numbered list of 3–4 plain-language steps tracing how data moves from the caller to the fix, naming **at most 2–3 primary functions**.

### 3. Core Code Steps (Up to 3 chronological steps)
For each step (`#### Step 1: [Practical question?]`, `#### Step 2: ...`, `#### Step 3: ...`):
- **1 plain-language transition sentence** connecting this step to the previous one.
- **Short code snippet (8–18 lines):** Put all explanations of variables, indices (`Groups[1]`, `Groups[3]`), and edge conditions directly in inline comments (in the user's language) above/beside the code lines.
- **2 short bullets below the code:**
  - **Why this design:** Why this approach was chosen.
  - **If omitted:** What concrete bug occurs in practice.

### 4. Verification & Hidden Risks
- **Verified:** 1 bullet on what is proven to work + a 1-line **Quick verification** (exact `breakpoint` line, log, or visual check).
- **Assumptions & Risks:** 1–2 bullets on unverified `edge cases` or library upgrade risks.

### 5. Debugging Check (1–2 short questions)
- Ask **1–2 single-focus debugging or boundary questions** (answerable in 2–3 sentences).
- **Only ask about code shown in Section 3:** Every question must target a specific condition or line visible in the Section 3 snippets (never ask about unseen helper functions or omitted lines) and quote that condition inline so the user never has to scroll up or open another file.
- **Wait for the user's reply** before giving the answers.
- *(Optional)* Add a 5-bullet repo gotcha note **only** if omitting it would cause a bug next month and it is not already recorded in project memory; ask before writing to repo files.

## Red Flags — STOP and Trim Before Sending

- Total prose outside code blocks exceeds **500 words**.
- Any dual-language header or bullet label (e.g., `Đã kiểm chứng (Verified):`, `Triệu chứng (Symptom):`).
- Over-translating standard developer terms (`breakpoint`, `offset`, `fallback`, `in-place`, `edge case`, `race condition`, `stream`, `buffer`).
- Explaining `Groups[1]` or variables in prose above the code AND again inside the code.
- Listing more than 3 function names in Section 2 (Main Execution Flow).
- Leaving raw hex/operator clutter in `Before -> After` examples instead of replacing unchanged parts with `...`.
- Asking a Section 5 question about a function or line not shown in Section 3.
