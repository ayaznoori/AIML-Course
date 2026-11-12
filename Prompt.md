# Lecture Script — Generation Prompt (Do / Don't + Master Template)

Use this document when asking an AI or writer to produce **facilitator-facing** lecture scripts. It reflects the [AIML sample script](https://github.com/ayaznoori/AIML-Course/blob/main/lecture_script_python_variables_conditions_colab.md), curriculum guidelines, and fixes found when comparing prompts to weak outputs.

---

## Quick gap check (prompt vs sample vs weak output)

| Area | Sample script (strong) | Common weak output | Fix in prompt |
|------|------------------------|--------------------|---------------|
| Structure | **Flow table** + numbered **blocks** with minutes | One long narrative | Require **Lecture Flow at a Glance** table |
| LO alignment | Every block maps to listed LOs | Title says FastAPI, LOs say data/analytics | **Title must match LOs**; reject mismatched titles |
| Hooks | Opening **story mistake** with consequence | Generic “today we learn…” | Require **problem with wrong outcome** in opening |
| Demos | **Predict before running** every demo | Demo with no prediction | Mandatory prediction line before each demo |
| Code for beginners | Line-by-line **[Script:]** after demo | Assumes prior coding | **Explain each line** in plain English when audience is beginner |
| Theory | Tables on board, classification **exercises** | Definitions only | Require **in-class solvable mini-task** per major concept |
| Timing | ~110 min; blocks sum to duration | Vague “sections” | Every block has **minutes**; total ≈ provided duration |
| Forward refs | Stays in “today” | “Next session…”, “you will learn later…” | Explicit **DON’T** list |
| Facilitator meta | Uses blocks, not “Slide 3” | “Recording”, “trainer”, “session” in **[Script:]** | **[Script:]** rules below |
| Instructor notes | `> 🎯 **Instructor Note:**` in blockquote | Random emoji decoration | **Only** 🎯 for Instructor Note; no other emojis |

---

## DO

### Inputs (you must provide every time)

- **Lecture title** (exact string for H1)
- **Learning objectives** (numbered; script may not teach outside these)
- **Duration** (minutes; e.g. 90, 110)
- **Delivery type:** `IM` (Instructional Mentor / facilitator-led live) or `Professor` (academic lecture style—still facilitator script, slightly more formal **[Script:]**)
- **Audience level** (e.g. beginner, no prior tech background)
- Optional: **topic summary** or **lecture notes** URL/path (scope boundary only)
- Optional: **Practical constraints** (Colab, no install, Hindi/English medium, etc.)

### Structure and flow

- Start the file with `# Lecture Script: {Title}` and a one-line **Format | Duration | Level** line (metadata for facilitators only—not spoken to learners).
- Add **## Lecture Flow at a Glance** — markdown table: Block | Topic | Time (minutes). Sum of times ≈ **Duration** (±5 min).
- List **## Learning Objectives** exactly as supplied (copy, do not rewrite scope).
- Organize body as **## Block N — {Topic}** aligned to LOs (one block per major LO or sub-skill).
- Within each block use: **Problem → Explain → Walkthrough → Demo → Recap** (label these words as subheadings where helpful).
- Add **### Recap of Block N before moving on:** bullet list after each major block (sample pattern).
- End with **## Lecture Summary** — one bullet per LO + final bullet on **practical value**.

### Pedagogy

- **Why Does This Matter?** early (include hook activity/question).
- **What Is the Concept?** — official definition, **In simple words**, **Real-life example** (India-friendly: UPI, kirana, college ERP, Zomato, metro, etc. when relevant).
- **How Do We Apply It?** per LO: Problem → Translate logic → Write code (if any) → **Predict before running** → Explain result.
- Include **1–2 runnable demos per coding block**, **max 10 lines** each, **only concepts already introduced**.
- Before every demo: **Predict before running: What will happen?** + 🎯 Instructor Note on expected answer.
- For **theory-only** topics: at least one **in-class solvable exercise** (individual, not group—suitable for Zoom): classify, label lifecycle stage, compute on board, role-play, exit ticket.
- Use **🎯 Instructor Note** in blockquote: `> 🎯 **Instructor Note:**` for timing, gate checks (“do not proceed until…”), board content, common wrong answers.
- Use **[Script:]** for verbatim spoken lines (energetic, clear, practical).
- Use ***Pause:*** for confusion points and volunteer answers.
- Relate **every abstract term** to one **concrete real-world example** before moving on.
- For **non-tech learners** and any code: after each demo, **[Script:]** walks **line by line** (“this line means…”); avoid jargon without definition.

### Delivery type nuance

- **IM:** More dialogue, hands-up, prediction, board work, informal plain English.
- **Professor:** Same structure; **[Script:]** may be slightly more formal; still no “this course/session/module” phrasing in spoken lines.

### Style

- Markdown only in output; **no** outer triple-backtick wrapper around the whole document.
- Bold important terms; use tables for comparisons when useful (Python vs JS only when LO-relevant).
- Mental models and **common mistakes** inside concept sections, not a separate “mistakes lecture.”

---

## DON'T

### Scope and metadata

- Do **not** teach topics, APIs, frameworks, or tools **not** implied by the **Learning Objectives**.
- Do **not** paste a **wrong title** (e.g. FastAPI) when LOs are about another topic—**title and LOs must match**.
- Do **not** include student-facing lecture notes; output is **facilitator script only**.
- Do **not** put images unless explicitly requested.

### Wording in [Script:] and learner-facing lines

- Do **not** say: lecture, trainer, facilitator, session, recording, slides, deck, “turn to slide”, “we are live”, module number, week number.
- Do **not** say: “will be covered in the **next** session/lesson/class”, “upcoming session”, “future unit”, “later in the course”, “you will learn in session X”, “as we saw in session Y”.
- Do **not** reference **previous** content by session number—only “earlier today” or a concept name if needed.
- Do **not** use decorative emojis except **🎯** on **Instructor Note** lines (no stars, fire, checkmarks in prose).

### Pedagogy

- Do **not** use group-only activities if delivery is Zoom solo-learner default—default to **individual** activities.
- Do **not** use “Ask students to…” — write activities as facilitator actions: “Read aloud…”, “Learners write…”, “Silent classify…”.
- Do **not** ship **partial code** in demos when the guideline asks full runnable snippets (keep ≤10 lines but complete).
- Do **not** skip **Predict before running** for any runnable demo.
- Do **not** omit **time boxes**—un timed scripts are rejected.

### Internal instructions

- Do **not** expose prompt meta in the script (“keep it lite”, “light version”, “per guidelines”, “learning objective 3”).
- Do **not** include AWS keys, URLs to internal tools, or “AI generated” disclaimers.

---

## Master prompt (copy, fill placeholders, send to writer/AI)

```markdown
You are a senior curriculum designer. Produce the Lecture Script (facilitator-facing) as production-ready Markdown.

## Provided inputs
- **Lecture title:** {LECTURE_TITLE}
- **Duration:** {DURATION_MINUTES} minutes
- **Delivery type:** {IM | Professor}
- **Audience:** {e.g. Beginner, no prior tech background}
- **Learning objectives (strict scope—do not add topics):**
  1. {LO_1}
  2. {LO_2}
  3. {LO_3}
  …

Optional context (scope only, do not quote verbatim as “session notes”): {SUMMARY_OR_NOTES_PATH}

## Output rules
- Return **Markdown only**. No preamble before the H1. No outer triple-backtick wrapper.
- H1: `# Lecture Script: {LECTURE_TITLE}`
- Include: Format | Duration | Level line; **Lecture Flow at a Glance** table; copied LO list; blocks that sum to ~{DURATION_MINUTES} minutes.
- Pedagogy: Problem → Explain → Walkthrough → Demo → Recap per major block; **Why Does This Matter?** with 🎯 Instructor Note + [Script:]; **What Is the Concept?** with definitions, mental models, common mistakes; **How Do We Apply It?** per LO with Problem → Translate logic → Code (if any) → Predict before running → Explain result.
- Demos: max 10 lines, runnable, only taught concepts; explain code line-by-line for non-tech beginners in [Script:].
- Theory blocks: at least one practical in-class exercise (individual, Zoom-friendly).
- Real-world examples: relatable, plain Indian English where appropriate.
- Use only 🎯 for Instructor Note (blockquote). Use [Script:] and *Pause:* as specified.
- **DO NOT** reference lecture/trainer/session/recording/slides; **DO NOT** forward-reference future lessons or say “next session”; **DO NOT** use session numbers; **DO NOT** use emojis except 🎯 on Instructor Notes; **DO NOT** teach outside the LOs; **DO NOT** mismatch title and LOs.

## Reference style
Match structure and tone of this sample (flow table, blocks, predict-before-run, block recaps):
https://github.com/ayaznoori/AIML-Course/blob/main/lecture_script_python_variables_conditions_colab.md

End with **Lecture Summary**: one bullet per LO + one bullet on practical value.
```

---

## Quality checklist (before publishing script)

- [ ] Title matches every LO; no stray framework in title
- [ ] Flow table minutes sum ≈ provided duration
- [ ] Every LO appears in ≥1 block and in final summary
- [ ] Opening hook uses a **concrete mistake or consequence**
- [ ] Each demo has **Predict before running** + explanation
- [ ] Code demos ≤10 lines OR theory block has solvable exercise
- [ ] No “next session / future lesson / session number” in file
- [ ] No lecture/trainer/session/recording/slides in **[Script:]**
- [ ] Only 🎯 on Instructor Notes; blockquotes used for notes
- [ ] Recap bullets at end of each major block
- [ ] **[Script:]** explains code for beginners where code exists

---

## Example filled inputs (Data & Intelligence Landscape)

```text
Lecture title: MasterClass: The Data and Intelligence Landscape
Duration: 110
Delivery type: IM
Audience: Beginner, no prior tech background
LOs:
1. Describe the information lifecycle: generate, store, process, consume
2. Distinguish structured, semi-structured, unstructured data and uses
3. Map AI/analytics ecosystem and data analyst role
4. Explain how ML emerged and why it matters for analytics
```

Use the **Master prompt** above with these inputs—not a FastAPI title unless LOs are about FastAPI.

---

## Optional: regenerate script for a topic

When regenerating `lecture-script.md` for a unit:

1. Paste **Master prompt** + filled inputs + link to student `lecture-notes.md` (scope only).
2. Run **Quality checklist**.
3. Save to `docs/lessons/{unit-slug}/lecture-script.md`.
