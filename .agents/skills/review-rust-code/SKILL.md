---
name: review-rust-code
description: Review a student's Rust scientific-coding exercise answer for this course. Use when asked to review Rust code, Cargo projects, answer.rs, src/lib.rs, tests, performance exercises, tenferro examples, or scientific-coding exercise solutions. Follow AGENTS.md, act as an educator, and do not generate the full solution.
---

# Review Rust Code

Use this skill to review a student's Rust scientific-coding exercise answer in
this repository. The goal is educational feedback, not replacing the student's
work.

## Inputs

- Path to `answer.rs`, a Cargo project, or an exercise work directory.
- Optional path to the relevant exercise page or handout.
- Whether this is review-only or editing is allowed. Default to review-only.

## Workflow

1. Read `AGENTS.md` and follow Learning Mode.
2. Read the relevant exercise text and Notes for the exercise when the section
   or path is clear.
3. Inspect the submitted Rust file or Cargo project. If no path is given, ask
   for the path.
4. If a Cargo project is present, run `cargo test` from the project directory.
   If only a single file is present, inspect it and report that no project-level
   tests were run.
5. Review correctness before style.
6. Report findings first, with file and line references when possible.
7. Give concise improvement suggestions without writing the full solution.
8. If editing is explicitly allowed, make only minimal changes and explain them.

## Review Checklist

- Does the answer match the exercise and notes?
- Are inputs, outputs, assumptions, and units clear?
- Are boundary cases and failure modes considered?
- Is the code organized into functions or modules?
- Do functions avoid hidden global mutable state?
- Are ownership and borrowing choices understandable at function boundaries?
- For 1D numerical data, does owned storage use `Vec<f64>` and do function
  boundaries use `&[f64]` or `&mut [f64]`?
- For 2D and higher numerical data, is `tenferro::TypedTensor` used unless the
  exercise explicitly asks for a flat-buffer implementation?
- Are nested vectors such as `Vec<Vec<f64>>` avoided for numerical arrays?
- Are small hand-checkable tests and edge cases included?
- Was `cargo test` run, or is the missing test run clearly reported?
- For numerical experiments that generate plots, are computation and plotting
  separated so that results and metadata are written to a file before plotting?
- Does the saved result format fit the data size and shape, using JSON or plain
  text for small outputs and formats such as `.npy`, `.npz`, or HDF5 for large
  or multidimensional arrays?
- When performance is discussed, is cache-friendly access considered?
- Are scientific validation issues separated from Rust syntax issues?

## Boundaries

Do not generate a complete replacement solution for the student. Prefer hints,
targeted examples, and test suggestions. Unsafe Rust should be treated as an
advanced topic: require explicit invariants, tests, and a reason it is needed.
