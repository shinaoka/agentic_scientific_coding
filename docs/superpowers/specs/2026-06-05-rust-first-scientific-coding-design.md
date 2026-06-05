# Rust-First Scientific Coding Course Design

Date: 2026-06-05

## Purpose

This document specifies a full rewrite of the course from a Julia-first route
to a Rust-first route for scientific coding with AI agents.

The new course should teach students to start scientific programming in Rust,
use AI agents responsibly, and validate numerical code through tests,
benchmarks, and reproducible workflows. Julia should no longer be the default
language or the first teaching language.

The central technical path is:

- Rust and Cargo from the beginning,
- one-dimensional numerical data through `Vec<T>` ownership and slice
  interfaces such as `&[f64]` and `&mut [f64]`,
- multidimensional arrays and tensors through `tenferro::TypedTensor`,
- `ndarray` and `ndarray-linalg` mentioned as important Rust ecosystem
  alternatives, but not as the standard course path.

## Scope

Rewrite the Quarto book, repository guidance, and local agent instructions so
that the normal student workflow is Rust-first.

The rewrite includes:

- updating `README.md`,
- updating `AGENTS.md`,
- updating `book/index.qmd`,
- rewriting the Quarto table of contents in `book/_quarto.yml`,
- replacing Julia getting-started pages with Rust/Cargo pages,
- replacing `book/julia-syntax-basics/` with Rust basics material,
- absorbing the old abstraction/interface chapter into Rust basics,
- absorbing the existing Rust practice chapter into earlier Rust-first
  chapters,
- rewriting scientific-computing examples in Rust,
- adding tenferro-centered multidimensional-array material,
- expanding matrix multiplication into a performance case study,
- adding a small automatic differentiation stub page,
- adding a new `.agents/skills/review-rust-code/` skill for reviewing student
  Rust scientific-coding work.

Backups under `backups/` are historical material and do not need to be edited.

## Non-Goals

The rewrite should not become a comprehensive Rust textbook.

The course should not require students to write every line of Rust by hand
without AI assistance. The teaching goal is to help students specify problems,
read generated Rust, review diffs, design tests, validate results, and explain
scientific assumptions.

The course should not make `Vec<Vec<f64>>` a normal multidimensional-array
representation for numerical computing. Nested vectors may appear only as an
anti-pattern or short contrast example.

The course should not require Jupyter notebooks.

The course should not present `ndarray` or `ndarray-linalg` as the main array
route, although both should remain visible as ecosystem alternatives.

## Course Structure

Use this high-level section structure:

1. Safe Workflow Before Coding
2. Learning with AI and Web Search
3. Essential Computer Model
4. Rust Basics for Scientific Code
5. Correctness and Validation
6. Algorithmic Thinking
7. Scientific Computing with Rust and tenferro
8. Advanced Scientific Topics
9. Practical Project Exercises

Do not keep a separate advanced Rust-agent workflow section. Its material should
be absorbed into workflow, Rust basics, validation, tenferro, and performance
chapters.

## Rust Basics

The Rust basics section should replace the old Julia syntax section and absorb
the old types/abstraction/interfaces section.

It should cover:

- variables and state,
- scalar types and numeric values,
- conditionals,
- loops,
- functions,
- `Vec<T>`,
- one-dimensional slices,
- ownership and borrowing,
- mutable borrowing,
- `struct`,
- `enum`,
- `trait`,
- modules and crates.

The early numerical examples should prefer small functions over large programs.
For one-dimensional numerical input, teach function signatures such as:

```rust
fn mean(xs: &[f64]) -> f64
```

and mutation signatures such as:

```rust
fn scale_in_place(xs: &mut [f64], alpha: f64)
```

Students should learn to separate ownership at the program boundary from
borrowing at the function boundary.

## Array and Tensor Policy

Use this policy consistently in the course:

- one-dimensional numerical data: `Vec<f64>`, `&[f64]`, and `&mut [f64]`;
- two-dimensional and higher numerical arrays/tensors:
  `tenferro::TypedTensor`;
- low-level performance exercises: flat buffers such as `Vec<f64>` with
  explicit shape and indexing are allowed;
- nested vectors such as `Vec<Vec<f64>>` are not allowed as the standard
  representation for numerical arrays.

