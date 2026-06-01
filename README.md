# Agentic Scientific Coding

This repository contains teaching material for learning numerical scientific computing with an AI coding agent.

The goal is not to copy code produced by an AI. The goal is to learn how to use an AI agent as a collaborator while keeping responsibility for the physical model, numerical method, reproducibility, validation, and interpretation of results.

## Purpose

The materials are designed for students beginning computational physics or condensed-matter theory projects.

Students should practice the full workflow of a small numerical study:

1. Define the physical or numerical problem.
2. Ask the AI agent to discuss algorithmic options.
3. Choose a method using explicit criteria.
4. Ask the AI agent to help implement the method.
5. Save numerical results with enough metadata to inspect the run.
6. Analyze saved data separately from the simulation.
7. Add minimal implementation tests where useful.
8. Validate results using physical and numerical checks.
9. State what the calculation supports and what remains uncertain.

## Core Principle

Do not start by asking the AI agent to write code.

Start by asking:

- What method should we use?
- What assumptions does the method make?
- What can go wrong in the implementation?
- What tests can catch simple mistakes?
- What validation would make the numerical result trustworthy?
- What data must be saved so the analysis can be reproduced?

For this tutorial, a good result is not just a working script. A good result is a numerical calculation whose method, data, checks, limitations, and interpretation can be explained.

## Repository Structure

Each exercise lives in its own subdirectory and has its own README.

```text
exercises/
  ising_mcmc/
    README.md
```

The exercise README is the teaching material. It describes the problem, the required workflow, and the checks students should ask the AI agent to help perform.

## Standard Workflow

The first exercise uses Python as the standard route. Students should ask their AI agent to create a minimal Python project using `uv`.

The repository intentionally does not provide a fixed `pyproject.toml`, lockfile, or source tree. Creating the project structure is part of the agentic-coding workflow.

Typical instructions to the AI agent may include:

```text
Use Python and uv to create a minimal reproducible project.
Keep the structure simple.
Separate simulation from analysis.
Save simulation results as JSON.
Use matplotlib for the standard plots.
Add minimal unit tests for deterministic implementation details.
```

## Data Policy

The exercises use JSON as the standard saved-data format.

Simulation output should include both measured time series and metadata such as model parameters, random seed, lattice size, temperature, number of sweeps, discarded sweeps, measurement interval, and boundary condition.

Analysis should read saved JSON files. It should not silently rerun the simulation to regenerate plots.

## Current Exercises

- [Ising MCMC](exercises/ising_mcmc/)  
  Simulate the two-dimensional Ising model with single-spin Metropolis updates, save results as JSON, validate against qualitative limits and exact enumeration, and attempt a Binder-parameter estimate of the transition temperature.

- [Tight-Binding Density of States](exercises/tight_binding_dos/)  
  Compute the DOS of 1D, 2D, and 3D nearest-neighbor tight-binding models with dense k-mesh histograms, validate normalization and the 1D analytic DOS, and investigate the 2D Van Hove singularity.

## What This Repository Does Not Provide

This repository intentionally does not provide completed solution code.

It also does not initially provide fixed project files, generated datasets, precomputed figures, or a required work-log file. Students should create the implementation and supporting files through interaction with their AI agent, then judge the result using tests and validation.
