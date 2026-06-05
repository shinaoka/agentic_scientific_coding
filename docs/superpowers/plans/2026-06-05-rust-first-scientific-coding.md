# Rust-First Scientific Coding Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rewrite the course into a Rust-first scientific coding note with
slices for 1D data, tenferro typed tensors for 2D and higher arrays, and a
course-specific Rust review skill.

**Architecture:** Treat the rewrite as a documentation migration with explicit
checkpoints. First update repository-agent instructions and skills, then move
the Quarto book structure to Rust-first, then rewrite content by chapter group,
then remove active Julia-first material and verify the rendered book.

**Tech Stack:** Quarto book (`.qmd`), Markdown, Rust/Cargo examples, local
repository skills under `.agents/skills/`, `tenferro` Git dependency,
`ndarray`, `ndarray-linalg`.

---

## Source Spec

Implement the approved design in:

```text
docs/superpowers/specs/2026-06-05-rust-first-scientific-coding-design.md
```

Do not edit files under `backups/`.

Do not touch the existing untracked handoff/review files unless the user asks:

```text
agentic_coding_tutorial_handoff.md
agentic_scientific_coding_review.md
scientific_agentic_coding_handoff.md
```

## Target File Structure

### Repository Instructions and Skills

Modify:

```text
AGENTS.md
README.md
book/index.qmd
book/_quarto.yml
```

Create:

```text
.agents/skills/review-rust-code/SKILL.md
```

Remove:

```text
.agents/skills/review-julia-exercise/
```

### Getting Started

Keep:

```text
book/getting-started/download-the-material.qmd
book/getting-started/using-an-ai-agent.qmd
book/getting-started/minimal-git-diff-safety.qmd
book/getting-started/basic-workflow.qmd
book/getting-started/what-not-to-do.qmd
```

Replace:

```text
book/getting-started/default-language-julia.qmd -> book/getting-started/default-language-rust.qmd
book/getting-started/installing-julia.qmd -> book/getting-started/installing-rust.qmd
book/getting-started/running-julia.qmd -> book/getting-started/running-rust.qmd
```

### Rust Basics

Create:

```text
book/rust-basics/variables-and-state.qmd
book/rust-basics/types-and-numeric-values.qmd
book/rust-basics/conditionals.qmd
book/rust-basics/loops.qmd
book/rust-basics/functions.qmd
book/rust-basics/vec-and-slices.qmd
book/rust-basics/ownership-and-borrowing.qmd
book/rust-basics/struct-enum-trait.qmd
book/rust-basics/modules-and-crates.qmd
```

Remove from active book:

```text
book/julia-syntax-basics/
book/types-abstraction-interfaces/
book/rust-practice/
```

### Existing Chapter Groups to Rewrite

Modify:

```text
book/ai-search/asking-useful-questions.qmd
book/ai-search/reading-documentation-and-examples.qmd
book/ai-search/checking-ai-explanations.qmd
book/computer-basics/memory-storage-and-files.qmd
book/computer-basics/indexing-conventions.qmd
book/computer-basics/multidimensional-arrays-and-layout.qmd
book/computer-basics/reshape-view-copy-and-transpose.qmd
book/computer-basics/stack-heap-and-references.qmd
book/computer-basics/processes-and-threads.qmd
book/computer-basics/cost-model-flops-bandwidth-and-cache.qmd
book/correctness-validation/overview.qmd
book/correctness-validation/testing-and-tdd.qmd
book/correctness-validation/edge-cases-and-failure-modes.qmd
book/correctness-validation/reproducibility-as-validation.qmd
book/algorithmic-thinking/complexity.qmd
book/algorithmic-thinking/search-and-sort.qmd
book/algorithmic-thinking/matrix-multiplication-performance.qmd
book/scientific-computing/numerical-integration.qmd
book/scientific-computing/root-finding.qmd
book/scientific-computing/linear-algebra-computations.qmd
book/scientific-computing/monte-carlo-and-randomness.qmd
book/reproducible-workflow/git-basics-for-scientific-workflow.qmd
book/reproducible-workflow/project-structure-and-reproducibility.qmd
book/reproducible-workflow/ai-agent-workflow.qmd
book/project-exercises/overview.qmd
book/project-exercises/exercise-format.qmd
```

Create:

```text
book/scientific-computing/tenferro-typed-tensor.qmd
book/scientific-computing/rust-array-ecosystem.qmd
book/advanced-scientific-topics/automatic-differentiation.qmd
```

## Global Content Rules

Apply these rules to all active material outside `backups/`:

- Rust is the default language for examples and exercise checks.
- Use `&[f64]` and `&mut [f64]` for 1D numerical function boundaries.
- Use tenferro typed tensors for 2D and higher numerical arrays. The current
  concrete path is `tenferro_tensor::TypedTensor` from `tenferro-tensor`.
- Keep `ndarray` and `ndarray-linalg` as alternatives, not the course default.
- Do not present `Vec<Vec<f64>>` as a normal numerical-array representation.
- Mention nested vectors only as an anti-pattern or contrast example.
- Do not tell students to install Julia, run Julia, use `juliaup`, or write
  `.jl` files.
- Do not use Julia as an active comparison example in the current book. Use
  MATLAB and Fortran when a 1-based or column-major comparison is necessary.
- Do not point active text to `review-julia-exercise`.
- Keep each subsection in the shape `Explanation`, `Things to look up`,
  `Exercise`, and `Notes for the exercise`.
- Add `Optional coding check` only where needed.
- Keep prose lines near 80-100 characters, except syntax-sensitive lines.

## Task 1: Rewrite Repository Agent Instructions and Add Rust Review Skill