Nested vectors should be described as poor defaults for scientific arrays
because they do not guarantee a single contiguous data buffer, make shape
validation awkward, and obscure cache-friendly access patterns.

The tenferro dependency should be shown as a Git dependency tracking the main
branch:

```toml
tenferro = { git = "https://github.com/tensor4all/tenferro-rs", branch = "main" }
```

Student projects should commit `Cargo.lock` so the resolved Git revision is
recorded. In prose, this can be described as following `origin/main`, but Cargo
examples should use `branch = "main"`.

## tenferro Section

Add or rewrite scientific-computing material so that `tenferro::TypedTensor` is
the standard representation for 2D and higher examples.

The tenferro material should include:

- what a typed tensor represents,
- shape and axis meaning,
- why shape belongs in the type or tensor metadata rather than in informal
  comments,
- conversion from small flat data examples when useful,
- validation of shape and numerical output,
- comparison points with `ndarray` and `ndarray-linalg`.

The Rust array ecosystem page should state that `ndarray` and `ndarray-linalg`
are mature and important alternatives. The course path still uses tenferro for
multidimensional examples both for pedagogy and to expose students to the
developing tensor4all Rust ecosystem.

## Scientific Computing Pages

Rewrite the scientific-computing pages in Rust:

- numerical integration,
- root finding,
- linear algebra computations,
- Monte Carlo and randomness.

Each page should keep the standard subsection structure:

- Explanation,
- Things to look up,
- Exercise,
- Notes for the exercise.

Small one-dimensional checks should use slices. Multidimensional examples should
use `tenferro::TypedTensor` unless the exercise is deliberately about flat
buffer indexing.

## Matrix Multiplication Case Study

Expand the matrix multiplication material into a performance case study.

The page should ask students to work with an AI agent to:

1. specify row/column shape conventions and a reference check,
2. generate a simple safe Rust GEMM implementation,
3. test correctness on small hand-checkable matrices,
4. run a controlled benchmark,
5. compare against a tenferro-based implementation or operation,
6. ask the agent to improve the hand-written implementation,
7. evaluate whether the optimized code still satisfies the same scientific
   contract.

The page should introduce terms such as:

- cache-friendly access,
- contiguous memory,
- blocking or tiling,
- vectorization,
- bounds-check elimination,
- memory layout,
- FLOPs and memory bandwidth.

The discussion of Rust bounds checks should be precise:

Safe indexing such as `xs[i]` includes bounds checks unless the compiler can
prove the access is in range. Up-front shape and buffer-length checks, row
slices, and loop forms that make bounds obvious can help the optimizer remove
checks. Unsafe indexing should not be part of the main student route. If it
appears in an advanced exercise, the required invariants and tests must be
explicit.

The review checklist for performance-oriented work should stay general:

- performance claims need controlled measurements,
- correctness tests must still pass after optimization,
- memory access patterns should be explained,
- optimization must not obscure the scientific contract,
- unsafe code, if present, needs explicit invariants and tests.

Specific benchmark settings such as one-thread runs, blocking, or vectorization
belong in the GEMM page itself, not in every performance review.

## Advanced Scientific Topics

Add a short advanced-topic stub page for automatic differentiation.

The page should briefly state:

- what automatic differentiation is,
- why gradients matter in scientific computing,
- why AI-generated differentiation code needs validation,
- that future Rust ecosystem material may connect to tenferro-related work,
  `tidu-rs`, and `chainrules-rs`.

Do not add a full AD tutorial in this rewrite.

## AGENTS.md

Rewrite `AGENTS.md` for Rust-first teaching.

Keep the common work path convention:

```text
work/chapter_x.y/
```

Change examples from Julia files to Rust/Cargo work:

- small single-file experiments may use `answer.rs`,
- exercises that need execution or tests should use a minimal Cargo project,
- normal Rust project files include `Cargo.toml`, `Cargo.lock`, `src/lib.rs`,
  `src/main.rs` when needed, and `tests/`.

Learning Mode should tell agents to act as Rust scientific-coding educators.
Agents should not generate complete exercise solutions at once. They should
first discuss algorithm choice, inputs and outputs, boundary cases, data
representation, and test design.

Exercise review guidance should require checking:

