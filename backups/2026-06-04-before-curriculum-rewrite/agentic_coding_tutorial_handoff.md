# Agentic Coding-Based Numerical Computing Tutorial for B4 Students

## Purpose

This document summarizes the design concept for a numerical computing tutorial for fourth-year undergraduate students in computational physics and condensed matter theory. The tutorial is based on agentic coding: students work with an AI coding agent to design, implement, validate, and analyze small numerical physics projects.

The central goal is not to teach students how to copy AI-generated code. The goal is to train them to use AI agents as collaborators while retaining responsibility for physical reasoning, numerical method selection, validation, reproducibility, and interpretation of results.

## Core Educational Philosophy

The tutorial should teach the following principle:

> Do not just ask AI to write code. Ask AI to help you choose, test, analyze, and doubt the method.

In Japanese:

> AIにコードを書かせる前に、AIと方法を議論せよ。AIに結果を説明させる前に、収束・誤差・保存則を確認せよ。

Students should experience the full research workflow:

1. Define a physical or numerical problem.
2. Ask the AI agent to propose multiple algorithmic options.
3. Compare the options using explicit decision criteria.
4. Choose a method and record the decision-making process.
5. Implement the method with the help of the AI agent.
6. Save numerical results to files.
7. Analyze saved results separately.
8. Validate the results using physical and numerical checks.
9. Revise the method or parameters if needed.
10. Record the work process in a work log.

## What Should Not Be Included as Fixed Material

The tutorial should not include fixed generated code as part of the teaching material.

Reasons:

- AI-generated code varies by agent, model, language, and context.
- Fixed code encourages students to treat the material as a conventional programming exercise.
- The main learning objective is the decision and validation process, not reproducing a canonical implementation.
- Different students may use Python, Julia, Rust, or another suitable language.

Instead of generated code, the material should provide:

- Problem statements.
- Required analysis tasks.
- Algorithmic options to be considered, or instructions to ask the AI agent to propose options.
- Decision criteria.
- Validation checklists.
- Expected qualitative behavior.
- Common failure modes.
- Work-log requirements.

## Minimum Project Template

The project template should be minimal. Avoid elaborate scaffolding. Students should learn how to ask the AI agent to create a suitable project structure for each task.

A minimal initial repository may contain only:

```text
AGENTS.md
README.md
work_log.md
```

Optionally, each exercise may have its own directory:

```text
exercise_01_harmonic_oscillator/
  README.md
  work_log.md
exercise_02_tight_binding/
  README.md
  work_log.md
exercise_03_ising_mcmc/
  README.md
  work_log.md
```

The detailed project structure should be negotiated with the AI agent, subject to the minimum requirements below.

## Minimum Requirements for Every Exercise

Each exercise must satisfy the following requirements:

1. The code must run from the command line.
2. A language-specific project environment must be used.
3. Numerical results must be saved to files.
4. Analysis must read saved result files rather than silently recomputing the simulation.
5. Important parameters and random seeds must be recorded.
6. At least one validation check must be performed.
7. A work log must record the decision-making process and the performed validation.

## Role of AGENTS.md

`AGENTS.md` should define the common working rules for the AI coding agent. Students should instruct the agent to read this file before doing any work.

A typical first instruction to the agent may be:

```text
Read AGENTS.md first. Then help me solve this exercise following the workflow described there.
```

The purpose of `AGENTS.md` is not to provide answers. It is a rule file that forces the AI agent to follow the tutorial philosophy: method selection, reproducibility, validation, and work logging.

## Proposed AGENTS.md Content