**Files:**
- Modify: `AGENTS.md`
- Create: `.agents/skills/review-rust-code/SKILL.md`
- Create: `.agents/skills/review-rust-code/agents/openai.yaml`
- Remove: `.agents/skills/review-julia-exercise/SKILL.md`

- [ ] **Step 1: Inspect current agent instructions and skill**

Run:

```bash
sed -n '1,220p' AGENTS.md
sed -n '1,220p' .agents/skills/review-julia-exercise/SKILL.md
```

Expected: both files describe Julia-first exercise review.

- [ ] **Step 2: Initialize `review-rust-code` using the skill creator script**

Run:

```bash
python3 /Users/hiroshi/.codex/skills/.system/skill-creator/scripts/init_skill.py \
  review-rust-code \
  --path .agents/skills \
  --interface display_name="Review Rust Code" \
  --interface short_description="Review Rust scientific-coding exercise answers for this course." \
  --interface default_prompt="Review my Rust scientific-coding exercise answer using the course rules."
```

Expected: `.agents/skills/review-rust-code/SKILL.md` and
`.agents/skills/review-rust-code/agents/openai.yaml` exist.

- [ ] **Step 3: Replace `review-rust-code` skill body**

Use `apply_patch` to replace `.agents/skills/review-rust-code/SKILL.md` with
frontmatter and body matching this content contract:

```markdown
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
- For 2D and higher numerical data, are tenferro typed tensors used unless the
  exercise explicitly asks for a flat-buffer implementation? In the current
  crate layout, this means `tenferro_tensor::TypedTensor`.
- Are nested vectors such as `Vec<Vec<f64>>` avoided for numerical arrays?
- Are small hand-checkable tests and edge cases included?
- Was `cargo test` run, or is the missing test run clearly reported?
- When performance is discussed, is cache-friendly access considered?
- Are scientific validation issues separated from Rust syntax issues?

## Boundaries

Do not generate a complete replacement solution for the student. Prefer hints,
targeted examples, and test suggestions. Unsafe Rust should be treated as an
advanced topic: require explicit invariants, tests, and a reason it is needed.
```

- [ ] **Step 4: Rewrite `AGENTS.md`**

Use `apply_patch` to replace `AGENTS.md` with Rust-first instructions covering
these exact sections:

```text
# AGENTS.md

Common Working Path:
- Use work/chapter_x.y/.
- Small single-file attempts may use answer.rs.
- Runnable/tested attempts should use Cargo.toml, Cargo.lock, src/lib.rs,
  src/main.rs when needed, and tests/.

Learning Mode:
- Act as a Rust scientific-coding educator.
- Show available repository skills under .agents/skills/.
- Do not generate entire solutions at once.
- Discuss algorithm, inputs, outputs, boundary cases, representation, and tests
  before code.
- Review all exercises for functions/modules, hidden global mutable state,
  ownership/borrowing, slices for 1D, tenferro typed tensors for 2D+, no
  nested vectors, cargo test, and cache-friendly access when performance
  matters.
- Mention review-rust-code as the review skill.

Editing Mode:
- Edit material as a concept map with short explanations and exercises.
- Use Rust by default.
- Use slices for 1D numerical examples.
- Use tenferro typed tensors for 2D+ examples, currently
  `tenferro_tensor::TypedTensor` from `tenferro-tensor`.
- Keep ndarray and ndarray-linalg as alternatives.
- Do not include long installation procedures.
- Prefer official Rust and crate documentation links.
- Do not make notebooks mandatory.
- Keep subsection structure Explanation, Things to look up, Exercise, Notes
  for the exercise.
```

Expected: `AGENTS.md` contains no `Julia`, `juliaup`, `.jl`, `answer.jl`, or
`review-julia-exercise`.

- [ ] **Step 5: Remove old Julia review skill**

Run:

```bash
rm -rf .agents/skills/review-julia-exercise
```

Expected: `find .agents/skills -maxdepth 2 -name SKILL.md -print` lists
`review-rust-code` and `create-more-exercise`, not `review-julia-exercise`.

- [ ] **Step 6: Validate the new skill**

Run:

```bash
python3 /Users/hiroshi/.codex/skills/.system/skill-creator/scripts/quick_validate.py \
  .agents/skills/review-rust-code
```

Expected: validation passes.

- [ ] **Step 7: Search for forbidden agent-instruction references**

Run:

```bash
rg -n "Julia|juliaup|answer\\.jl|\\.jl|review-julia-exercise" AGENTS.md .agents/skills
```

Expected: no matches.

- [ ] **Step 8: Commit Task 1**

Run:

```bash
git add AGENTS.md .agents/skills
git commit -m "Make repository agent guidance Rust-first"
```

Expected: commit succeeds.

## Task 2: Update Top-Level README, Book Introduction, and Quarto Structure

**Files:**
- Modify: `README.md`
- Modify: `book/index.qmd`
- Modify: `book/_quarto.yml`

- [ ] **Step 1: Rewrite `README.md` student setup section**

Use `apply_patch` to replace the Julia install paragraph with:

```markdown
Small examples use Rust by default. Install Rust through `rustup` using the
official instructions:

- https://www.rust-lang.org/tools/install

Confirm the installation with `rustc --version` and `cargo --version`. Jupyter
notebooks are optional; Cargo projects and ordinary `.rs` files are enough for
the basic exercises.
```

Expected: README points students to Rust, not Julia.

- [ ] **Step 2: Rewrite `book/index.qmd`**

Use `apply_patch` to make the introduction state:

