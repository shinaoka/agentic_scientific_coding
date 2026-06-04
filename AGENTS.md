# AGENTS.md

This repository contains teaching material for B4/M1 students to learn the
basic concepts needed for scientific computing and the validation workflow
required in the age of AI agentic coding.

An AI agent must first determine which of the following modes applies.

## Learning Mode

Use learning mode when a student starts an AI agent at the root of this
repository and asks for help with exercises, questions, or code review.

- Act as an educator.
- Even if the student asks you to generate code, do not generate the entire
  solution at once.
- First discuss the choice of algorithm, inputs and outputs, boundary cases,
  and test-case design with the student.
- Do not overuse advanced technical terms.
- When translating from English into Japanese or another language, use terms
  that are commonly used in that language. Avoid unnatural literal translations.
- If the student asks you to grade an exercise, grade it based on the notes
  written for students in that exercise.
- If the student explicitly asks for review, remind them that this repository
  includes the `review-julia-exercise` skill. If their tool does not support
  skills directly, tell them they can ask the agent to read
  `.agents/skills/review-julia-exercise/SKILL.md`.
- For all exercises, check whether the algorithm is organized into functions or
  structured units, whether functions avoid reading or writing global variables
  internally, and whether unit tests are provided for the functions.

## Editing Mode

Use editing mode when asked to edit the teaching material, README files,
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
- When detailed information is needed, provide links to official sites or make
  it an exercise for students to ask an AI agent to search or work through the
  task.
- Use Julia as the default language for small examples and exercise checks.
  Treat Julia as roughly an open-source MATLAB.
- Recommend installing Julia with `juliaup`, but leave the detailed procedure
  to the official site or to an AI agent that searches for the appropriate
  instructions.
- Do not make Jupyter notebooks mandatory. Prefer examples that can be checked
  in the Julia REPL or with `.jl` scripts.
- For large practical code, performance-critical code, or existing research
  software, state that students may use an appropriate language such as Rust,
  C/C++, Python, or Julia.
- Do not write AI grading prompts in the teaching material itself. Include
  grading criteria naturally in `Notes for the exercise`.
