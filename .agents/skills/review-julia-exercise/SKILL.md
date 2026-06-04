---
name: review-julia-exercise
description: Review a student's Julia exercise answer for this course. Use when asked to review answer.jl or a Julia/scientific-coding exercise solution. Follow AGENTS.md, act as an educator, and do not generate the full solution.
---

# Review Julia Exercise

Use this skill to review a student's Julia exercise answer in this repository.
The goal is educational feedback, not replacing the student's work.

## Inputs

- Path to `answer.jl` or an exercise work directory.
- Optional path to the relevant exercise page or handout.
- Whether this is review-only or editing is allowed. Default to review-only.

## Workflow

1. Read `AGENTS.md` and follow Learning Mode.
2. Read the relevant exercise text and notes if the path or section is clear.
3. Inspect the submitted file. If no path is given, ask for the path.
4. If it is safe and Julia is available, run the answer from its directory:

   ```bash
   julia --project=@. answer.jl
   ```

   Do not install packages unless the exercise needs them and the user permits
   it. If required packages are missing, report that instead of guessing.

5. Review the answer for:

   - alignment with the exercise,
   - inputs, outputs, and assumptions,
   - boundary cases and failure modes,
   - tests or validation,
   - use of functions or other structured units,
   - avoidance of unnecessary global state inside functions,
   - Julia clarity and idioms,
   - scientific-computing correctness and reproducibility.

6. Report findings first. Use file and line references when possible.
7. Give concise improvement suggestions. Do not write the full solution.
8. If editing is explicitly allowed, make only minimal changes and explain them.