```text
- The material is for physics-oriented numerical computing with Rust and AI
  agents.
- It is not a complete Rust textbook.
- Students learn specification, code review, tests, validation, and
  reproducibility.
- Rust and Cargo are used from the beginning.
- 1D data use Vec and slices.
- 2D+ arrays use tenferro typed tensors, currently
  `tenferro_tensor::TypedTensor` from `tenferro-tensor`.
- ndarray and ndarray-linalg are ecosystem alternatives.
- The chapter list has nine sections matching the approved spec.
```

Keep the existing `figures/ai-agent-loop.svg` image reference.

Expected: `book/index.qmd` contains no statement that Julia is used first or by
default.

- [ ] **Step 3: Replace `book/_quarto.yml` render list and chapters**

Use `apply_patch` to rewrite the `project.render` and `book.chapters` lists to
this order:

```yaml
project:
  type: book
  output-dir: dist
  render:
    - index.qmd
    - getting-started/download-the-material.qmd
    - getting-started/using-an-ai-agent.qmd
    - getting-started/minimal-git-diff-safety.qmd
    - getting-started/default-language-rust.qmd
    - getting-started/installing-rust.qmd
    - getting-started/running-rust.qmd
    - getting-started/basic-workflow.qmd
    - getting-started/what-not-to-do.qmd
    - ai-search/asking-useful-questions.qmd
    - ai-search/reading-documentation-and-examples.qmd
    - ai-search/checking-ai-explanations.qmd
    - computer-basics/memory-storage-and-files.qmd
    - computer-basics/indexing-conventions.qmd
    - computer-basics/multidimensional-arrays-and-layout.qmd
    - computer-basics/reshape-view-copy-and-transpose.qmd
    - computer-basics/stack-heap-and-references.qmd
    - computer-basics/processes-and-threads.qmd
    - computer-basics/cost-model-flops-bandwidth-and-cache.qmd
    - rust-basics/variables-and-state.qmd
    - rust-basics/types-and-numeric-values.qmd
    - rust-basics/conditionals.qmd
    - rust-basics/loops.qmd
    - rust-basics/functions.qmd
    - rust-basics/vec-and-slices.qmd
    - rust-basics/ownership-and-borrowing.qmd
    - rust-basics/struct-enum-trait.qmd
    - rust-basics/modules-and-crates.qmd
    - correctness-validation/overview.qmd
    - correctness-validation/testing-and-tdd.qmd
    - correctness-validation/edge-cases-and-failure-modes.qmd
    - correctness-validation/reproducibility-as-validation.qmd
    - algorithmic-thinking/complexity.qmd
    - algorithmic-thinking/search-and-sort.qmd
    - algorithmic-thinking/matrix-multiplication-performance.qmd
    - scientific-computing/tenferro-typed-tensor.qmd
    - scientific-computing/rust-array-ecosystem.qmd
    - scientific-computing/numerical-integration.qmd
    - scientific-computing/root-finding.qmd
    - scientific-computing/linear-algebra-computations.qmd
    - scientific-computing/monte-carlo-and-randomness.qmd
    - advanced-scientific-topics/automatic-differentiation.qmd
    - reproducible-workflow/git-basics-for-scientific-workflow.qmd
    - reproducible-workflow/project-structure-and-reproducibility.qmd
    - reproducible-workflow/ai-agent-workflow.qmd
    - project-exercises/overview.qmd
    - project-exercises/exercise-format.qmd
```

Use these section titles in `book.chapters`:

```text
Section 1: Safe Workflow Before Coding
Section 2: Learning with AI and Web Search
Section 3: Essential Computer Model
Section 4: Rust Basics for Scientific Code
Section 5: Correctness and Validation
Section 6: Algorithmic Thinking
Section 7: Scientific Computing with Rust and tenferro
Section 8: Advanced Scientific Topics
Section 9: Reproducible Scientific Workflow and Projects
```

Expected: `_quarto.yml` no longer references `julia-syntax-basics`,
`types-abstraction-interfaces`, or `rust-practice`.

- [ ] **Step 4: Verify YAML parses**

Run:

```bash
ruby -ryaml -e 'data = YAML.load_file("book/_quarto.yml"); raise "not a book" unless data["project"]["type"] == "book"; raise "missing chapters" unless data["book"]["chapters"]; puts "ok"'
```

Expected: prints `ok`.

- [ ] **Step 5: Commit Task 2**

Run:

```bash
git add README.md book/index.qmd book/_quarto.yml
git commit -m "Move book structure to Rust-first course"
```

Expected: commit succeeds.

## Task 3: Replace Getting-Started Julia Pages with Rust/Cargo Pages

**Files:**
- Create: `book/getting-started/default-language-rust.qmd`
- Create: `book/getting-started/installing-rust.qmd`
- Create: `book/getting-started/running-rust.qmd`
- Modify: `book/getting-started/basic-workflow.qmd`
- Modify: `book/getting-started/using-an-ai-agent.qmd`
- Remove: `book/getting-started/default-language-julia.qmd`
- Remove: `book/getting-started/installing-julia.qmd`
- Remove: `book/getting-started/running-julia.qmd`

- [ ] **Step 1: Rename the three language pages**

Run:

```bash
git mv book/getting-started/default-language-julia.qmd book/getting-started/default-language-rust.qmd
git mv book/getting-started/installing-julia.qmd book/getting-started/installing-rust.qmd
git mv book/getting-started/running-julia.qmd book/getting-started/running-rust.qmd
```

Expected: old filenames no longer exist; new filenames exist.

- [ ] **Step 2: Rewrite `default-language-rust.qmd`**

Use `apply_patch` to set:

```yaml
---
title: "1.4 Default language: Rust"
---
```

Content contract:

