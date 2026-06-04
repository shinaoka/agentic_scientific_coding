# Agentic Scientific Coding

This repository contains teaching material for B4/M1 students who are beginning to use programming and AI agents for scientific computing.

The goal is not to copy code produced by an AI agent. The goal is to learn how to describe a computational problem, inspect code, design tests, validate numerical results, and keep calculations reproducible.

This material is intentionally not a comprehensive textbook. Each subsection gives a short explanation, a list of things to look up, an exercise, and notes for the exercise. Details should be learned by reading official documentation, using web search, and asking an AI agent focused questions.

## Section 0: How to Use This Material

### 0.1 Repository setup

#### Explanation

Use this material as a repository, not as isolated copied files. Download or clone the whole repository, then work from the repository root.

If you use `git clone`, the workflow is:

```bash
git clone <repository-url>
cd agentic_scientific_coding
```

If you download a ZIP file, unpack it and move to the top-level folder before working.

#### Things to look up

- Git clone
- Repository root
- Current working directory
- Command line basics for your OS

#### Exercise

Find the repository root on your machine. Write down:

- the path to the repository root,
- how you opened a terminal there,
- which files or folders you see at the top level.

#### Notes for the exercise

- Do not work from a random subdirectory unless the exercise explicitly says so.
- Be able to explain what "repository root" means.
- If you use an AI agent, tell it the repository root and ask it to inspect the current files before editing anything.

### 0.2 Using an AI agent

#### Explanation

If you use an AI agent, start it from the repository root. This lets the agent read the teaching material, exercise files, work logs, and project structure as one workspace.

For example:

```bash
cd agentic_scientific_coding
codex
```

The exact command depends on the AI agent you use. The important point is that the workspace should be this repository.

#### Things to look up

- AI coding agent
- Workspace
- AGENTS.md
- Diff review

#### Exercise

Write three prompts you could give an AI agent before asking it to write code. Each prompt must ask about one of the following:

- inputs and outputs,
- boundary cases,
- tests or validation,
- possible failure modes.

#### Notes for the exercise

- Do not ask only "Is this correct?"
- Ask for specific checks.
- Ask the agent to explain assumptions.
- Keep responsibility for the final judgment yourself.

### 0.3 Default language: Julia

#### Explanation

Small examples and exercise checks in this material use Julia by default. Here Julia is treated roughly as an open-source MATLAB: it is convenient for arrays, functions, linear algebra, and numerical experiments.

The goal is not to learn every part of Julia. The goal is to learn concepts that transfer across scientific programming: specification, input, output, mutation, side effects, edge cases, complexity, validation, and reproducibility.

For large practical code, performance-critical code, or existing research software, use a language appropriate for the task. Rust, C/C++, Python, and Julia can all be reasonable choices depending on the project.

#### Things to look up

- Julia programming language
- MATLAB and Julia comparison
- Scientific computing language
- Array programming

#### Exercise

Write a short paragraph explaining why a simple numerical exercise might use Julia, while a larger research code might use another language.

#### Notes for the exercise

- Do not argue that one language is always best.
- Separate language syntax from scientific workflow.
- Mention at least two criteria, such as readability, libraries, performance, reliability, or existing code.

### 0.4 Installing Julia

#### Explanation

Install Julia with `juliaup` unless you have a good reason to use another route. Do not rely on old blog posts for installation details. Use the official Julia installation page or ask an AI agent to search for the current instructions for your OS.

Official site:

- https://julialang.org/install/

Julia documentation:

- https://docs.julialang.org/

#### Things to look up

- juliaup
- Julia official installation
- Julia version manager
- `julia --version`

#### Exercise

Install Julia or confirm that Julia is already installed. Record:

- how you found the installation instructions,
- your OS,
- the output of `julia --version`.

#### Notes for the exercise

- Prefer official information.
- If an AI agent gives install commands, ask where the instructions came from.
- Confirm the installation by running Julia, not only by reading an install log.
- Do not include long installation notes in your solution unless the exercise asks for them.

### 0.5 Running Julia

#### Explanation

Jupyter notebooks are not required for this material. For short checks, the Julia REPL is enough. For reproducible checks, a `.jl` script is often better because it can be saved, reviewed, and compared with `git diff`.

Use notebooks if they help, but do not make the notebook itself the point of the exercise.

#### Things to look up