```markdown
# AGENTS.md

This repository is for an agentic-coding tutorial in computational physics.

## General principles

Do not start by writing code immediately.

First:
1. Understand the physical problem.
2. Propose multiple numerical methods or algorithms.
3. Compare them using explicit decision criteria.
4. Ask the student to choose or confirm a method.
5. Record the decision-making process in `work_log.md`.

## Reproducibility

Use a proper project environment for the chosen language.

Examples:
- Python: use `pyproject.toml`, `uv`, `venv`, or another standard mechanism.
- Julia: use `Project.toml` and `Pkg.activate(".")`.
- Rust: use `Cargo.toml`.

Keep the project structure minimal. Do not create unnecessary directories or frameworks.

## Workflow

Separate simulation and analysis.

The simulation code should:
- run from the command line,
- save raw numerical results to files,
- save parameters and random seeds,
- avoid silently overwriting important results.

The analysis code should:
- read saved result files,
- produce figures or summary tables,
- not recompute the simulation unless explicitly requested.

## Work log

Maintain `work_log.md`.

For each major step, record:
- what was attempted,
- what the AI agent suggested,
- what options were considered,
- the decision points,
- the chosen method,
- the reason for the choice,
- what was implemented,
- what validation was actually performed,
- what problems remain.

Do not include the full prompt history. Summaries are sufficient.

Do not claim that a test, validation, or numerical experiment was performed unless it was actually run.

## Validation

Every numerical result must be checked.

Possible validation methods include:
- comparison with an analytic solution,
- convergence with respect to step size,
- conservation laws,
- limiting cases,
- small-system exact results,
- random-seed dependence,
- statistical error estimates,
- autocorrelation analysis for MCMC.

If validation is incomplete, state this explicitly in `work_log.md`.

## Style

Prefer simple, readable code.

Avoid over-engineering. This is a small computational physics project, not a production software package.

Explain numerical choices in terms of physics, stability, accuracy, computational cost, and ease of validation.
```

## Work Log Design

The work log is more important than the raw prompt history. Students do not need to submit full prompts. They should submit a structured summary of what they asked, what options were considered, what decisions were made, and how the results were validated.

Suggested `work_log.md` structure:

```markdown
# Work log

## Goal

What was the goal of this step or exercise?

## Interaction summary

Summarize what was asked of the AI agent. Do not paste the full prompt history.

## Options considered

List the algorithmic, numerical, or project-structure options that were considered.

## Decision points

List the criteria used for choosing among the options.

Examples:
- accuracy,
- stability,
- energy conservation,
- computational cost,
- implementation simplicity,
- ease of validation,
- robustness to step size,
- convergence behavior.

## Decision

State the chosen method or design.

## Reason

Explain why this choice was made.

## Implementation summary

Summarize what was implemented or changed.

## Validation actually performed

List only checks that were actually run.

## Results

Summarize the numerical results and figures.

## Problems and revisions

Describe problems encountered and how they were addressed.

## Remaining concerns

State what is still uncertain or unverified.
```

## Computational Workflow

The tutorial should emphasize moving away from notebook-centered workflows. Jupyter notebooks may be used for exploration, but final outputs should be based on scripts, saved data, and reproducible analysis.

Preferred workflow:

```text
simulation script
  parameters -> raw result files

analysis script
  raw result files -> figures, tables, diagnostics
```

Students should learn that computational results consist of more than code. A result includes:

- code,
- environment,
- parameters,
- random seeds,
- raw data,
- analysis procedure,
- validation checks,
- figures,
- interpretation.

## File Saving Policy

Numerical results should be saved to files. The analysis should read those files.

For simple B4 exercises, CSV plus JSON metadata is sufficient.

Example:

```text
results/raw/run_0001.csv
results/raw/run_0001.json
results/processed/summary.csv
results/figures/energy_error.png
```

Example metadata:

```json
{
  "model": "harmonic_oscillator",
  "method": "velocity_verlet",
  "dt": 0.01,
  "n_steps": 10000,
  "parameters": {
    "mass": 1.0,
    "k": 1.0
  },
  "seed": null
}
```

For MCMC examples, the metadata should include at least:

- lattice size,
- temperature,
- coupling constants,
- number of sweeps,
- thermalization sweeps,
- measurement interval,
- random seed.

