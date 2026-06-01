# Exercise: Ising MCMC

## Goal

Use an AI coding agent to design, implement, test, validate, and analyze a Markov-chain Monte Carlo simulation of the two-dimensional Ising model.

The goal is not to obtain a polished reference code. The goal is to learn how to make an AI-generated numerical result inspectable and scientifically defensible.

By the end of this exercise, you should have:

- a Python implementation created with help from an AI agent,
- saved simulation output in JSON format,
- an analysis script that reads saved JSON files,
- minimal unit tests for deterministic implementation details,
- validation against simple physical limits and a very small exact system,
- a final challenge using the Binder parameter to estimate the transition temperature.

## Physical Problem

Study the ferromagnetic Ising model on a two-dimensional square lattice:

```text
H = -J sum_<ij> s_i s_j
```

where each spin is `s_i = +1` or `-1`, and the sum is over nearest-neighbor pairs. Use periodic boundary conditions.

For the standard exercise, use:

- `J = 1`
- `k_B = 1`
- square lattice size `L x L`
- temperature `T`
- single-spin Metropolis updates

One sweep should mean `L * L` attempted single-spin updates.

## Standard Route

Use Python as the standard implementation language.

Ask your AI agent to use `uv` to create a minimal Python project. The exact project structure is part of the exercise; do not begin with a fixed template.

Use matplotlib for standard plotting.

Use JSON as the saved-data format.

Do not rely on plots created directly inside the simulation. The simulation should save data, and the analysis should read saved data.

## Step-by-Step Workflow

Work through the exercise in order. At each step, ask the AI agent for help, then check the result before moving on.

The prompts below are starting points. You may edit them, but do not skip the confirmation items.

### Step 1: Design the Calculation Before Coding

Prompt to the AI agent:

```text
I want to solve the 2D ferromagnetic Ising model with single-spin Metropolis MCMC.
Before writing code, help me design the method, saved JSON format, minimal unit tests, and validation checks.
Use Python with uv, keep the project minimal, and separate simulation from analysis.
```

Confirm before moving on:

- You can explain the Metropolis acceptance rule.
- You know what one sweep means.
- You have chosen what to save in JSON.
- You have a list of minimal unit tests.
- You have a validation plan beyond just plotting results.

### Step 2: Create the Minimal Python Project

Prompt to the AI agent:

```text
Create a minimal Python project using uv for this exercise.
Do not over-engineer the structure.
Include separate entry points or scripts for simulation, analysis, and tests.
Do not write the full simulation yet; first set up the project so commands can run from the command line.
```

Confirm before moving on:

- The project can be run with `uv`.
- The intended simulation and analysis commands are clear.
- The project contains no unnecessary framework or large scaffold.

### Step 3: Implement Deterministic Physics Functions First

Prompt to the AI agent:

```text
Implement the deterministic parts first: spin representation, periodic neighbors, total energy, magnetization, and local Delta E for a proposed spin flip.
Also create minimal unit tests for these functions before implementing the full Monte Carlo loop.
```

Confirm before moving on:

- Energy is correct for simple configurations.
- Magnetization normalization is clear.
- Periodic boundaries are tested.
- Local `Delta E` agrees with full energy recomputation.
- The tests run successfully.

### Step 4: Implement One Metropolis Run

Prompt to the AI agent:

```text
Implement a single-temperature, single-seed Metropolis simulation.
Use one sweep = L * L attempted spin updates.
Save the energy and magnetization time series plus metadata to one JSON file.
Keep simulation and analysis separate.
```

Confirm before moving on:

- One command runs the simulation.
- One JSON file is created.
- The JSON contains `L`, `T`, sweeps, discarded sweeps, measurement interval, seed, boundary condition, and measured time series.
- Re-running with the same seed gives the same short trajectory.

### Step 5: Analyze Saved JSON

Prompt to the AI agent:

```text
Write an analysis script that reads saved JSON files.
It should not rerun the simulation.
Plot energy and magnetization time series, discard the requested initial sweeps, and compute post-discard averages.
```

Confirm before moving on:

- The analysis reads JSON files produced by the simulation.
- The analysis does not silently run the simulation again.
- The discarded sweeps are handled consistently.
- The generated plots and averages can be regenerated from saved data.

### Step 6: Check Low and High Temperatures

Prompt to the AI agent:

```text
Help me run and analyze one low-temperature case and one high-temperature case.
Use the saved JSON files to compare energy, magnetization, absolute magnetization, and time-series behavior.
```

Confirm before moving on:

- Low temperature shows larger absolute magnetization than high temperature.
- Energy behaves differently at low and high temperature.
- The early part of the time series is inspected rather than blindly averaged.
- Any unexpected behavior is investigated before continuing.