```text
Explanation:
- Rust is the default language for examples and exercise checks.
- Cargo gives a standard build/test workflow.
- Rust compiler errors are useful feedback when working with AI agents.
- 1D data use Vec and slices.
- 2D+ arrays use tenferro typed tensors, currently
  `tenferro_tensor::TypedTensor` from `tenferro-tensor`.
- The goal is scientific workflow, not memorizing all Rust syntax.

Things to look up:
- Rust programming language
- Cargo
- rustup
- Ownership and borrowing
- Slice
- Crate

Exercise:
- Write a short paragraph explaining why a scientific exercise can start in
  Rust with AI-agent help.

Notes:
- Mention tests, compiler feedback, and reproducibility.
- Do not argue that Rust is always the best language.
```

- [ ] **Step 3: Rewrite `installing-rust.qmd`**

Use `apply_patch` to set:

```yaml
---
title: "1.5 Installing Rust"
---
```

Content contract:

```text
Explanation:
- Install Rust with rustup unless there is a local reason not to.
- Use official Rust installation instructions.
- Confirm both rustc and cargo are available.

Things to look up:
- Rust installation
- rustup
- rustc
- cargo

Exercise:
- Install Rust or confirm it is installed.
- Record OS, install route, rustc --version, cargo --version.

Notes:
- Confirm by running commands.
- Do not rely on old blog posts.
```

- [ ] **Step 4: Rewrite `running-rust.qmd`**

Use `apply_patch` to set:

```yaml
---
title: "1.6 Running Rust"
---
```

Content contract:

```text
Explanation:
- For reproducible checks, prefer a tiny Cargo project.
- cargo run executes the program.
- cargo test runs tests.
- Single .rs files can be useful for tiny experiments, but exercises with tests
  should use Cargo.

Things to look up:
- cargo new
- cargo run
- cargo test
- src/main.rs
- src/lib.rs
- tests directory

Exercise:
- Create work/chapter_1.6/hello_rust with cargo new.
- Run cargo run.
- Add one simple test and run cargo test.

Notes:
- Keep generated files small.
- Commit Cargo.lock for exercise projects that add dependencies.
```

- [ ] **Step 5: Rewrite `basic-workflow.qmd`**

Use `apply_patch` to replace Julia workflow with:

```text
- read the page,
- write a small plan,
- create work/chapter_x.y/,
- use answer.rs for tiny scratch work or Cargo for tested work,
- write source in src/lib.rs for reusable functions,
- write tests in tests/ or #[cfg(test)] modules,
- run cargo test,
- inspect git diff before asking an AI agent to continue.
```

Include one command block:

```bash
mkdir -p work/chapter_4.6
cd work/chapter_4.6
cargo new slice_check
cd slice_check
cargo test
```

Expected: no Julia command remains.

- [ ] **Step 6: Update `using-an-ai-agent.qmd` skill references**

Use `apply_patch` to replace the Julia review skill example with
`review-rust-code`:

```text
For example, the Rust code review skill is in
.agents/skills/review-rust-code/SKILL.md.
```

Expected: active text points to `review-rust-code`.

- [ ] **Step 7: Search getting-started pages**

Run:

```bash
rg -n "Julia|julia|juliaup|\\.jl|answer\\.jl|review-julia-exercise" book/getting-started
```

Expected: no matches.

- [ ] **Step 8: Commit Task 3**

Run:

```bash
git add book/getting-started
git commit -m "Rewrite getting started pages for Rust"
```

Expected: commit succeeds.

## Task 4: Create Rust Basics Chapter and Remove Julia Syntax Chapter

**Files:**
- Create all files under `book/rust-basics/`
- Remove all active files under `book/julia-syntax-basics/`
- Remove active files under `book/types-abstraction-interfaces/`
- Use source ideas from `book/rust-practice/` before removing it in Task 9

- [ ] **Step 1: Create directory**

Run:

```bash
mkdir -p book/rust-basics
```

Expected: directory exists.

- [ ] **Step 2: Write `variables-and-state.qmd`**

Content contract:

```text
Title: "4.1 Variables and state"
Explanation:
- let binding,
- mut for mutation,
- values have owners,
- mutation is explicit,
- avoid hidden global mutable state.
Code:
- fn main() { let x = 2; let mut y = 3; y += x; println!("{y}"); }
Exercise:
- Predict values before running.
- Ask an AI agent to explain why mut is required.
Notes:
- Separate name binding from object mutation.
```

- [ ] **Step 3: Write `types-and-numeric-values.qmd`**

Content contract:

```text
Title: "4.2 Types and numeric values"
Explanation:
- i32, usize, f64, bool, String,
- integer and floating-point literals differ,
- scientific code should choose numeric types deliberately.
Code:
- let n: usize = 10;
- let dx: f64 = 0.1;
- let ok: bool = n > 0;
Exercise:
- Predict types for small expressions.
Notes:
- Indices are often usize.
- Physical quantities are often f64 in this course.
```

- [ ] **Step 4: Write `conditionals.qmd`**

Content contract:

```text
Title: "4.3 Conditionals"
Explanation:
- if / else,
- comparisons,
- branch conditions for numerical algorithms.
Code:
- fn sign_label(x: f64) -> &'static str { if x < 0.0 { "negative" } else if x > 0.0 { "positive" } else { "zero" } }
Exercise:
- Test x = -1.0, 0.0, 1.0.
Notes:
- Boundary case x = 0.0 must be handled deliberately.
```

- [ ] **Step 5: Write `loops.qmd`**

Content contract:

```text
Title: "4.4 Loops"
Explanation:
- for loops over ranges,
- iterating over slices,
- 0-based indexing.
Code:
- for i in 0..xs.len() { ... }
- for x in xs { ... }
Exercise:
- Sum a slice by hand and with a function.
Notes:
- Prefer iterator forms when indices are not needed.
- Use indices when the algorithm depends on positions.
```