## Language and Environment Expectations

The tutorial should not mandate a single language unless operationally necessary. Python and Julia are natural first choices; Rust may be used by advanced students.

Students should ask the AI agent to set up a minimal reproducible project environment.

Expected environment conventions:

### Python

Acceptable tools:

- `pyproject.toml`,
- `uv`,
- `venv`,
- `pytest`,
- optionally `ruff` or another formatter/linter.

### Julia

Expected tools:

- `Project.toml`,
- `Manifest.toml` if needed,
- `Pkg.activate(".")`,
- `Test`.

### Rust

Expected tools:

- `Cargo.toml`,
- `cargo run`,
- `cargo test`.

The purpose is not to teach package management in detail. The purpose is to make students aware that reproducibility requires an explicit environment.

## Candidate Exercise Modules

A compact tutorial may contain four to six modules.

### Module 1: Project Setup and Reproducible Workflow

Goal:

- Learn to use `AGENTS.md`.
- Ask the AI agent to propose a minimal project structure.
- Set up a language-specific environment.
- Create a workflow with separate simulation and analysis.
- Write and maintain `work_log.md`.

Possible task:

- Generate a small dataset from a known function.
- Save it to a file.
- Analyze it using a separate script.
- Produce a figure.

### Module 2: Time Evolution and Step-Size Dependence

Physical problem:

- One-dimensional harmonic oscillator.

Equation:

```text
m d^2x/dt^2 = -k x
```

Algorithmic options to consider:

- explicit Euler,
- symplectic Euler,
- Runge-Kutta,
- velocity Verlet,
- other symplectic integrators.

Decision criteria:

- long-time stability,
- energy conservation,
- accuracy,
- implementation simplicity,
- ease of comparison with analytic solution.

Required analysis:

- plot position and velocity,
- plot total energy,
- analyze step-size dependence,
- compare with analytic solution,
- discuss long-time energy drift or bounded oscillation.

Expected qualitative behavior:

- explicit Euler tends to show systematic energy drift,
- velocity Verlet tends to keep the energy error bounded,
- smaller step sizes reduce numerical error,
- phase error may accumulate at long times.

### Module 3: Eigenvalue Problems in Condensed Matter Theory

Physical problem:

- One-dimensional tight-binding chain.

Hamiltonian:

```text
H = -t sum_i (c_i^dagger c_{i+1} + c_{i+1}^dagger c_i)
```

Algorithmic options to consider:

- exact diagonalization of finite matrix,
- k-space analytic dispersion,
- Green's function with broadening,
- density of states from eigenvalues,
- Chebyshev expansion as an advanced option.

Decision criteria:

- boundary conditions,
- finite-size effects,
- computational cost,
- ease of validation,
- extensibility to disorder or interactions.

Required analysis:

- compare open and periodic boundary conditions,
- compare numerical eigenvalues with analytic dispersion where possible,
- compute density of states,
- check normalization of the density of states.

Expected qualitative behavior:

- periodic boundary conditions reproduce discrete momenta,
- open boundary conditions modify eigenstates and finite-size spectra,
- broadening changes the smoothness of the density of states,
- normalization errors often reveal missing factors.

### Module 4: Monte Carlo and MCMC Convergence

Physical problem:

- Two-dimensional Ising model.

Hamiltonian:

```text
H = -J sum_<ij> s_i s_j
```

Algorithmic options to consider:

- single-spin Metropolis,
- heat-bath update,
- Wolff cluster update,
- Swendsen-Wang update,
- exact enumeration for small systems.

Decision criteria:

- implementation simplicity,
- detailed balance,
- thermalization behavior,
- autocorrelation time,
- performance near the critical temperature,
- ease of validation.

Required analysis:

- running average of energy,
- running average of magnetization,
- thermalization time,
- autocorrelation time,
- random-seed dependence,
- finite-size dependence if time permits.