### Step 7: Validate With Exact Enumeration

Prompt to the AI agent:

```text
Add exact enumeration for a very small lattice, such as L = 2.
Use the same Hamiltonian, boundary convention, energy normalization, and magnetization definition as the MCMC code.
Compare exact thermal averages with MCMC estimates.
```

Confirm before moving on:

- The exact enumeration uses the same conventions as the simulation.
- MCMC estimates are compared with exact values at one or more temperatures.
- Disagreement is discussed in terms of sampling error, run length, correlations, or possible implementation mistakes.

### Step 8: Compare Independent Seeds

Prompt to the AI agent:

```text
Run the same parameters with several independent random seeds.
Analyze the saved JSON files together and compare the qualitative behavior and averaged observables.
```

Confirm before moving on:

- Different seeds do not produce identical trajectories.
- Independent seeds give compatible qualitative behavior.
- Strong disagreement is investigated before making physical claims.

### Step 9: Compare Several Temperatures

Prompt to the AI agent:

```text
Run a small set of temperatures: one low temperature, one near the expected transition region, and one high temperature.
Analyze only the saved JSON files and compare the temperature dependence of energy and absolute magnetization.
```

Confirm before moving on:

- The same saved-data format is used for every temperature.
- The analysis can compare multiple JSON files.
- You can explain the observed temperature dependence qualitatively.

### Step 10: Attempt the Binder-Parameter Challenge

Prompt to the AI agent:

```text
Using the existing JSON-based workflow, help me estimate the transition temperature from Binder-parameter crossings.
Run several lattice sizes and temperatures, compute U_L(T) = 1 - <m^4> / (3 <m^2>^2), and plot Binder curves from saved JSON files.
```

Confirm before making a transition-temperature claim:

- Each Markov chain has plausibly forgotten its initial condition.
- Correlations are considered when quoting Binder estimates.
- Independent seeds give compatible Binder curves.
- Binder crossing estimates become more stable as larger lattice sizes are included.
- The temperature grid is fine enough near the crossing region.

## Method Design

The standard algorithm is single-spin Metropolis.

For each proposed update:

1. Choose one spin.
2. Compute the energy change `Delta E` if that spin is flipped.
3. Accept the flip if `Delta E <= 0`.
4. If `Delta E > 0`, accept with probability `exp(-Delta E / T)`.

Discuss these choices with the AI agent before implementation:

- random sequential updates or shuffled sweeps,
- initial condition, such as all-up or random spins,
- number of sweeps,
- number of discarded sweeps,
- measurement interval,
- random seed policy,
- energy and magnetization normalization.

Use a simple method first. Cluster algorithms are useful, but they are not part of the standard route for this exercise.

## Saved JSON Data

Every simulation run should save one JSON file.

The file must contain enough information to inspect the run without reading the source code. Use clear field names and record the units or normalization convention.

A minimal structure is:

```json
{
  "metadata": {
    "model": "2d_ising",
    "algorithm": "single_spin_metropolis",
    "boundary_condition": "periodic",
    "L": 16,
    "J": 1.0,
    "temperature": 2.3,
    "n_sweeps": 10000,
    "discarded_sweeps": 2000,
    "measurement_interval": 1,
    "seed": 12345,
    "energy_convention": "per_spin",
    "magnetization_convention": "per_spin"
  },
  "measurements": {
    "sweep": [0, 1, 2],
    "energy_per_spin": [-1.1, -1.2, -1.3],
    "magnetization_per_spin": [0.1, 0.2, 0.3]
  }
}
```

This example is a data-format illustration only. It is not expected output.

For larger temperature scans, use one JSON file per run or a simple directory of JSON files. Keep the format inspectable.

## Analysis Requirements

Write analysis code separately from simulation code.

The analysis should:

- read saved JSON files,
- discard the requested initial sweeps during analysis,
- plot energy and magnetization time series,
- compute averages after discarding initial sweeps,
- compare low-temperature and high-temperature behavior,
- generate figures with matplotlib.

The analysis should not rerun the simulation unless you explicitly ask it to do so as a separate step.

## Minimal Unit Tests

Ask the AI agent to create minimal unit tests for the implementation before relying on the simulation results.

The tests should focus on small, deterministic checks, not on reproducing full Monte Carlo behavior.

Suggested tests:

- energy for simple spin configurations,
- magnetization normalization,
- periodic-boundary neighbor indexing,
- local `Delta E` compared with full energy recomputation,
- reproducibility of a short run with a fixed random seed.

Passing unit tests does not validate the physics result. Use unit tests to catch implementation mistakes, then use physical and numerical validation to judge the simulation output.

## Required Validation

Perform both qualitative and quantitative validation.

### Qualitative Checks