- [ ] **Step 6: Write `functions.qmd`**

Content contract:

```text
Title: "4.5 Functions"
Explanation:
- functions define input/output contracts,
- small functions are easier to test,
- avoid reading hidden global state.
Code:
- fn square(x: f64) -> f64 { x * x }
- fn sum(xs: &[f64]) -> f64 { xs.iter().sum() }
Exercise:
- Implement mean(xs: &[f64]) -> Option<f64>.
Notes:
- Empty slice is a boundary case.
```

- [ ] **Step 7: Write `vec-and-slices.qmd`**

Content contract:

```text
Title: "4.6 Vec and slices"
Explanation:
- Vec owns a contiguous 1D buffer,
- &[T] borrows read-only data,
- &mut [T] borrows mutable data,
- this is the standard 1D course interface.
Code:
- fn mean(xs: &[f64]) -> Option<f64>
- fn scale_in_place(xs: &mut [f64], alpha: f64)
Exercise:
- Create tests for mean and scale_in_place.
Notes:
- Do not take &Vec<f64> when &[f64] is enough.
```

- [ ] **Step 8: Write `ownership-and-borrowing.qmd`**

Content contract:

```text
Title: "4.7 Ownership and borrowing"
Explanation:
- one owner,
- many immutable borrows or one mutable borrow,
- function signatures show whether data is read or changed.
Code:
- fn norm2(xs: &[f64]) -> f64
- fn normalize(xs: &mut [f64])
Exercise:
- Ask an AI agent why two simultaneous mutable borrows are rejected.
Notes:
- Rust does not forbid all aliasing; it controls mutable aliasing.
```

- [ ] **Step 9: Write `struct-enum-trait.qmd`**

Content contract:

```text
Title: "4.8 Struct, enum, and trait"
Explanation:
- struct groups data with names,
- enum represents alternatives,
- trait describes shared behavior,
- use these for scientific interfaces.
Code:
- struct Grid1D { n: usize, dx: f64 }
- enum Boundary { Open, Periodic }
- trait Energy { fn energy(&self) -> f64; }
Exercise:
- Design a small struct for a 1D grid.
Notes:
- Keep fields meaningful and units clear.
```

- [ ] **Step 10: Write `modules-and-crates.qmd`**

Content contract:

```text
Title: "4.9 Modules and crates"
Explanation:
- src/lib.rs exposes reusable functions,
- src/main.rs is for command-line entry points,
- tests can live in #[cfg(test)] modules or tests/.
Code:
- minimal module layout for src/lib.rs and tests/basic.rs.
Exercise:
- Ask an AI agent to create a tiny Cargo library and one integration test.
Notes:
- Keep runtime code and test code separated.
```

- [ ] **Step 11: Remove old Julia and type-interface directories**

Run:

```bash
rm -rf book/julia-syntax-basics book/types-abstraction-interfaces
```

Expected: directories are gone.

- [ ] **Step 12: Search Rust basics**

Run:

```bash
rg -n "Julia|julia|Vec<Vec|review-julia-exercise" book/rust-basics
```

Expected: no matches. A future intentional `Vec<Vec` anti-pattern mention
belongs in array/scientific pages, not Rust basics.

- [ ] **Step 13: Commit Task 4**

Run:

```bash
git add book/rust-basics book/julia-syntax-basics book/types-abstraction-interfaces
git commit -m "Replace Julia syntax chapters with Rust basics"
```

Expected: commit succeeds.

## Task 5: Rewrite Computer Basics, AI Search, and Validation for Rust

**Files:**
- Modify all files listed in "Existing Chapter Groups to Rewrite" for
  `book/ai-search/`, `book/computer-basics/`, and `book/correctness-validation/`

- [ ] **Step 1: Update AI search pages**

Use `apply_patch` so:

```text
reading-documentation-and-examples.qmd:
- uses official Rust docs, docs.rs, crate README, GitHub issues as source types,
- uses tenferro, ndarray, or rand examples instead of Julia functions.

asking-useful-questions.qmd:
- asks for Rust function signatures, Cargo setup, tests, and validation checks.

checking-ai-explanations.qmd:
- tells students to verify generated Rust with cargo test and source docs.
```

Expected: AI-search section no longer uses Julia as the example ecosystem.

- [ ] **Step 2: Update computer basics pages**

Use `apply_patch` so:

```text
memory-storage-and-files.qmd:
- examples are Rust variables, Cargo files, and result files.

indexing-conventions.qmd:
- Rust, C, C++, and Python are 0-based.
- MATLAB and Fortran may be mentioned as 1-based or column-major comparison
  languages. Do not mention Julia in active book text.
- exercises ask how to refer to first element in Rust.

multidimensional-arrays-and-layout.qmd:
- distinguish shape, stride, memory layout, and tensor metadata.
- Rust standard path is tenferro typed tensors for 2D+, currently
  `tenferro_tensor::TypedTensor` from `tenferro-tensor`.
- nested vectors are identified as a poor numerical-array default.

reshape-view-copy-and-transpose.qmd:
- connects reshape/view/copy to tensor metadata and owned buffers.

stack-heap-and-references.qmd:
- uses Rust ownership, stack values, heap Vec buffer, references.

processes-and-threads.qmd:
- uses cargo commands and numerical jobs as examples.

cost-model-flops-bandwidth-and-cache.qmd:
- emphasizes cache-friendly access, contiguous buffers, BLAS/LAPACK ecosystem,
  tenferro, ndarray-linalg, and performance measurement.
```

