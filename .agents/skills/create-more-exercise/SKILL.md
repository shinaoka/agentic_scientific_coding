---
name: create-more-exercise
description: Create deeper follow-up exercises for a chapter or subsection in this course. Use when asked to make additional, more advanced, or more probing exercises for the lecture material.
---

# Create More Exercise

Use this skill to create additional exercises that help students understand a
course chapter or subsection more deeply than the existing exercise.

## First Step

Start by confirming the target chapter, subsection, or source file. If it is
not provided in the user's request, ask for it before creating exercises.

Accept targets such as:

- `5.1`
- `Section 6`
- `Variables and state`
- a source file under `book/`

## Workflow

1. Read `AGENTS.md`.
2. Locate the relevant `.qmd` file under `book/`.
3. Read the page's explanation, things to look up, exercise, and notes.
4. Create exercises that go deeper than the existing exercise. Do not merely
   rephrase the current task.
5. Do not provide full solutions unless the user explicitly asks for them.
6. Keep the style consistent with the course:

   - short concept-map explanations,
   - hand-written reasoning before coding,
   - Rust as the default language for small checks,
   - no long syntax lists or installation procedures,
   - validation, boundary cases, and reproducibility when relevant.

## Output

Provide 2-4 additional exercises. For each exercise, include:

- a short title,
- the task,
- what the student should learn or check,
- brief notes for the exercise.

When the topic involves Rust or scientific coding, prefer exercises that make
students reason about inputs, outputs, boundary cases, tests, functions,
unnecessary global state, ownership and borrowing, numerical validation, or
reproducibility.

If the user asks to edit the teaching material, insert the exercise using the
page's existing structure and keep the change concise.