Run at least one low-temperature case and one high-temperature case.

Check whether:

- low temperature tends to produce large absolute magnetization,
- high temperature tends to produce small magnetization,
- energy behaves differently at low and high temperature,
- early time-series behavior depends on the initial condition,
- discarded sweeps affect the measured averages.

Do not claim correctness only because the plots look plausible.

### Exact Enumeration for a Small Lattice

For a very small lattice, such as `L = 2`, ask the AI agent to implement exact enumeration.

The exact-enumeration check should:

- enumerate all spin configurations,
- compute the exact thermal averages for the same Hamiltonian and boundary convention,
- compare those averages with MCMC estimates,
- discuss finite sampling error and remaining disagreement.

Use the same definitions of energy, magnetization, and boundary conditions in both the simulation and exact enumeration.

### Independent Seeds

Run the same parameter set with multiple random seeds.

The time series should not be identical across seeds, but the qualitative temperature dependence should be compatible. If independent seeds disagree strongly, investigate run length, discarded sweeps, correlations, and possible implementation errors.

## Points to Audit

Modern AI agents often produce plausible implementations. The harder problem is checking whether the conventions, statistics, and claims are consistent.

Audit points include:

- whether energy and magnetization normalization are the same in simulation, analysis, tests, and exact enumeration,
- whether the periodic-boundary bond convention is defined clearly, especially for very small lattices,
- whether discarded sweeps are excluded in analysis, not just recorded in metadata,
- whether magnetization `m` and absolute magnetization `|m|` are used for different purposes deliberately,
- whether correlated MCMC samples are treated with appropriate caution,
- whether different temperatures, seeds, or lattice sizes are compared using compatible run lengths and measurement intervals,
- whether Binder quantities are computed from the magnetization time series, not from already averaged magnetization,
- whether the final claim is about finite-size numerical evidence rather than the thermodynamic limit itself.

## Optional Work Record

You may ask the AI agent to create a short work record.

The record should summarize decisions and evidence, not copy the full prompt history.

Useful entries include:

- methods considered,
- implementation choices,
- unit tests added,
- validation checks actually performed,
- failures or inconsistencies found,
- parameter changes made after validation,
- remaining concerns.

## Challenge: Binder-Parameter Estimate of the Transition Temperature

After the basic simulation and validation are working, estimate the transition temperature using the Binder parameter.

For magnetization per spin `m`, define:

```text
U_L(T) = 1 - <m^4> / (3 <m^2>^2)
```

Run simulations for several lattice sizes, such as:

```text
L = 8, 16, 32
```

Use temperatures around the expected transition region. For the square-lattice Ising model with `J = 1` and `k_B = 1`, the transition is near `T = 2.27`, but the exercise is to estimate it from finite-size data rather than simply quote the known value.

Suggested procedure:

1. Choose several lattice sizes.
2. Run simulations over a temperature grid near the transition region.
3. Save every run as JSON.
4. From saved JSON files, compute `<m^2>`, `<m^4>`, and `U_L(T)`.
5. Plot `U_L(T)` for several `L` values on the same graph.
6. Refine the temperature grid near the crossing region.
7. Increase `L` and check whether crossing estimates become more stable.
8. State what your finite-size calculation supports and what it does not establish.

Checks before claiming a transition temperature:

- Has each Markov chain forgotten its initial condition, and are correlations small enough for the Binder estimate being quoted?
- Do independent seeds give compatible Binder curves?
- Do Binder-parameter crossing points become stable as larger lattice sizes are included?
- Is the temperature grid fine enough near the crossing region?
- Are all Binder curves computed from saved JSON files rather than rerunning simulations during analysis?

## Extension: Speed Up With Rust or Julia

After completing the Python version, you may ask the AI agent to help speed up the simulation using Rust or Julia.

This is an extension, not a replacement for validation.

Keep the same:

- Hamiltonian,
- measured quantities,
- JSON output format,
- random seed policy where possible,
- validation checks,
- Binder-parameter analysis.

Compare runtime scaling with lattice size. For a fixed number of sweeps, the raw number of attempted updates grows like `L^2`. Near the transition region, the effective cost of obtaining stable averages may grow faster because samples are more strongly correlated.

## Completion Criteria

The exercise is complete when you can show:

- the AI-assisted implementation runs from the command line,
- the Python environment was created using `uv`,
- simulation and analysis are separate,
- simulation results are saved as JSON,
- analysis reads saved JSON files,
- minimal unit tests were created and run,
- low-temperature and high-temperature behavior were checked,
- exact enumeration was used for a very small lattice,
- independent seeds were compared,
- limitations of the result were stated,
- the Binder-parameter challenge was attempted or the reason for not attempting it was recorded.