- Julia REPL
- Running a Julia script
- Jupyter with Julia
- Literate programming

#### Exercise

Run one simple calculation in the Julia REPL. Then write the same calculation in a `.jl` file and run it from the command line.

#### Notes for the exercise

- You should know which commands you ran.
- You should know which file contains the code.
- You should be able to reproduce the result later.

### 0.6 Basic workflow

#### Explanation

Use this workflow for most subsections:

1. Read the short explanation.
2. Look up unfamiliar terms.
3. Solve the exercise by hand.
4. Use Julia only when a small coding check helps.
5. If you use an AI agent, review its reasoning and any diff it creates.

#### Things to look up

- Hand-written exercise
- Pseudocode
- Unit test
- Validation
- Reproducibility

#### Exercise

For one subsection below, write a short study plan that includes reading, searching, a hand-written answer, and an optional Julia check.

#### Notes for the exercise

- Do not skip the hand-written step.
- Do not start by asking an AI agent for the final answer.
- Make the validation step explicit.

### 0.7 What not to do

#### Explanation

Do not use the AI agent as a replacement for understanding. A working script is not enough. You must be able to explain the problem, the algorithm, the tests, and the limitations.

#### Things to look up

- Hallucination in AI coding
- Rubber duck debugging
- Code review
- Scientific reproducibility

#### Exercise

Rewrite each weak prompt into a better prompt:

- "Write the code."
- "Is this correct?"
- "Make it faster."
- "Fix the error."

#### Notes for the exercise

- A better prompt names the goal.
- A better prompt gives context.
- A better prompt asks for checks, assumptions, or failure modes.
- A better prompt does not remove your responsibility to inspect the answer.

## Section 1: Learning with AI and Web Search

### 1.1 Asking useful questions

#### Explanation

Useful questions are specific. Instead of asking whether something is correct, ask about inputs, outputs, assumptions, edge cases, complexity, and validation.

#### Things to look up

- How to ask coding questions
- Input and output
- Edge case
- Failure mode
- Prompting for code review

#### Exercise

You are given an unknown function that computes a numerical quantity from an array. Write five questions you would ask an AI agent to understand the function.

#### Notes for the exercise

- Include at least one question about input.
- Include at least one question about output.
- Include at least one question about boundary cases.
- Include at least one question about validation.
- Avoid the phrase "Is it correct?" unless you also specify a criterion.

### 1.2 Reading documentation and examples

#### Explanation

Documentation and examples are not the same. Documentation describes the intended behavior. Examples show common usage. You need both, and you should prefer official documentation when learning core tools.

#### Things to look up

- Official documentation
- API reference
- Example code
- Minimal working example
- Julia documentation

#### Exercise

Choose one Julia function related to arrays or mathematics. Find its official documentation and one example. Write down what the function does, what inputs it expects, and one possible mistake when using it.

#### Notes for the exercise

- Use an official source when possible.
- Do not copy a long documentation passage.
- Explain the function in your own words.
- Include one concrete input example.

### 1.3 Checking AI explanations

#### Explanation

AI explanations can be useful and still incomplete. Check whether the explanation states assumptions, handles boundary cases, and gives a way to verify the claim.

#### Things to look up

- Hallucination
- Verification
- Assumption
- Counterexample
- Sanity check

#### Exercise

Ask an AI agent to explain a simple algorithm, such as computing the maximum of an array. Then write three checks you would apply to the explanation.

#### Notes for the exercise

- Check whether empty input is discussed.
- Check whether the algorithm updates the correct state.
- Check whether the explanation distinguishes the algorithm from its implementation.

## Section 2: Programming Fundamentals with Julia

### 2.1 Variables and state

#### Explanation

A variable name refers to a value. Assignment updates what a name refers to. In programming, `x = x + 1` is a state update, not a mathematical equation.

#### Things to look up

- Variable
- Assignment
- State
- Mutation
- Julia variable assignment

#### Exercise

For the following pseudocode, make a table showing the value of each variable after every line:

```text
x = 2
y = x + 3
x = x + 1
z = x * y
```

Then explain why `x = x + 1` is not a mathematical equality.

#### Notes for the exercise

- Track the order of updates.
- Separate variable names from values.
- Do not skip intermediate states.

#### Optional coding check

Run the same lines in the Julia REPL and compare the result with your table.

