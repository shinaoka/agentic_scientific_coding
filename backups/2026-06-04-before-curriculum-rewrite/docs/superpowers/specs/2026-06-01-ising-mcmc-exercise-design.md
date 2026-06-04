# Ising MCMC Exercise Design

Date: 2026-06-01

## Purpose

This repository will host public teaching material for agentic scientific coding under `github.com/shinaoka/agentic_scientific_coding`.

The first exercise is a two-dimensional Ising model Markov-chain Monte Carlo exercise for fourth-year undergraduate students. The goal is not to provide fixed solution code. The goal is to teach students how to use an AI coding agent to design, implement, test, validate, and analyze a numerical physics calculation while keeping responsibility for the scientific judgment.

The exercise should make students practice asking the AI agent to help choose methods, define checks, create minimal tests, save reproducible data, and question the resulting numerical claims.

## Repository Structure

The initial repository should stay lightweight:

```text
README.md
exercises/
  ising_mcmc/
    README.md
```

The top-level `README.md` introduces the tutorial philosophy and links to exercise subdirectories.

Each exercise subdirectory must contain its own `README.md`. The Ising exercise README is the main teaching artifact for this first module.

The repository should not initially include generated solution code, a fixed `pyproject.toml`, `uv.lock`, or prebuilt source files. Students should ask their AI agent to create those files as part of the exercise.

## Standard Implementation Route

The standard route is Python.

Students should instruct the AI agent to use `uv` to create a minimal reproducible Python environment. The README should not prescribe a concrete `pyproject.toml`; it should tell students to ask the AI agent to create the minimal project structure needed for their implementation.

Plotting should use matplotlib in the standard route.

At the end of the exercise, an optional extension should invite students to speed up the implementation using Rust or Julia. The extension must keep the same physical quantities, JSON output structure, and validation requirements so that speed is not treated as a substitute for correctness.

## Data Policy

The standard saved data format is JSON only.

Each simulation run should write a JSON file containing enough information to inspect and reproduce the numerical experiment:

- model name
- algorithm name
- lattice size `L`
- temperature `T`
- number of sweeps
- discarded or thermalization sweeps
- measurement interval
- random seed
- boundary condition
- measured energy time series
- measured magnetization time series

Analysis must read saved JSON files. Analysis scripts should not silently rerun simulations to generate plots.

The exercise should avoid CSV, NumPy `.npy`/`.npz`, and HDF5 as required formats. HDF5 can be mentioned later as a possible research-scale data format, but it is not part of the standard exercise.

## Exercise Flow

The Ising MCMC README should provide a medium-grained guide rather than a fully scripted walkthrough.

Recommended flow:

1. Ask the AI agent to discuss the 2D ferromagnetic Ising model, Metropolis updates, data saving, and validation strategy before writing code.
2. Implement a single-spin Metropolis simulation at one temperature and one random seed.
3. Save the energy and magnetization time series plus metadata as JSON.
4. Analyze the saved JSON with matplotlib.
5. Check low-temperature and high-temperature qualitative behavior.
6. Add minimal unit tests for deterministic implementation details.
7. Validate against exact enumeration for a very small lattice.
8. Compare independent random seeds.
9. Compare low-temperature, near-transition, and high-temperature behavior.
10. Attempt the Binder-parameter challenge for estimating the transition temperature.

## Minimal Unit Tests

The README should ask students to have the AI agent create minimal unit tests.

The tests should focus on small, deterministic implementation checks rather than trying to prove full Monte Carlo correctness.

Suggested targets:

- energy for simple spin configurations
- magnetization normalization
- periodic-boundary neighbor indexing
- local `Delta E` compared with full energy recomputation
- reproducibility of a short run with a fixed random seed

The README should explicitly distinguish unit tests from physical validation:

Unit tests catch implementation mistakes. Physical and numerical validation judge whether the simulation output is trustworthy.

## Required Validation

The standard exercise should require both qualitative and quantitative validation.

Qualitative checks:

- low-temperature behavior
- high-temperature behavior
- comparison of magnetization and absolute magnetization where relevant
- visible thermalization behavior in time series

Quantitative validation:

- exact enumeration for a very small lattice, such as `L = 2`
- comparison between exact thermal averages and MCMC estimates within the limitations of finite sampling

The exercise should encourage students to treat disagreement as diagnostic information, not as a reason to tune parameters until the plot looks acceptable.

## Work Record

The repository should not include a fixed `work_log.md` file.

The Ising README should include an optional work-record template or prompt. Students may ask the AI agent to create a work log if useful.

The work record should emphasize decisions and evidence rather than full prompt transcripts:

- methods considered
- choices made
- tests added
- validations performed
- failures or inconsistencies found
- remaining concerns

## Binder-Parameter Challenge

The final challenge should ask students to estimate the transition temperature using the Binder parameter rather than only using peak positions.

For magnetization per spin `m`, define

```text
U_L(T) = 1 - <m^4> / (3 <m^2>^2)
```

Students should compute `U_L(T)` for several lattice sizes and plot the curves on the same graph. The estimated transition temperature should come from Binder-parameter crossings.

Suggested procedure:

1. Choose several lattice sizes, such as `L = 8, 16, 32`.
2. Run simulations over temperatures near the expected transition region.
3. Save each run as JSON.
4. Compute `<m^2>`, `<m^4>`, and `U_L(T)` from saved JSON files.
5. Plot Binder curves for several `L`.
6. Refine the temperature grid near the crossing region.
7. Increase `L` and check whether crossing estimates become more stable.
8. State clearly what the finite-size calculation supports and what it does not establish.

Checks before claiming a transition temperature:

- Has each Markov chain forgotten its initial condition, and are correlations small enough for the Binder estimate being quoted?
- Do independent seeds give compatible Binder curves?
- Do Binder-parameter crossing points become stable as larger lattice sizes are included?
- Is the temperature grid fine enough near the crossing region?
- Are all Binder curves computed from saved JSON files rather than rerunning simulations during analysis?

## Top-Level README Content

The top-level README should include:

- the purpose of the tutorial
- the principle that students should discuss methods and validation with the AI before asking for code
- the reason fixed solution code is not included
- the standard Python and `uv` route
- the JSON-only saved-data policy for the first exercise
- links to exercise subdirectories
- a short note that each exercise subdirectory has its own README

## Ising README Content

The Ising README should include:

- physical problem statement
- standard implementation route
- instructions to ask the AI agent to use Python, `uv`, JSON, and matplotlib
- algorithmic expectations for single-spin Metropolis
- saved JSON requirements
- analysis requirements
- minimal unit-test request
- required validation
- common failure modes
- optional work-record template
- Binder-parameter challenge
- optional Rust or Julia speedup extension

## Non-Goals

The initial generated repository should not include:

- completed solution code
- fixed generated scripts
- fixed Python project files
- large datasets
- precomputed figures
- a required `work_log.md`
- CSV as a standard output format

These may be created by students and their AI agents during the exercise.