- [ ] **Step 3: Update correctness and validation pages**

Use `apply_patch` so:

```text
overview.qmd:
- validation loop is Rust/Cargo-first.

testing-and-tdd.qmd:
- example project uses src/lib.rs and tests/mean.rs.
- command is cargo test.
- mean_value uses &[f64] and returns Option<f64> for empty data.

edge-cases-and-failure-modes.qmd:
- examples use Rust functions and `Option` or `Result` for invalid inputs and
  recoverable failures.

reproducibility-as-validation.qmd:
- metadata includes rustc version, cargo version, command, git commit, and
  Cargo.lock status.
```

- [ ] **Step 4: Search these chapter groups**

Run:

```bash
rg -n "Julia|julia|juliaup|\\.jl|answer\\.jl|review-julia-exercise" \
  book/ai-search book/computer-basics book/correctness-validation
```

Expected: no active Julia-first references. Comparison-only mentions of
`Julia` in indexing/layout are allowed only when they do not make Julia the
course default.

- [ ] **Step 5: Commit Task 5**

Run:

```bash
git add book/ai-search book/computer-basics book/correctness-validation
git commit -m "Rewrite core concepts and validation for Rust"
```

Expected: commit succeeds.

## Task 6: Rewrite Algorithmic Thinking and Expand GEMM Performance Case Study

**Files:**
- Modify: `book/algorithmic-thinking/complexity.qmd`
- Modify: `book/algorithmic-thinking/search-and-sort.qmd`
- Modify: `book/algorithmic-thinking/matrix-multiplication-performance.qmd`

- [ ] **Step 1: Rewrite complexity page**

Content contract:

```text
Explanation:
- Big-O describes growth.
- Rust examples use loops over slices.
- Allocation and memory movement matter.
Exercise:
- Count operations for sum, pairwise comparison, and nested loops.
Notes:
- Big-O does not replace measurement.
```

- [ ] **Step 2: Rewrite search and sort page**

Content contract:

```text
Explanation:
- search over &[f64] or &[i32],
- sorting Vec<T> with sort_by for floats when needed,
- tests compare against known small examples.
Exercise:
- Write my_find and test missing, first, last, repeated cases.
Notes:
- Do not use standard sort inside a custom-sort exercise except in tests.
```

- [ ] **Step 3: Rewrite matrix multiplication page**

Content contract:

```text
Explanation:
- GEMM computes C = A B.
- State shape convention explicitly.
- Use flat Vec<f64> for hand-written low-level GEMM.
- Use tenferro typed tensors for the library/tensor comparison path.
- Verify current `tenferro-rs` crate names, import paths, and resolved Git
  revision before using generated tensor code.
- Explain cache-friendly access, contiguous memory, row/column layout, blocking,
  vectorization, bounds-check elimination, FLOPs, and memory bandwidth.
- State that safe indexing has bounds checks unless the compiler proves access
  is in range.
- Recommend up-front shape checks and row slices before inner loops.

Exercise:
- Ask an AI agent to create a Cargo project in work/chapter_6.3/.
- Generate simple safe Rust GEMM.
- Test small matrices by hand.
- Benchmark a controlled run.
- Compare with a tenferro-based calculation.
- Ask the agent to optimize the hand-written implementation.
- Re-run correctness tests after optimization.
- Explain memory access pattern and whether optimization preserved the
  scientific contract.

Notes:
- Unsafe indexing is not part of the main route.
- If unsafe appears, require invariants and tests.
- Performance claims need measurements.
```

- [ ] **Step 4: Search algorithmic thinking**

Run:

```bash
rg -n "Julia|julia|BenchmarkTools|\\.jl|Vec<Vec|review-julia-exercise" \
  book/algorithmic-thinking
```

Expected: no matches for Julia-first material. `Vec<Vec` should not appear.

- [ ] **Step 5: Commit Task 6**

Run:

```bash
git add book/algorithmic-thinking
git commit -m "Expand Rust algorithmic performance material"
```

Expected: commit succeeds.

## Task 7: Rewrite Scientific Computing Around tenferro

**Files:**
- Create: `book/scientific-computing/tenferro-typed-tensor.qmd`
- Create: `book/scientific-computing/rust-array-ecosystem.qmd`
- Modify: `book/scientific-computing/numerical-integration.qmd`
- Modify: `book/scientific-computing/root-finding.qmd`
- Modify: `book/scientific-computing/linear-algebra-computations.qmd`
- Modify: `book/scientific-computing/monte-carlo-and-randomness.qmd`

- [ ] **Step 1: Write `tenferro-typed-tensor.qmd`**

Content contract:

```text
Title: "7.1 Typed tensors with tenferro"
Explanation:
- tenferro typed tensors are the standard course path for 2D+ arrays.
- In the current crate layout, use `tenferro_tensor::TypedTensor` from
  `tenferro-tensor`.
- A tensor carries data plus shape/axis meaning.
- Shape validation is part of correctness.
- Cargo dependency uses:
  tenferro-tensor = { git = "https://github.com/tensor4all/tenferro-rs", branch = "main" }
- Commit Cargo.lock in complete exercise projects.
- There is currently no all-in-one `tenferro` facade crate.
Things to look up:
- tenferro
- tenferro-tensor
- TypedTensor
- tensor shape
- tensor axis
Exercise:
- Ask an AI agent to inspect current tenferro docs and create a tiny example
  that constructs a 2D typed tensor.
Notes:
- API may evolve because tenferro is under development.
- Verify examples against the current repository before trusting generated code.
```

- [ ] **Step 2: Write `rust-array-ecosystem.qmd`**

Content contract:

