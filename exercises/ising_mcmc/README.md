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

## First Conversation With the AI Agent

Before asking for code, ask the AI agent to discuss the numerical design.

A good first instruction is:

```text
I want to solve the 2D ferromagnetic Ising model with single-spin Metropolis MCMC.
Before writing code, help me design the method, saved JSON format, unit tests, and validation checks.
Use Python with uv, keep the project minimal, and separate simulation from analysis.
```

The first discussion should cover:

- how the Metropolis update works,
- how to compute the local energy change `Delta E`,
- what one sweep means,
- which quantities to measure,
- what metadata must be saved,
- which unit tests should be added,
- how to validate the result.

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

## Common Failure Modes

Ask the AI agent what could go wrong, then check the dangerous parts.

Common mistakes include:

- wrong sign in `Delta E`,
- incorrect periodic boundary conditions,
- double-counting or under-counting bonds in the energy,
- mixing up `T` and `1 / T`,
- treating magnetization and absolute magnetization as the same quantity,
- including discarded sweeps in final averages,
- treating strongly correlated samples as independent,
- saving too little metadata to reproduce the run,
- computing Binder quantities from averaged magnetization rather than from the magnetization time series.

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
