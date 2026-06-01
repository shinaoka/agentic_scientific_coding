# Exercise: Tight-Binding Density of States

## Goal

Use an AI coding agent to compute and analyze the density of states (DOS) of nearest-neighbor tight-binding models in one, two, and three dimensions.

The standard calculation uses a dense k-mesh and a histogram. This is intentionally simple: the purpose is to learn how DOS depends on dimension, how to validate a numerical DOS, and where a simple histogram becomes insufficient.

By the end of this exercise, you should have:

- a Python implementation created with help from an AI agent,
- JSON files containing processed DOS data and metadata,
- plots of the 1D, 2D, and 3D DOS generated from saved JSON files,
- normalization and bandwidth checks,
- a comparison with the analytic 1D DOS,
- a qualitative observation of the 2D Van Hove feature near `E = 0`,
- an advanced investigation of the 2D Van Hove singularity using more accurate methods.

## Physical Problem

Study the nearest-neighbor tight-binding model on hypercubic lattices in dimensions `d = 1, 2, 3`.

Use:

- lattice constant `a = 1`
- hopping `t = 1`
- Brillouin zone `k_i in [-pi, pi)`
- dimensions `d = 1, 2, 3`

The dispersion is:

```text
E(k) = -2t sum_{i=1}^d cos(k_i)
```

The band range is:

```text
E in [-2dt, 2dt]
```

The numerical task is to approximate the DOS:

```text
rho(E) = integral_BZ d^d k / (2pi)^d delta(E - E(k))
```

with the normalization:

```text
integral rho(E) dE = 1
```

## Standard Route

Use Python as the standard implementation language.

Ask your AI agent to use `uv` to create a minimal Python project. The exact project structure is part of the exercise.

Use matplotlib for standard plotting.

Use JSON as the saved-data format. Save processed DOS data and metadata, not the full raw list of sampled energies by default.

Do not rely on plots created directly inside the sampling script. The DOS calculation should save data, and the analysis should read saved data.

## Step-by-Step Workflow

Work through the exercise in order. At each step, ask the AI agent for help, then check the result before moving on.

The prompts below are starting points. You may edit them, but do not skip the confirmation items.

### Step 1: Design the Calculation Before Coding

Prompt to the AI agent:

```text
I want to compute the density of states of nearest-neighbor tight-binding models in 1D, 2D, and 3D.
Before writing code, help me design the k-mesh sampling, histogram normalization, saved JSON format, and validation checks.
Use Python with uv, keep the project minimal, and separate DOS generation from analysis.
```

Confirm before moving on:

- You can explain the dispersion `E(k) = -2t sum_i cos(k_i)`.
- You know the expected band range `[-2dt, 2dt]`.
- You have chosen a DOS normalization convention.
- You have chosen what processed data to save in JSON.
- You have a validation plan beyond just plotting smooth curves.

### Step 2: Create the Minimal Python Project

Prompt to the AI agent:

```text
Create a minimal Python project using uv for this exercise.
Do not over-engineer the structure.
Include separate entry points or scripts for DOS generation, analysis, and tests or checks.
Do not implement the full calculation yet; first set up the project so commands can run from the command line.
```

Confirm before moving on:

- The project can be run with `uv`.
- The intended DOS-generation and analysis commands are clear.
- The project contains no unnecessary framework or large scaffold.

### Step 3: Implement the Dispersion

Prompt to the AI agent:

```text
Implement the nearest-neighbor hypercubic tight-binding dispersion for dimensions d = 1, 2, and 3:
E(k) = -2t sum_i cos(k_i).
Add small deterministic checks for high-symmetry k-points and expected band edges.
```

Confirm before moving on:

- `E(k = 0) = -2dt`.
- `E(k_i = pi for all i) = 2dt`.
- The implementation works for `d = 1`, `d = 2`, and `d = 3`.
- The hopping `t` is handled consistently.

### Step 4: Generate Dense k-Mesh Histogram DOS

Prompt to the AI agent:

```text
Implement dense k-mesh sampling and histogram DOS estimation for d = 1, 2, and 3.
Use a mesh size per dimension and a chosen number of energy bins.
Normalize the histogram so that the integral of rho(E) over energy is approximately 1.
```

Confirm before moving on:

- The bin width is defined explicitly.
- The histogram uses bin centers consistently.
- The approximate DOS integral is close to 1.
- The sampled energy range is consistent with `[-2dt, 2dt]`.