- whether the code is organized into functions or modules,
- whether functions avoid hidden global mutable state,
- whether ownership and borrowing choices are understandable,
- whether 1D numerical interfaces use slices where appropriate,
- whether 2D and higher arrays use `tenferro::TypedTensor` unless there is a
  justified low-level flat-buffer exercise,
- whether nested vectors are avoided for numerical arrays,
- whether unit tests exist and are run with `cargo test`,
- whether cache-friendly access is considered when performance matters.

Editing Mode should say:

- use Rust as the default language for examples and exercise checks,
- use slices for 1D numerical examples,
- use `tenferro::TypedTensor` for 2D and higher examples,
- keep `ndarray` and `ndarray-linalg` as alternatives where useful,
- prefer official Rust and crate documentation over long installation
  procedures,
- do not make notebooks mandatory,
- keep the subsection structure readable and compact.

Remove direct references to Julia as the default language, `juliaup`, Julia REPL
workflow, `.jl` scripts, and the old Julia review skill.

## review-rust-code Skill

Create a new repository skill:

```text
.agents/skills/review-rust-code/SKILL.md
```

This skill should review student Rust scientific-coding exercises for this
course. It should be educational, not a generic production-code review skill.

The skill should instruct agents to:

- read the exercise text and notes for the exercise before judging the answer,
- avoid writing the full solution for the student,
- start from the student's algorithm, inputs, outputs, boundary cases, and test
  design,
- identify correctness risks before style issues,
- check function/module organization,
- check for hidden global mutable state,
- check ownership and borrowing at function boundaries,
- check that 1D data use slices where appropriate,
- reject nested vectors as the normal representation for numerical arrays,
- check that 2D and higher numerical arrays use `tenferro::TypedTensor` unless a
  flat-buffer implementation is explicitly part of the exercise,
- check tests, including small hand-checkable examples and edge cases,
- check that `cargo test` has been run or clearly report when it has not,
- check cache-friendly access when performance claims are made,
- distinguish scientific validation issues from Rust syntax issues.

Remove the old `.agents/skills/review-julia-exercise/` skill directory during
the rewrite. The historical backup copies under `backups/` do not need to be
edited.

## File-Level Rewrite Plan

Replace or rename these getting-started files:

- `book/getting-started/default-language-julia.qmd`
  -> `book/getting-started/default-language-rust.qmd`
- `book/getting-started/installing-julia.qmd`
  -> `book/getting-started/installing-rust.qmd`
- `book/getting-started/running-julia.qmd`
  -> `book/getting-started/running-rust.qmd`

Replace the old Julia basics directory with Rust basics:

```text
book/rust-basics/
  variables-and-state.qmd
  types-and-numeric-values.qmd
  conditionals.qmd
  loops.qmd
  functions.qmd
  vec-and-slices.qmd
  ownership-and-borrowing.qmd
  struct-enum-trait.qmd
  modules-and-crates.qmd
```

Remove the old standalone chapter role of:

```text
book/types-abstraction-interfaces/
book/rust-practice/
```

Relevant material from those directories should be absorbed into Rust basics,
validation, tenferro, and AI-agent workflow pages before the directories are
removed from the rendered book.

Add or rewrite tenferro-related pages:

```text
book/scientific-computing/tenferro-typed-tensor.qmd
book/scientific-computing/rust-array-ecosystem.qmd
```

Add the automatic differentiation stub page:

```text
book/advanced-scientific-topics/automatic-differentiation.qmd
```

Update `book/_quarto.yml` so the render list and table of contents match the
new structure.

## Verification

After implementation, verify:

- no rendered book page outside backups presents Julia as the default language,
- no current teaching text tells students to install Julia or run `.jl` files,
- no current teaching text points to `review-julia-exercise`,
- `book/_quarto.yml` references only existing files,
- the Quarto book builds,
- the new `review-rust-code` skill has valid frontmatter and clear triggering
  text,
- Rust examples compile where they are intended to be runnable,
- Cargo examples use committed lock files when a complete exercise project is
  included.

## Completion Criteria

The rewrite is complete when the course reads as a Rust-first scientific coding
note, the normal array path is slices for 1D and `tenferro::TypedTensor` for
2D and higher, the old Julia-first route is gone from active material, and the
repository includes a Rust-specific review skill for student exercise work.