```text
Title: "7.2 Rust array ecosystem"
Explanation:
- Course default: slices for 1D, tenferro typed tensors for 2D+.
- In the current crate layout, use `tenferro_tensor::TypedTensor` from
  `tenferro-tensor`.
- ndarray is an important Rust N-dimensional array library.
- ndarray-linalg connects ndarray-style arrays to BLAS/LAPACK routines.
- Choosing a library requires checking API, maintenance, dependencies, and
  validation needs.
- Nested vectors are not the normal scientific-array representation.
Things to look up:
- ndarray
- ndarray-linalg
- BLAS
- LAPACK
- contiguous memory
Exercise:
- Compare tenferro-tensor and ndarray documentation for one basic array
  operation.
- Inspect ndarray-linalg documentation for one BLAS/LAPACK-style operation.
Notes:
- Do not switch libraries only because an AI agent suggests one.
```

- [ ] **Step 3: Rewrite numerical integration**

Content contract:

```text
Title keeps numerical integration.
Example uses:
- fn trapezoid<F: Fn(f64) -> f64>(f: F, a: f64, b: f64, n: usize) -> Option<f64>
Exercise:
- implement trapezoidal rule in Rust,
- test constant and linear functions,
- check convergence for x*x.
Notes:
- n = 0 is invalid.
```

- [ ] **Step 4: Rewrite root finding**

Content contract:

```text
Example uses:
- bisection with Fn(f64) -> f64,
- Result<f64, String> or Option<f64> for invalid bracket.
Exercise:
- implement bisection with tests for a valid bracket and invalid bracket.
Notes:
- Check sign change before iterations.
```

- [ ] **Step 5: Rewrite linear algebra computations**

Content contract:

```text
Explanation:
- 1D vector operations use slices.
- 2D matrix/tensor examples use tenferro typed tensors, currently
  `tenferro_tensor::TypedTensor` from `tenferro-tensor`.
- ndarray and ndarray-linalg are alternatives for established BLAS/LAPACK
  workflows.
Exercise:
- use a small matrix-vector or matrix-matrix example with explicit shape checks.
Notes:
- Do not use Vec<Vec<f64>> as the matrix representation.
```

- [ ] **Step 6: Rewrite Monte Carlo and randomness**

Content contract:

```text
Explanation:
- use a Rust RNG crate such as rand,
- seed policy matters,
- report uncertainty instead of trusting one seed.
Exercise:
- estimate pi in Rust,
- run two sample sizes,
- record seed and command.
Notes:
- Multiple seeds or uncertainty estimates matter for scientific claims.
```

- [ ] **Step 7: Search scientific-computing pages**

Run:

```bash
rg -n "Julia|julia|\\.jl|Vec<Vec|review-julia-exercise" book/scientific-computing
```

Expected: no matches except a deliberate comparison-only mention of Julia in
the ecosystem page if kept. `Vec<Vec` should not appear.

- [ ] **Step 8: Commit Task 7**

Run:

```bash
git add book/scientific-computing
git commit -m "Center scientific computing chapters on tenferro"
```

Expected: commit succeeds.

## Task 8: Add Advanced Topic Page and Rewrite Reproducible Workflow/Projects

**Files:**
- Create: `book/advanced-scientific-topics/automatic-differentiation.qmd`
- Modify: `book/reproducible-workflow/git-basics-for-scientific-workflow.qmd`
- Modify: `book/reproducible-workflow/project-structure-and-reproducibility.qmd`
- Modify: `book/reproducible-workflow/ai-agent-workflow.qmd`
- Modify: `book/project-exercises/overview.qmd`
- Modify: `book/project-exercises/exercise-format.qmd`

- [ ] **Step 1: Create advanced topic directory**

Run:

```bash
mkdir -p book/advanced-scientific-topics
```

Expected: directory exists.

- [ ] **Step 2: Write automatic differentiation page**

Content contract:

```text
Title: "8.1 Automatic differentiation"
Explanation:
- AD computes derivatives of programs.
- Gradients matter in optimization, fitting, and sensitivity analysis.
- AI-generated derivative code must be checked.
- Future Rust ecosystem material may connect to tenferro-related work,
  tidu-rs, and chainrules-rs.
Things to look up:
- automatic differentiation
- forward mode
- reverse mode
- gradient check
Exercise:
- Ask an AI agent to explain finite-difference checking of a derivative.
Notes:
- This page is a short orientation, not a full AD tutorial.
```

- [ ] **Step 3: Rewrite reproducible workflow pages**

Use `apply_patch` so:

```text
git-basics-for-scientific-workflow.qmd:
- keeps git/diff concepts language-neutral with Rust examples.

project-structure-and-reproducibility.qmd:
- uses Cargo.toml and Cargo.lock as the primary environment files.
- explains src/lib.rs, src/main.rs, tests/, data/, results/.
- shows tenferro Git dependency syntax.

ai-agent-workflow.qmd:
- prompts ask for Rust/Cargo code review.
- prompts mention slices, TypedTensor, cargo test, and validation.
```

- [ ] **Step 4: Rewrite project exercises pages**

Use `apply_patch` so:

```text
overview.qmd:
- default project route is Rust/Cargo.
- projects should make environment explicit with Cargo.toml and Cargo.lock.
- existing Python project READMEs are historical or transitional until rewritten.

exercise-format.qmd:
- exercise expectations mention Rust tests, metadata, Cargo lock files,
  validation checks, and tenferro for 2D+ arrays.
```

- [ ] **Step 5: Search workflow and project pages**

Run:

```bash
rg -n "Julia|julia|juliaup|\\.jl|answer\\.jl|review-julia-exercise" \
  book/reproducible-workflow book/project-exercises book/advanced-scientific-topics
```