### Step 5: Save Processed DOS Data as JSON

Prompt to the AI agent:

```text
Save the processed DOS data as JSON.
Do not save the full raw list of sampled energies by default.
Include metadata such as dimension, hopping, mesh size per dimension, number of bins, energy range, bin width, and normalization check.
```

Confirm before moving on:

- One JSON file is created for each dimension or parameter set.
- The JSON contains enough metadata to understand the calculation.
- The JSON contains energy bin centers and DOS values.
- The analysis can regenerate plots from saved JSON without rerunning the k-mesh sampling.

### Step 6: Plot DOS From Saved JSON

Prompt to the AI agent:

```text
Write an analysis script that reads saved JSON files and plots the DOS for d = 1, 2, and 3.
The analysis must not silently rerun the k-mesh sampling.
```

Confirm before moving on:

- The analysis reads JSON files produced by the DOS-generation step.
- The analysis does not silently regenerate the sampled energies.
- The 1D, 2D, and 3D DOS can be plotted on comparable axes.
- The plots can be regenerated from saved data.

### Step 7: Validate Normalization and Band Edges

Prompt to the AI agent:

```text
Add validation checks for DOS normalization and band edges.
For each dimension d, check that the DOS integral is close to 1 and that the band lies in [-2dt, 2dt].
```

Confirm before moving on:

- `sum_i rho(E_i) Delta E` is close to 1.
- The numerical band edges match `[-2dt, 2dt]` within the expected mesh and bin resolution.
- Deviations are explained in terms of mesh spacing or binning, not ignored.

### Step 8: Compare With the Analytic 1D DOS

Prompt to the AI agent:

```text
Compare the numerical 1D DOS with the analytic nearest-neighbor 1D tight-binding DOS:
rho(E) = 1 / (pi sqrt(4t^2 - E^2)), for |E| < 2t.
Avoid comparing exactly at the divergent band edges.
```

Confirm before moving on:

- The comparison is done inside the band, away from `E = +/- 2t`.
- The numerical DOS approaches the analytic curve as mesh size and bin resolution are improved.
- The divergent 1D band-edge behavior is described correctly.

### Step 9: Observe the 2D Van Hove Feature

Prompt to the AI agent:

```text
Use the dense-mesh histogram results to inspect the 2D DOS near E = 0.
Explain why the enhanced feature near E = 0 is associated with a Van Hove singularity, but do not claim the simple histogram accurately resolves the singularity.
```

Confirm before moving on:

- The 2D DOS shows an enhanced feature near `E = 0`.
- You can state why this is only a qualitative observation with the histogram method.
- You have checked that the feature changes with bin width or mesh size.

### Step 10: Attempt the Advanced Van Hove Challenge

Prompt to the AI agent:

```text
Help me investigate the 2D Van Hove singularity more accurately.
Compare the constant-energy contour line-integral method and adaptive mesh refinement.
Choose an implementation strategy, compare it with the dense-mesh histogram, and verify the result against the known analytic 2D DOS near E = 0.
```

Confirm before making a claim about the singularity:

- The method is compared with the analytic 2D DOS away from exactly `E = 0`.
- The logarithmic behavior near `E = 0` is checked over a finite energy window.
- The result is symmetric under `E -> -E`.
- The full DOS remains normalized.
- Resolution, contour discretization, or refinement-threshold dependence is discussed.

## Saved JSON Data

Every standard DOS run should save processed DOS data as JSON.

A minimal structure is:

```json
{
  "metadata": {
    "model": "nearest_neighbor_tight_binding",
    "lattice": "hypercubic",
    "dimension": 2,
    "t": 1.0,
    "mesh_size_per_dimension": 800,
    "n_bins": 400,
    "energy_range": [-4.0, 4.0],
    "bin_width": 0.02,
    "normalization": "integral_rho_dE_equals_1",
    "dos_integral": 0.9998
  },
  "dos": {
    "energy_bin_centers": [-3.99, -3.97, -3.95],
    "density": [0.01, 0.02, 0.03]
  }
}
```

This example is a data-format illustration only. It is not expected output.

For the standard route, do not save the full raw sampled-energy array unless you have a specific reason.

## Analysis Requirements

Write analysis code separately from DOS-generation code.

The analysis should:

