# Ising MCMC Exercise Materials Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create the public-facing tutorial README files for the first agentic scientific coding exercise.

**Architecture:** The repository stays lightweight: one top-level README for tutorial philosophy and one exercise README for the Ising MCMC module. No solution code, fixed Python project files, lockfiles, datasets, or generated figures are added.

**Tech Stack:** Markdown documentation, GitHub public repository layout, Python with `uv` as the recommended student-generated environment, JSON as the required saved-data format for the exercise.

---

## File Structure

- Create `README.md`: top-level tutorial overview, repository policy, workflow principles, and link to the Ising exercise.
- Create `exercises/ising_mcmc/README.md`: full student-facing exercise guide for 2D Ising MCMC.
- Keep `agentic_coding_tutorial_handoff.md`: unmodified source handoff note.
- Keep `docs/superpowers/specs/2026-06-01-ising-mcmc-exercise-design.md`: committed design spec.

## Task 1: Top-Level README

**Files:**
- Create: `README.md`

- [ ] **Step 1: Write the top-level README**

Create a concise tutorial overview with these sections:

```markdown
# Agentic Scientific Coding

## Purpose
## Core Principle
## Repository Structure
## Standard Workflow
## Data Policy
## Current Exercises
## What This Repository Does Not Provide
```

The README must say that students should discuss methods and validation with the AI before asking for code, that fixed solution code is intentionally absent, that the first exercise uses Python with `uv`, and that the first exercise saves results as JSON.

- [ ] **Step 2: Verify the top-level README**

Run:

```bash
sed -n '1,220p' README.md
```

Expected: The README contains the sections above, links to `exercises/ising_mcmc/`, and does not mention CSV as a standard output format.

- [ ] **Step 3: Commit**

Run:

```bash
git add README.md
git commit -m "Add tutorial overview"
```

Expected: A commit is created containing only `README.md`.

## Task 2: Ising MCMC Exercise README

**Files:**
- Create: `exercises/ising_mcmc/README.md`

- [ ] **Step 1: Write the exercise README**

Create the exercise guide with these sections:

```markdown
# Exercise: Ising MCMC

## Goal
## Physical Problem
## Standard Route
## First Conversation With the AI Agent
## Method Design
## Saved JSON Data
## Analysis Requirements
## Minimal Unit Tests
## Required Validation
## Common Failure Modes
## Optional Work Record
## Challenge: Binder-Parameter Estimate of the Transition Temperature
## Extension: Speed Up With Rust or Julia
## Completion Criteria
```

The README must require single-spin Metropolis as the standard algorithm, JSON-only saved results, matplotlib for standard plotting, minimal unit tests requested from the AI agent, low- and high-temperature qualitative checks, exact enumeration for a small lattice, independent seed comparison, and a Binder-parameter challenge.

- [ ] **Step 2: Verify the exercise README**

Run:

```bash
sed -n '1,260p' exercises/ising_mcmc/README.md
```

Expected: The README contains the sections above and does not include completed solution code.

- [ ] **Step 3: Commit**

Run:

```bash
git add exercises/ising_mcmc/README.md
git commit -m "Add Ising MCMC exercise"
```

Expected: A commit is created containing only the Ising exercise README.

## Task 3: Final Review and Push

**Files:**
- Inspect: `README.md`
- Inspect: `exercises/ising_mcmc/README.md`
- Inspect: `docs/superpowers/specs/2026-06-01-ising-mcmc-exercise-design.md`

- [ ] **Step 1: Search for disallowed or inconsistent material**

Run:

```bash
rg -n "CSV|csv|uv.lock|work_log.md|TODO|TBD|solution code|\\.npz|\\.npy|HDF5" README.md exercises/ising_mcmc/README.md
```

Expected: Any mentions are either explicit non-goals, optional future context, or deliberate statements that these are not standard exercise requirements.

- [ ] **Step 2: Check repository status**

Run:

```bash
git status -sb
```

Expected: Only the original handoff file remains untracked unless the user asks to include it.

- [ ] **Step 3: Push main**

Run:

```bash
git push -u origin main
```

Expected: The public repository `shinaoka/agentic_scientific_coding` receives the committed materials.