### 2.2 Conditionals

#### Explanation

A conditional chooses a branch based on a condition. Scientific code often uses conditionals for boundary cases, stopping criteria, and special mathematical cases.

#### Things to look up

- Conditional statement
- Boolean expression
- If else
- Boundary value
- Piecewise function

#### Exercise

Write pseudocode for an absolute-value function and a sign function. For each function, explicitly state what happens when `x = 0`.

#### Notes for the exercise

- Conditions should not overlap in a confusing way.
- There should be no missing case.
- Boundary values must be handled explicitly.

#### Optional coding check

Implement the pseudocode in Julia and test `x = -1`, `x = 0`, and `x = 1`.

### 2.3 Loops

#### Explanation

A loop repeats a block of operations. Many loops use an accumulator, such as a running sum, count, maximum, or error estimate.

#### Things to look up

- For loop
- Accumulator
- Loop invariant
- Off-by-one error
- Empty array

#### Exercise

Write pseudocode to compute the sum, maximum, and average of an array. For each algorithm, state the initial value and what changes during each loop iteration.

#### Notes for the exercise

- The empty-array case must be discussed.
- The one-element case must be discussed.
- For the maximum, the initial value must be justified.
- For the average, explain when division occurs.

#### Optional coding check

Run your algorithm on arrays of length 0, 1, and 3. If empty input is invalid, make that explicit.

### 2.4 Functions

#### Explanation

A function should have a clear responsibility. To understand a function, identify its inputs, output, and side effects. A side effect is a change outside the returned value, such as modifying an input array, writing a file, or printing.

#### Things to look up

- Function
- Argument
- Return value
- Side effect
- Pure function

#### Exercise

Write a specification for a function that computes the mean and variance of a list of numbers. Include inputs, outputs, and exceptional cases.

#### Notes for the exercise

- State whether the function changes its input.
- State what happens for empty input.
- State which variance convention is used if relevant.
- Do not combine unrelated responsibilities in one function.

#### Optional coding check

Implement the function in Julia and test it on simple arrays where the answer can be computed by hand.

### 2.5 Data structures

#### Explanation

A data structure should match the question you need to answer. Lists, dictionaries, sets, arrays, and structs support different operations and make different assumptions visible.

#### Things to look up

- Array
- Dictionary
- Set
- Struct
- Lookup

#### Exercise

You need to store student names and scores. Describe at least two possible data structures and explain which one you would choose for:

- preserving input order,
- finding a score by name,
- removing duplicate names.

#### Notes for the exercise

- State whether order matters.
- State whether duplicates are allowed.
- State which operation should be fast.
- Do not choose a data structure only because it is familiar.

## Section 3: Program Structure

### 3.1 Abstraction and interface

#### Explanation

An abstraction hides details behind a small set of operations. An interface is what other code is allowed to rely on. This matters in scientific computing because the same concept, such as a vector, matrix, lattice, mesh, or model, can have many implementations.

Julia is useful here because simple functions and user-defined types can express mathematical ideas without much ceremony.

#### Things to look up

- Abstraction
- Interface
- Implementation detail
- Public API
- Julia struct

#### Exercise

Imagine representing a two-dimensional vector. Compare two designs:

- store it as a plain two-element array,
- define a `Vector2D` type with operations such as norm, addition, and scaling.

Write what users of each design need to know.

#### Notes for the exercise

- Separate what the user sees from how the data is stored.
- State what operations are guaranteed.
- State what changes would break user code.
- Do not add an abstraction unless it clarifies responsibility.

#### Optional coding check

Ask an AI agent to show a minimal Julia example of a custom type for a vector. Inspect whether the example makes the interface clearer or only adds complexity.

## Section 4: Correctness and Validation

### 4.1 Testing and TDD

#### Explanation

A test checks expected behavior. Test-driven development means stating the expected behavior before writing or changing the implementation. The important habit is not the ritual, but the discipline of making expectations explicit.

#### Things to look up

- Unit test
- Regression test
- Test-driven development
- Reference implementation
- Julia Test standard library

#### Exercise

Design five tests for a function that returns the mean of an array. For each test, write what property it checks.

#### Notes for the exercise

- Include empty input or explain why it is invalid.
- Include a one-element input.
- Include repeated values.
- Include negative values.
- Include a case whose answer is known by hand.
- Tests should check the specification, not copy the implementation.

