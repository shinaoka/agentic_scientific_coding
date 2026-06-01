# Tight-Binding DOS Exercise Materials Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add the student-facing tight-binding DOS exercise and link it from the tutorial overview.

**Architecture:** The repository remains documentation-only for exercises. The new exercise gets one README under `exercises/tight_binding_dos/`, and the top-level README gains a link and short description. No solution code, fixed Python project files, generated datasets, or figures are added.

**Tech Stack:** Markdown documentation, Python with `uv` as the recommended student-generated environment, matplotlib for standard plotting, JSON for saved processed DOS data.

---

## File Structure

- Create `exercises/tight_binding_dos/README.md`: student-facing exercise guide with step-by-step prompts and confirmation gates.
- Modify `README.md`: add a link and summary for the tight-binding DOS exercise under Current Exercises.
- Keep `docs/superpowers/specs/2026-06-01-tight-binding-dos-exercise-design.md`: committed design spec.

## Task 1: Tight-Binding Exercise README

**Files:**
- Create: `exercises/tight_binding_dos/README.md`

- [ ] **Step 1: Write the exercise README**

Create a README with these sections:

```markdown
# Exercise: Tight-Binding Density of States

## Goal
## Physical Problem
## Standard Route
## Step-by-Step Workflow
## Saved JSON Data
## Analysis Requirements
## Required Validation
## Van Hove Observation
## Points to Audit
## Advanced Challenge: 2D Van Hove Singularity
## Completion Criteria
```

The workflow must include prompts and confirmation items for:

1. design before coding
2. minimal Python project with `uv`
3. dispersion for `d = 1, 2, 3`
4. dense k-mesh and histogram DOS
5. JSON saved processed DOS data
6. plotting from saved JSON
7. normalization and bandwidth checks
8. 1D analytic DOS comparison
9. qualitative 2D Van Hove observation
10. advanced line-integral and AMR challenge

- [ ] **Step 2: Verify the exercise README**

Run:

```bash
sed -n '1,360p' exercises/tight_binding_dos/README.md
```

Expected: The README contains the sections above, gives prompts and confirmation items, uses JSON as the saved-data format, and includes no solution code.

- [ ] **Step 3: Commit**

Run:

```bash
git add exercises/tight_binding_dos/README.md
git commit -m "Add tight-binding DOS exercise"
```

Expected: A commit is created containing the new exercise README.

## Task 2: Top-Level README Link

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Add exercise link**

Add this exercise under Current Exercises:

```markdown
- [Tight-Binding Density of States](exercises/tight_binding_dos/)  
  Compute the DOS of 1D, 2D, and 3D nearest-neighbor tight-binding models with dense k-mesh histograms, validate normalization and the 1D analytic DOS, and investigate the 2D Van Hove singularity.
```

- [ ] **Step 2: Verify the top-level README**

Run:

```bash
sed -n '65,100p' README.md
```

Expected: Current Exercises lists both Ising MCMC and Tight-Binding Density of States.

- [ ] **Step 3: Commit**

Run:

```bash
git add README.md
git commit -m "Link tight-binding DOS exercise"
```

Expected: A commit is created containing the top-level README link update.

## Task 3: Final Review and Push

**Files:**
- Inspect: `README.md`
- Inspect: `exercises/tight_binding_dos/README.md`
- Inspect: `docs/superpowers/specs/2026-06-01-tight-binding-dos-exercise-design.md`

- [ ] **Step 1: Check for inconsistent material**

Run:

```bash
rg -n "CSV|csv|uv.lock|work_log.md|TODO|TBD|solution code|\\.npz|\\.npy|HDF5|KPM|tetrahedron|Green" README.md exercises/tight_binding_dos/README.md
```

Expected: Any mentions are explicit non-goals or deliberate statements, not required standard outputs or methods.

- [ ] **Step 2: Check repository status**

Run:

```bash
git status -sb
```

Expected: Only the original handoff file remains untracked.

- [ ] **Step 3: Push main**

Run:

```bash
git push origin main
```

Expected: The public repository receives the tight-binding DOS exercise materials.
