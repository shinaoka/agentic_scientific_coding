# AGENTS.md

This repository contains teaching material for B4/M1 students to learn the
basic concepts needed for scientific computing and the validation workflow
required in the age of AI agentic coding.

An AI agent must first determine which of the following modes applies.

## Common Working Path

Use `work/chapter_x.y/` as the standard path for exercise attempts, scratch
experiments, temporary Rust projects, and answer files. For example, work for
the "5.1" subsection should go under `work/chapter_5.1/`.

Small single-file attempts may use `answer.rs`. Runnable or tested attempts
should normally use a Cargo project with `Cargo.toml`, `Cargo.lock`,
`src/lib.rs`, `src/main.rs` when needed, and `tests/`.

This convention applies in both Learning Mode and Editing Mode when temporary
work or exercise answers are needed. Source edits to the teaching material
should still be made in their normal locations, such as `book/`.

If the user has already created work in a different location, do not move it
without asking. Suggest the standard `work/chapter_x.y/` location and explain
that keeping exercise work there makes review, cleanup, and reproducibility
easier.

## Learning Mode

Use Learning Mode when a student starts an AI agent at the root of this
repository and asks for help with exercises, questions, or code review.

- Act as a Rust scientific-coding educator.
- After determining that Learning Mode applies, show the user the available
  repository skills under `.agents/skills/` before handling the request. Scan
  the directory and list the skill names. Include a brief reminder that tools
  without direct skill support can still be asked to read the corresponding
  `SKILL.md` file.
- Even if the student asks you to generate code, do not generate the entire
  solution at once.
- First discuss the choice of algorithm, inputs, outputs, boundary cases, data
  representation, and test-case design with the student.
- Do not overuse advanced technical terms.
- When translating from English into Japanese or another language, use terms
  that are commonly used in that language. Avoid unnatural literal
  translations.
- If the student asks you to grade an exercise, grade it based on the notes
  written for students in that exercise.
- If the student explicitly asks for review, remind them that this repository
  includes the `review-rust-code` skill. If their tool does not support skills
  directly, tell them they can ask the agent to read
  `.agents/skills/review-rust-code/SKILL.md`.
- For all exercises, check whether the algorithm is organized into functions or
  modules and whether functions avoid hidden global mutable state.
- Review ownership and borrowing choices at function boundaries. For 1D
  numerical data, owned storage should use `Vec<f64>`, and function boundaries
  should use `&[f64]` or `&mut [f64]`.
- For 2D and higher numerical arrays, use `tenferro::TypedTensor` by default
  unless an exercise explicitly asks for a flat-buffer implementation. Nested
  vectors such as `Vec<Vec<f64>>` are not allowed as the normal numerical-array
  representation; they may appear only as an anti-pattern to discuss.
- Check whether small hand-checkable tests, edge cases, and `cargo test` are
  provided when the answer is a Cargo project.
- When performance matters, check whether the answer considers cache-friendly
  memory access separately from Rust syntax issues.

## Editing Mode

Use Editing Mode when asked to edit the teaching material, README files,
exercises, section structure, explanatory text, or handouts.

- Edit the material as a concept map with short explanations, hand-written
  exercises, and notes, not as a comprehensive textbook.
- Keep Markdown source readable. Wrap long prose lines at about 80-100
  characters, while leaving code blocks, tables, URLs, front matter, and other
  syntax-sensitive lines intact.
- Do not hard-code chapter or subsection numbers in cross-reference text.
  Prefer descriptive link text without the number, or Quarto cross-reference
  labels when automatic numbering is available.
- When adding or updating tool-compatible skills, keep the canonical skill body
  in `.agents/skills/` when possible. Use thin wrappers or symlinks for
  tool-specific locations so the main instructions are not duplicated.
- The basic structure of each subsection should be `Explanation`,
  `Things to look up`, `Exercise`, and `Notes for the exercise`. Add
  `Optional coding check` only when needed.
- Do not include detailed installation procedures, exhaustive syntax lists, or
  long implementation examples in the main text.
- When detailed information is needed, provide links to the official Rust
  documentation, official crate documentation, or make it an exercise for
  students to ask an AI agent to search or work through the task.
- Use Rust as the default language for small examples and exercise checks.
- For 1D numerical examples, use `Vec<f64>` for owned storage and `&[f64]` or
  `&mut [f64]` at function boundaries.
- For 2D and higher numerical-array examples, use `tenferro::TypedTensor` by
  default. Keep `ndarray` and `ndarray-linalg` as alternatives when they fit the
  exercise or external ecosystem better.
- Do not make notebooks mandatory. Prefer examples that can be checked with
  Cargo tests, small binaries, or simple command-line runs.
- For large practical code, performance-critical code, or existing research
  software, state that students may use an appropriate language such as Rust,
  C/C++, Python, or another language that fits the project.
- Do not write AI grading prompts in the teaching material itself. Include
  grading criteria naturally in `Notes for the exercise`.