#### Optional coding check

Use Julia's `Test` standard library to write one of your tests. Use official documentation or an AI agent to find the minimal syntax.

### 4.2 Edge cases and failure modes

#### Explanation

An edge case is an input near a boundary of the specification. A failure mode is a way the code or method can give a wrong, misleading, or unusable result.

#### Things to look up

- Edge case
- Failure mode
- Boundary condition
- Invalid input
- Defensive programming

#### Exercise

For each task, list at least three edge cases and two failure modes:

- computing an average,
- finding a root of a function,
- running a Monte Carlo simulation.

#### Notes for the exercise

- Include both programming failures and scientific failures.
- Do not list only syntax errors.
- Include cases where the code runs but the result is misleading.

### 4.3 Reproducibility as validation

#### Explanation

A result is more trustworthy when someone can reproduce how it was obtained. Reproducibility requires keeping the input, code version, parameters, random seed when relevant, and output together in an inspectable form.

#### Things to look up

- Reproducibility
- Random seed
- Metadata
- Version information
- Saved output

#### Exercise

Suppose a script computes a numerical estimate and writes a result file. List the information that should be saved with the result so another student can inspect the calculation.

#### Notes for the exercise

- Include parameters.
- Include software or code version information.
- Include the random seed if randomness is used.
- Include enough information to connect the output to the run that produced it.

## Section 5: Algorithmic Thinking

### 5.1 Complexity

#### Explanation

Complexity estimates how work grows with input size. A calculation that is fast for `N = 100` can become impossible for `N = 10^6` if the algorithm scales poorly.

#### Things to look up

- Big O notation
- Linear time
- Quadratic time
- Nested loop
- Hidden constant

#### Exercise

Estimate the number of loop iterations for one loop, two nested loops, and three nested loops over an input of size `N`. Then compare what happens for `N = 10^3` and `N = 10^6`.

#### Notes for the exercise

- Count loops before naming the complexity.
- Distinguish `O(N)`, `O(N^2)`, and `O(N^3)`.
- Do not conclude that an algorithm scales well only because it works for a small input.

### 5.2 Search and sort

#### Explanation

Searching and sorting are basic operations with important assumptions. Binary search is fast, but only applies when the data are sorted. Some sorting algorithms are simple but slow for large inputs.

#### Things to look up

- Linear search
- Binary search
- Sorting algorithm
- Stable sort
- Selection sort or insertion sort

#### Exercise

Write the difference between linear search and binary search. Then manually run selection sort or insertion sort on a short list of numbers.

#### Notes for the exercise

- State the precondition for binary search.
- Explain how duplicates are handled.
- Count how many comparisons or steps are needed for the small example.
- State whether stable sorting matters for your example.

#### Optional coding check

Ask Julia to sort a small array. Then explain what the built-in function hides from you.

## Section 6: Scientific Computing Basics with Julia

### 6.1 Numerical integration

#### Explanation

Numerical integration approximates an integral by combining function values. A single plausible number is not enough. You should compare with known answers when possible and check how the result changes as the step size changes.

#### Things to look up

- Numerical integration
- Trapezoidal rule
- Simpson's rule
- Step size
- Convergence

#### Exercise

Use the trapezoidal rule to approximate the integral of `f(x) = x^2` from `0` to `1`. Write the formula, compute a small example by hand, and state what should happen when the step size is halved.

#### Notes for the exercise

- Compare with the exact answer.
- Do not rely on one step size.
- State how error should change qualitatively.
- Discuss what could go wrong for a non-smooth function.

#### Optional coding check

Implement the trapezoidal rule in Julia and test several step sizes.

### 6.2 Root finding

#### Explanation

Root finding tries to solve `f(x) = 0`. Bisection is robust when a sign change brackets a root. Newton's method can be fast, but it depends on the initial guess and derivative behavior.

#### Things to look up

- Bisection method
- Newton's method
- Bracketing
- Stopping criterion
- Convergence failure

#### Exercise

For bisection, write the invariant that must remain true at every step. For Newton's method, describe two situations where the method can fail.

#### Notes for the exercise

- Bisection requires a bracketing condition.
- The stopping criterion must be explicit.
- Newton's method depends on the initial value.
- A small derivative or multiple roots can cause trouble.

#### Optional coding check