Expected qualitative behavior:

- early samples depend strongly on initial condition,
- thermalization samples should be discarded,
- autocorrelation grows near the critical temperature,
- single-spin Metropolis suffers from critical slowing down,
- independent seeds should agree within statistical error.

Common failure modes:

- forgetting thermalization,
- underestimating errors due to autocorrelation,
- confusing magnetization with absolute magnetization,
- implementing periodic boundary conditions incorrectly,
- claiming convergence based only on a short time series.

### Module 5: Optimization

Optimization should be included because it naturally connects numerical analysis, computational physics, and modern agentic workflows.

Recommended first topic:

- Variational energy minimization.

Example physical problem:

- Minimize the variational energy of a trial wavefunction for a harmonic oscillator.

Trial function:

```text
psi_alpha(x) = exp(-alpha x^2 / 2)
```

Task:

- Minimize E(alpha).
- Compare optimization strategies.

Algorithmic options to consider:

- grid search,
- gradient descent,
- Newton method,
- derivative-free optimization,
- library optimizer.

Decision criteria:

- robustness,
- convergence speed,
- need for derivatives,
- sensitivity to step size or initial value,
- ease of diagnosing failure,
- comparison with analytic result.

Required analysis:

- plot objective function,
- check convergence behavior,
- test sensitivity to initial conditions,
- compare with a simple baseline such as grid search,
- validate against known analytic minimum if available.

Expected qualitative behavior:

- grid search is slow but robust,
- gradient descent is step-size dependent,
- Newton-type methods can converge quickly near the optimum but may fail if used carelessly,
- a library optimizer is convenient but should not be treated as magic.

Alternative optimization topic:

- Least-squares fitting of noisy numerical data.

Example:

```text
C(t) = A exp(-t / tau) + noise
```

This connects naturally to correlation functions and autocorrelation analysis.

### Module 6: Mini Project

The final project should require students to choose a small computational physics problem and apply the full workflow.

Possible topics:

- density of states of a one-dimensional tight-binding model,
- two-dimensional Ising model near the critical temperature,
- diffusion equation with stability analysis,
- finite-difference Schrödinger equation,
- variational energy minimization,
- simple Langevin dynamics,
- two-site or four-site Hubbard model exact diagonalization.

The final project should not be judged primarily by code length or sophistication. It should be judged by method selection, reproducibility, validation, and analysis.

## Assessment Criteria

Suggested grading dimensions:

### Reproducibility

- Uses an explicit project environment.
- Provides clear run commands.
- Saves results and metadata.
- Separates simulation and analysis.

### Method Selection

- Considers multiple algorithmic options.
- Uses explicit decision points.
- Explains the chosen method and rejected alternatives.

### Validation

- Performs relevant checks.
- Uses analytic solutions or limiting cases when available.
- Checks convergence with respect to step size, sample size, or system size.
- Treats statistical uncertainty appropriately.

### Result Analysis

- Produces meaningful figures or summary tables.
- Interprets results in physical and numerical terms.
- Identifies failures or limitations.

### Agent Usage

- Uses the AI agent for discussion, implementation, debugging, and analysis.
- Does not blindly trust the AI output.
- Records decisions and remaining uncertainties in the work log.

## Submission Requirements

For each exercise, students should submit:

```text
README.md
AGENTS.md if modified
work_log.md
source code or scripts
analysis script
saved result files or clear regeneration instructions
figures or summary tables
```

The work log should be treated as a central submission artifact.

## Suggested README Structure for Each Exercise

```markdown
# Exercise title

## Physical problem

Describe the model or equation.

## Tasks

List the required computational tasks.

## Required decisions

List the algorithmic choices the student must discuss with the AI agent.

## Required validation

List the minimum validation checks.

## Expected behavior

Describe qualitative behavior that correct implementations should approximately show.

## Common failure modes

List typical mistakes or misleading conclusions.
```