Expected: no matches unless a project page explicitly says an older exercise is
transitional.

- [ ] **Step 6: Commit Task 8**

Run:

```bash
git add book/advanced-scientific-topics book/reproducible-workflow book/project-exercises
git commit -m "Update reproducible workflow for Rust projects"
```

Expected: commit succeeds.

## Task 9: Remove Absorbed Rust Practice Directory and Verify File References

**Files:**
- Remove: `book/rust-practice/`
- Modify: active files reported by the stale-link search

- [ ] **Step 1: Search for links to removed directories**

Run:

```bash
rg -n "julia-syntax-basics|types-abstraction-interfaces|rust-practice|default-language-julia|installing-julia|running-julia" \
  book README.md AGENTS.md CLAUDE.md .agents/skills exercises
```

Expected: report any stale links that must be updated before deletion.

- [ ] **Step 2: Update stale links**

Use `apply_patch` to replace stale links with the new Rust pages:

```text
default-language-julia.qmd -> default-language-rust.qmd
installing-julia.qmd -> installing-rust.qmd
running-julia.qmd -> running-rust.qmd
julia-syntax-basics/... -> rust-basics/...
types-abstraction-interfaces/... -> rust-basics/struct-enum-trait.qmd or rust-basics/modules-and-crates.qmd
rust-practice/rust-array-libraries.qmd -> scientific-computing/rust-array-ecosystem.qmd
```

Expected: the search in Step 1 returns no active stale links.

- [ ] **Step 3: Remove absorbed Rust practice directory**

Run:

```bash
rm -rf book/rust-practice
```

Expected: directory is gone.

- [ ] **Step 4: Verify Quarto references point to existing files**

Run:

```bash
ruby -ryaml -e 'root = "book"; data = YAML.load_file("#{root}/_quarto.yml"); missing = data["project"]["render"].reject { |p| File.exist?(File.join(root, p)) }; raise "missing render files: #{missing.join(", ")}" unless missing.empty?; puts "all render files exist"'
```

Expected: prints `all render files exist`.

- [ ] **Step 5: Commit Task 9**

Run:

```bash
git add book README.md AGENTS.md CLAUDE.md .agents/skills exercises
git commit -m "Remove absorbed legacy book sections"
```

Expected: commit succeeds, or reports nothing to commit if all removals were
already committed in earlier tasks.

## Task 10: Full Verification and Final Cleanup

**Files:**
- Modify only files needed to fix verification failures.

- [ ] **Step 1: Run active-material Julia scan**

Run:

```bash
rg -n "Julia|julia|juliaup|\\.jl|answer\\.jl|review-julia-exercise|BenchmarkTools" \
  README.md AGENTS.md CLAUDE.md .agents book exercises docs \
  --glob '!backups/**' \
  --glob '!docs/superpowers/specs/2026-06-05-rust-first-scientific-coding-design.md' \
  --glob '!docs/superpowers/plans/2026-06-05-rust-first-scientific-coding.md'
```

Expected: no active Julia-first matches. Allowed matches are historical design
documents only:

```text
docs/superpowers/specs/2026-06-01-ising-mcmc-exercise-design.md
docs/superpowers/plans/2026-06-01-ising-mcmc-exercise-materials.md
```

Do not leave Julia matches in active book pages, repository instructions,
skills, README files, or exercise READMEs.

- [ ] **Step 2: Run nested-vector scan**

Run:

```bash
rg -n "Vec<Vec" README.md AGENTS.md .agents book exercises docs --glob '!backups/**'
```

Expected: matches only where nested vectors are rejected as an anti-pattern.

- [ ] **Step 3: Run tenferro policy scan**

Run:

```bash
rg -n "tenferro|TypedTensor|ndarray|ndarray-linalg" README.md AGENTS.md book .agents/skills --glob '!backups/**'
```

Expected: matches show tenferro as the standard 2D+ path and ndarray/
ndarray-linalg as alternatives.

- [ ] **Step 4: Validate skill again**

Run:

```bash
python3 /Users/hiroshi/.codex/skills/.system/skill-creator/scripts/quick_validate.py \
  .agents/skills/review-rust-code
```

Expected: validation passes.

- [ ] **Step 5: Build the Quarto book**

Run:

```bash
make build
```

Expected: Quarto render completes and `book/dist/index.html` exists.

- [ ] **Step 6: Inspect git status**

Run:

```bash
git status --short
```

Expected: only intentional changes remain. The pre-existing untracked handoff
files may still be listed and should not be committed unless the user asks.

- [ ] **Step 7: Commit verification fixes**

If Steps 1-5 required fixes, commit them:

```bash
git add README.md AGENTS.md .agents book exercises docs
git commit -m "Fix Rust-first rewrite verification issues"
```

Expected: commit succeeds if fixes were made. If no fixes were made, there is
nothing to commit.

## Final Acceptance Criteria

The rewrite is ready for user review when:

- `make build` succeeds,
- `book/_quarto.yml` references only existing files,
- active course material no longer presents Julia as the default language,
- active course material no longer tells students to install Julia or run `.jl`
  files,
- `review-rust-code` exists and validates,
- `review-julia-exercise` is removed from active repository skills,
- 1D numerical examples use slices,
- 2D and higher numerical examples use tenferro typed tensors, currently
  `tenferro_tensor::TypedTensor` from `tenferro-tensor`,
- `ndarray` and `ndarray-linalg` remain as alternatives,
- nested vectors are explicitly rejected as normal numerical arrays,
- the GEMM page covers cache-friendly access, controlled benchmarks, shape
  checks, safe bounds-check discussion, and optimization validation.