Ask an AI agent to help write a minimal Julia bisection function. Before accepting it, check the bracketing condition and stopping rule.

### 6.3 Linear algebra computations

#### Explanation

Linear algebra code must track shapes. Matrix-vector products, matrix-matrix products, transposes, and factorizations all have shape requirements. Large matrix operations also have important complexity costs.

#### Things to look up

- Matrix-vector product
- Matrix-matrix product
- Shape mismatch
- Dense matrix
- Sparse matrix

#### Exercise

Write pseudocode for matrix-vector multiplication and matrix-matrix multiplication. State the input shapes and estimate the complexity for square matrices of size `N`.

#### Notes for the exercise

- State all shapes before writing loops.
- Check index order.
- Matrix-matrix multiplication is usually `O(N^3)` for the basic algorithm.
- Mention whether symmetry or sparsity could change the approach.

#### Optional coding check

Use Julia to multiply small matrices and vectors. Then intentionally try an incompatible shape and inspect the error message.

### 6.4 Monte Carlo and randomness

#### Explanation

Monte Carlo methods use randomness to estimate quantities. A Monte Carlo result should include the random seed, sample size, estimated uncertainty, and a check that the result changes sensibly with more samples.

#### Things to look up

- Monte Carlo method
- Random seed
- Sampling error
- Error bar
- Law of large numbers

#### Exercise

Describe how to estimate pi by sampling random points in a square. State what data you would save and how you would check whether the estimate is reliable.

#### Notes for the exercise

- Record the seed.
- Record the number of samples.
- Do not judge from one run only.
- Include an uncertainty estimate or repeated independent runs.
- Explain how the error should change as the sample size increases.

#### Optional coding check

Write a small Julia script for the pi estimate. Run it with two sample sizes and compare the results.

## Section 7: Reproducible Scientific Workflow

### 7.1 Git basics for scientific workflow

#### Explanation

Git is a way to keep a history of a project. In scientific computing, Git is not only a software development tool. It is also a way to inspect what changed between calculations.

Official Git book:

- https://git-scm.com/book/en/v2

#### Things to look up

- Repository
- Commit
- Diff
- Git history
- Working tree

#### Exercise

Explain repository, commit, and diff in your own words. Then list what you should inspect after an AI agent changes code.

#### Notes for the exercise

- A repository is a project folder with history.
- A commit is a saved checkpoint.
- A diff shows what changed.
- Read the diff after AI-agent edits.
- Do not put a large unrelated set of changes into one commit.

### 7.2 Project structure and reproducibility

#### Explanation

A scientific computing project should separate source code, data, results, scripts, and notes. A reader should be able to find the entry point, understand what was run, and reproduce key results.

#### Things to look up

- Project structure
- README
- Source code
- Results directory
- Work log

#### Exercise

Design a project structure with at least:

- `src/`
- `data/`
- `results/`
- `scripts/`
- `README.md`
- `notes/`

Then list what information is needed to reproduce one result.

#### Notes for the exercise

- Do not mix input data, source code, and generated output.
- State what should be saved and what can be regenerated.
- Make the README the entry point.
- Connect each result file to its input parameters and code version.

### 7.3 AI-agent workflow

#### Explanation

An AI agent can write code, tests, explanations, and logs. That does not remove the need for human review. The core workflow is: define the task, ask for a plan, inspect the change, run checks, and record what was done.

#### Things to look up

- Agentic coding
- Code review
- Work log
- Test report
- Validation report

#### Exercise

Write a checklist of ten items to inspect after an AI agent changes code for a scientific calculation. Then write three review prompts that are more specific than "Is this correct?"

#### Notes for the exercise

- Include diff review.
- Include tests.
- Include validation against known results or sanity checks.
- Include reproducibility information.
- Include the connection between code changes and numerical results.
- Do not accept "it runs" as sufficient evidence.

## Practical Project Exercises

The directories under `exercises/` contain larger agentic-coding exercises. They may use Python or another language when that is appropriate for the project.

- [Ising MCMC](exercises/ising_mcmc/)
- [Tight-Binding Density of States](exercises/tight_binding_dos/)

These exercises are not meant to replace the basic sections above. They are larger practice tasks for applying the workflow: design before coding, implement in small pieces, save inspectable data, test deterministic parts, validate numerical results, and review AI-agent diffs.