- read saved JSON files,
- plot DOS curves,
- compare dimensions `d = 1, 2, 3`,
- show normalization and band-edge checks,
- compare the 1D result with the analytic formula,
- describe the 2D Van Hove feature qualitatively.

The analysis should not rerun the k-mesh sampling unless you explicitly ask it to do so as a separate step.

## Required Validation

Perform the following checks before trusting the DOS.

### Normalization

Check:

```text
sum_i rho(E_i) Delta E approximately equals 1
```

This is the most basic check that the DOS normalization is consistent.

### Band Edges

For dimension `d`, check:

```text
E_min approximately -2dt
E_max approximately  2dt
```

The agreement is limited by k-mesh and bin resolution.

### Analytic 1D DOS

For `d = 1`, compare with:

```text
rho_1d(E) = 1 / (pi sqrt(4t^2 - E^2))
```

for `|E| < 2t`.

Do not compare exactly at `E = +/- 2t`, where the analytic DOS diverges.

## Van Hove Observation

For `d = 2`, the square-lattice DOS has a Van Hove singularity near `E = 0`.

The dense-mesh histogram should show an enhanced feature near this energy. Treat this as a qualitative observation. A simple histogram is sensitive to mesh size and bin width, so it should not be used alone to make a precise statement about the singularity.

## Points to Audit

Modern AI agents can usually produce plausible DOS code. The harder part is auditing the conventions and numerical claims.

Audit points include:

- whether the DOS normalization convention is explicit,
- whether bin centers and bin widths are used consistently,
- whether the band edges are correct for each dimension,
- whether the 1D analytic comparison avoids the divergent band edges,
- whether the 2D Van Hove feature is treated as qualitative in the standard route,
- whether plots are generated from saved JSON files,
- whether mesh size and bin count convergence are checked before making a claim,
- whether the advanced Van Hove analysis is compared with analytic behavior, not only with another numerical method.

## Advanced Challenge: 2D Van Hove Singularity

The standard histogram calculation can show the Van Hove feature, but it does not accurately resolve the singularity.

For the 2D square-lattice dispersion:

```text
E(k_x, k_y) = -2t (cos k_x + cos k_y)
```

the Van Hove singularity comes from saddle points such as `(0, pi)` and `(pi, 0)`, where `grad E = 0` and `E = 0`.

Investigate the following advanced methods.

### Constant-Energy Line Integral

In 2D, the DOS can be written as an integral over constant-energy contours:

```text
rho(E) = integral_{E(k)=E} dl / ((2pi)^2 |grad E(k)|)
```

This method directly uses the geometry of constant-energy contours. It is delicate near saddle points because `|grad E|` becomes small.

### Adaptive Mesh Refinement

Use adaptive refinement to concentrate computational effort near difficult regions.

Possible refinement criteria:

- cells crossed by a target energy contour,
- cells near `E = 0`,
- regions where `|grad E(k)|` is small,
- regions where the DOS estimate changes rapidly with resolution.

### Verification Near `E = 0`

For the 2D square-lattice nearest-neighbor model, the analytic DOS can be written using the complete elliptic integral:

```text
rho_2d(E) = 1 / (2 pi^2 t) K(1 - (E / 4t)^2)
```

for `|E| < 4t`, where `K` is the complete elliptic integral of the first kind.

Near `E = 0`, the singularity is logarithmic:

```text
rho_2d(E) ~ 1 / (2 pi^2 t) log(16t / |E|)
```

Do not compare exactly at `E = 0`. Instead:

- compare against the analytic expression in a finite energy window,
- inspect or fit `rho(E)` against `log(1 / |E|)`,
- exclude the central bin containing `E = 0` for histogram-based checks,
- check symmetry `rho(E) = rho(-E)`,
- check normalization over the whole band,
- check convergence with mesh size, bin width, contour resolution, or refinement threshold.

## Completion Criteria

The exercise is complete when you can show:

- the AI-assisted implementation runs from the command line,
- the Python environment was created using `uv`,
- DOS generation and analysis are separate,
- processed DOS data are saved as JSON,
- analysis reads saved JSON files,
- DOS curves for `d = 1, 2, 3` were generated,
- normalization was checked,
- band edges were checked,
- the 1D analytic DOS comparison was performed,
- the 2D Van Hove feature was observed qualitatively,
- the limitations of dense-mesh histograms were stated,
- the advanced Van Hove challenge was attempted or the reason for not attempting it was recorded.