## Example Exercise README: Harmonic Oscillator

```markdown
# Exercise 1: Harmonic Oscillator

## Physical problem

Simulate a one-dimensional harmonic oscillator and study long-time energy behavior.

## Tasks

- Ask the AI agent to propose multiple time-integration methods.
- Choose a method using explicit decision points.
- Implement the simulation.
- Save numerical results to files.
- Analyze step-size dependence and energy conservation.
- Record the workflow and decisions in `work_log.md`.

## Required decisions

Discuss at least three candidate methods, such as:

- explicit Euler,
- Runge-Kutta,
- velocity Verlet,
- symplectic Euler.

Decision points should include:

- accuracy,
- long-time stability,
- energy conservation,
- implementation difficulty,
- ease of validation.

## Required validation

- Compare with the analytic solution.
- Plot total energy as a function of time.
- Study how the error changes with step size.
- Discuss whether the energy error drifts or remains bounded.

## Expected behavior

- Explicit Euler is generally poor for long-time Hamiltonian dynamics.
- Velocity Verlet should show better long-time energy behavior.
- Reducing the step size should reduce the error.

## Common failure modes

- Claiming correctness because the trajectory looks oscillatory.
- Not checking energy conservation.
- Comparing only short-time behavior.
- Forgetting to save parameters used for each run.
```

## Example Exercise README: Ising MCMC

```markdown
# Exercise: Two-Dimensional Ising Model

## Physical problem

Simulate the two-dimensional Ising model using Markov-chain Monte Carlo.

## Tasks

- Ask the AI agent to propose multiple Monte Carlo algorithms.
- Choose an initial algorithm using explicit decision points.
- Implement the simulation.
- Save time series of energy and magnetization.
- Analyze thermalization, running averages, and autocorrelation.
- Record decisions and validation in `work_log.md`.

## Required decisions

Discuss at least three candidate methods, such as:

- single-spin Metropolis,
- heat-bath update,
- Wolff cluster update,
- exact enumeration for small systems.

Decision points should include:

- ease of implementation,
- detailed balance,
- autocorrelation time,
- behavior near the critical temperature,
- validation strategy.

## Required validation

- Check high-temperature and low-temperature behavior.
- Estimate thermalization time.
- Compare independent random seeds.
- Analyze autocorrelation qualitatively or quantitatively.
- If possible, compare a very small system with exact enumeration.

## Expected behavior

- Near the critical temperature, single-spin Metropolis should show slow convergence.
- Early samples should be discarded as thermalization.
- Error bars should account for correlations.

## Common failure modes

- Treating correlated MCMC samples as independent.
- Forgetting to discard thermalization.
- Using only one random seed.
- Confusing magnetization and absolute magnetization.
- Declaring convergence from a visually short trajectory.
```

## Implementation Notes for Codex

If this document is handed to Codex for implementation, the recommended first steps are:

1. Create a minimal repository skeleton.
2. Add a root `AGENTS.md` based on the proposed content above.
3. Add a top-level `README.md` explaining the tutorial philosophy.
4. Add one directory per exercise, each with only a `README.md` and `work_log.md` template.
5. Do not add full generated solution code initially.
6. Add optional reference behavior and common failure modes to each exercise README.
7. Keep the structure intentionally lightweight.

Possible initial repository structure:

```text
agentic-numerical-physics-tutorial/
  README.md
  AGENTS.md
  exercises/
    01_project_workflow/
      README.md
      work_log.md
    02_harmonic_oscillator/
      README.md
      work_log.md
    03_tight_binding/
      README.md
      work_log.md
    04_ising_mcmc/
      README.md
      work_log.md
    05_optimization/
      README.md
      work_log.md
    06_mini_project/
      README.md
      work_log.md
```

The implementation should avoid premature complexity. The key educational artifact is the workflow specification, not a collection of polished reference codes.
