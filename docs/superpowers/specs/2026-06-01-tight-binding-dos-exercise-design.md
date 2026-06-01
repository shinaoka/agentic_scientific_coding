# Tight-Binding DOS Exercise Design

Date: 2026-06-01

## Purpose

This document specifies the second exercise for the agentic scientific coding tutorial.

The exercise introduces the density of states (DOS) of nearest-neighbor tight-binding models in one, two, and three dimensions. Students use an AI coding agent to design and implement a numerical DOS calculation, validate it against known analytic structure, and then investigate the limits of a simple histogram method near a Van Hove singularity.

The standard route should stay simple: dense k-mesh sampling plus histogram. The advanced challenge should ask students to improve the 2D Van Hove singularity calculation using a constant-energy contour line-integral method and adaptive mesh refinement.

## Repository Structure

Add a new exercise subdirectory:

```text
exercises/
  tight_binding_dos/
    README.md
```

Update the top-level `README.md` to link to the new exercise.

Do not add solution code, fixed project files, generated datasets, or figures.

## Physical Model

Use the nearest-neighbor tight-binding model on the hypercubic lattice in dimensions `d = 1, 2, 3`.

Use:

- lattice constant `a = 1`
- hopping `t = 1`
- Brillouin zone `k_i in [-pi, pi)`
- dispersion

```text
E(k) = -2t sum_{i=1}^d cos(k_i)
```

The band range is:

```text
E in [-2dt, 2dt]
```

The exercise should compare how the DOS changes from 1D to 2D to 3D.

## Standard Implementation Route

The standard route is Python with `uv`, consistent with the Ising exercise.

Students should ask the AI agent to create a minimal project only when they work through the exercise. The repository README should not include generated code or a fixed `pyproject.toml`.

Plotting should use matplotlib.

## Data Policy

Use JSON only.

Save processed DOS data and metadata, not the full raw list of sampled energies by default.

A typical JSON output should contain:

- model name
- dimension
- hopping `t`
- mesh size per dimension
- number of bins
- energy range
- normalization convention
- energy bin centers
- DOS values
- bin width
- checks such as approximate normalization

The standard output should be suitable for regenerating DOS plots without rerunning the k-mesh sampling.

## Standard Workflow

The student-facing README should be step-by-step, with one prompt to the AI agent and confirmation items per step.

Recommended steps:

1. Design the model, DOS definition, JSON format, and validation checks before coding.
2. Create a minimal Python project with `uv`.
3. Implement the dispersion for dimensions 1, 2, and 3.
4. Generate dense k-mesh samples and histogram DOS.
5. Save processed DOS data and metadata as JSON.
6. Analyze saved JSON and plot DOS for 1D, 2D, and 3D.
7. Validate normalization and band edges.
8. Compare 1D numerical DOS with the analytic 1D DOS.
9. Observe the 2D Van Hove feature near `E = 0` qualitatively.
10. Attempt the advanced 2D Van Hove challenge.

## Required Validation

Required checks:

- DOS normalization:

```text
sum_i rho(E_i) Delta E approximately equals 1
```

- Bandwidth:

```text
E_min approximately -2dt
E_max approximately  2dt
```

- 1D analytic DOS comparison:

```text
rho_1d(E) = 1 / (pi sqrt(4t^2 - E^2))
```

for `|E| < 2t`.

Students should not compare exactly at the 1D band edges, where the analytic DOS diverges. They should compare over a finite interior energy window and study convergence with mesh size and bin count.

## Van Hove Observation

In the standard route, the 2D DOS should show an enhanced feature near `E = 0`.

This is a qualitative observation only. A dense-mesh histogram can show the Van Hove feature, but it should not be treated as an accurate resolution of the singularity.

## Advanced Challenge

The advanced challenge focuses on resolving the 2D Van Hove singularity more carefully.

For the 2D square-lattice dispersion,

```text
E(k_x, k_y) = -2t (cos k_x + cos k_y)
```

the Van Hove singularity occurs at saddle points such as `(0, pi)` and `(pi, 0)`, where `grad E = 0` and `E = 0`.

The README should ask students to investigate:

1. Constant-energy contour / line-integral method

```text
rho(E) = integral_{E(k)=E} dl / ((2pi)^2 |grad E(k)|)
```

This method directly uses the geometry of constant-energy contours. It is delicate near saddle points because `|grad E|` becomes small.

2. Adaptive mesh refinement

Refine regions where the DOS changes rapidly, where a target energy contour passes through a cell, or where `|grad E(k)|` is small. Use the refinement to concentrate computation near the Van Hove region.

The advanced task should ask students to compare an advanced method with the dense-mesh histogram and explain why the advanced method is more appropriate near `E = 0`.

## Advanced Verification Near E = 0

For the 2D square-lattice nearest-neighbor model, the analytic DOS can be written using the complete elliptic integral:

```text
rho_2d(E) = 1 / (2 pi^2 t) K(1 - (E / 4t)^2)
```

for `|E| < 4t`, where `K` is the complete elliptic integral of the first kind.

Near `E = 0`, the singularity is logarithmic:

```text
rho_2d(E) ~ 1 / (2 pi^2 t) log(16t / |E|)
```

The README should instruct students not to compare exactly at `E = 0`. Instead, they should:

- compare numerical DOS with the analytic elliptic-integral expression away from exactly `E = 0`
- inspect or fit `rho(E)` against `log(1 / |E|)` in a finite window
- exclude the central bin containing `E = 0` for histogram-based checks
- check symmetry `rho(E) = rho(-E)`
- check normalization over the full band
- check convergence with mesh size, bin width, contour resolution, or refinement threshold

## Points to Audit

The exercise should emphasize scientific judgment over producing a smooth plot.

Audit points:

- Is the DOS normalization convention clear?
- Does the histogram use bin centers and bin widths consistently?
- Are the band edges correct for each dimension?
- Is the 1D analytic comparison done away from divergent band edges?
- Is the 2D Van Hove feature treated as qualitative unless an advanced method is used?
- Are plots generated from saved JSON files?
- Are mesh size and bin count convergence checked before making a claim?
- Does the advanced Van Hove analysis compare against analytic behavior rather than only against another numerical method?

## Completion Criteria

The exercise is complete when students can show:

- a Python implementation created through AI-agent interaction
- JSON DOS output for dimensions 1, 2, and 3
- plots generated from saved JSON files
- normalization checks
- band-edge checks
- 1D analytic DOS comparison
- qualitative observation of the 2D Van Hove feature near `E = 0`
- a written statement of what the histogram method can and cannot justify
- for the advanced challenge, comparison to the analytic 2D Van Hove behavior

## Non-Goals

The initial exercise material should not include:

- completed source code
- precomputed DOS data
- precomputed plots
- a fixed Python project
- HDF5 or NumPy binary outputs as required formats
- a mandatory implementation of KPM, tetrahedron method, or Green's-function broadening

These may be discussed later, but the intended advanced methods for this exercise are the 2D line-integral method and adaptive mesh refinement.
