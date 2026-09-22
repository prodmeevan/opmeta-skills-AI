---
name: plan-super
description: Plan-first workflow with a user-confirmed DDR document (requirements, design, formulas/logic), an ordered workflow, and a hard approval gate before any execution. Use when the user asks to plan a feature or project before building it, wants a requirements/design document, or invokes "plan-super".
---

# Plan Super

Understand first, plan second, execute only after explicit approval. Every unclear point becomes a question to the user — never an assumption.

## Cara pakai (tutorial)

**Install** (sekali saja, pilih salah satu):

```bash
# dari repo GitHub yang berisi skill ini
npx skills add <owner>/repo --skill plan-super
```

atau salin folder `plan-super/` ini ke `.agents/skills/plan-super` di project kamu.

**Memanggil** — sebut namanya di prompt:

- "pakai plan-super untuk bikin fitur login"
- "plan-super: saya mau bikin API kasir"

**Yang akan terjadi:**

1. Aku baca konteks project dulu, lalu tanya 2–5 pertanyaan soal prioritas wajib.
2. Jawab saja — jawabanmu menentukan isi plan (ditandai [M] wajib / [O] opsional).
3. Aku tulis `DDR.md` (requirement, desain, logika, out of scope) + langkah kerja + saran opsional.
4. Aku berhenti dan tanya: **eksekusi** atau **revisi**. Tidak ada kode yang ditulis sebelum kamu bilang "eksekusi".

**Tips:**

- Kalau tidak yakin, jawab "kamu putuskan" — aku usulkan satu default dan minta setuju/tidak, jadi tetap bukan asumsi.
- "Eksekusi" = mulai kerja sesuai plan. "Revisi" + sebutkan apa yang diubah = plan disesuaikan, lalu konfirmasi ulang.
- Skill ini tidak menambah fitur yang tidak kamu minta. Kalau ada saran tambahan, itu opsional dan tidak masuk plan kecuali kamu angkat jadi wajib.

## Rules (apply at every phase)

1. **Never assume.** Anything that affects scope, behavior, priority, or tech choice must be confirmed by the user. If something is unclear, ask a concrete question. If you cannot form a question, say exactly what information is missing. Trivial defaults (file names, formatting) are allowed but must be stated, not hidden.
2. **No overengineering.** The plan covers only what the user marked mandatory. No extra layers, abstractions, config options, or "while we're at it" features.
3. **No AI slop.** Plain and concrete. No filler, no buzzwords, no emoji decoration, no restating the request in vaguer words. If a sentence carries no information, delete it.
4. **Logic first.** Every decision in the plan must have a reason connected to the user's answers. If a decision has no reason, remove it or ask.
5. **User's language.** All conversation and documents are written in the language the user writes in.

## Phases

### 1. Understand

- Restate the goal in 1–2 plain sentences.
- If working in an existing codebase, read the relevant code first so the questions are informed, not lazy.
- List what is known vs. what is unclear.

### 2. Ask mandatory priorities

Before writing anything, ask the user what is **mandatory** for this to count as done.

- 2–5 focused questions, multiple choice where possible, one topic per question: must-have scope, constraints, existing code to respect, definition of done.
- Tag every answer [M] (mandatory) or [O] (optional). Only [M] items drive the plan.
- If the user says "you decide", propose one concrete default and ask yes/no. A confirmed default is not an assumption; an unconfirmed one is.

### 3. Write the DDR

Create `DDR.md` in the project root (unless the user picks another location). Sections — only these:

````markdown
# DDR — <project/feature name>

## Overview
<2–3 sentences: what is being built and why>

## Requirements
1. [M] <mandatory requirement, taken from the user's answers>
2. [O] <optional, only if the user mentioned it>

## Design
<how it will be built: structure, files/components, data flow — as simple as possible>

## Formulas / Logic
<precise rules, calculations, or pseudo-code>
<"None" if there are none — do not invent any>

## Out of scope
<explicit list of what will NOT be done>
````

Keep it to roughly one page. Every [M] requirement must be traceable to something the user actually said. Do not add requirements the user never asked for.

### 4. Workflow

Turn the [M] requirements into an ordered, numbered step list covering exactly those requirements — nothing extra. Each step says what is done and which files/parts it touches. The last step is always verification: how "done" will be checked (build, tests, manual check).

### 5. Suggestions (optional)

Up to 3 short suggestions for things deliberately left out (edge cases, tests, future work), clearly labeled optional. They are not part of the plan unless the user promotes them to [M].

### 6. Confirmation gate

Present the plan and ask, in the user's language:

1. Execute as planned
2. Revise (tell me what to change)
3. Promote a suggestion to mandatory

- **Execute** → run the workflow exactly as written. If reality contradicts the DDR mid-execution, stop and re-confirm — never deviate silently.
- **Revise** → change only the affected sections/steps, show what changed, and ask again. Loop until approved.
- **Never execute without an explicit approval at this gate.**
