# Agentic Scientific Coding

This repository contains teaching material for B4/M1 physics majors and students
in related fields who are beginning to use programming and AI agents for
physics-oriented numerical computing.

The course text is a Quarto book. Manuscripts are in [`book/`](book/) as one `.qmd` file per subsection.

## Local preview

Install Quarto if needed, then run:

```bash
make serve
```

Build the static site with:

```bash
make build
```

The rendered site is written to `book/dist`.

## GitHub Pages

[`.github/workflows/pages.yml`](.github/workflows/pages.yml) is enabled for CI.

- Pull requests build the Quarto book and verify `book/dist/index.html`.
- Pushes to `main` build the book and deploy `book/dist` through GitHub Pages.
- Manual runs through `workflow_dispatch` also build and deploy.

In the repository settings, set Pages to use GitHub Actions as the source.

## For students

Start by downloading the ZIP archive:

- https://github.com/shinaoka/agentic_scientific_coding/archive/refs/heads/main.zip

The repository page is:

- https://github.com/shinaoka/agentic_scientific_coding

Unpack the ZIP file and work inside the top-level folder. If you already know Git, cloning the repository is also fine. You do not need to understand Git or repository management in detail at the beginning, but read [Minimal Git and diff safety](book/getting-started/minimal-git-diff-safety.qmd) before asking an AI agent to edit files.

If you use an AI agent, start it from the top-level course folder so it can read the course text, exercise files, and project structure together.

Small examples use Julia by default. Install Julia through `juliaup` using the official instructions:

- https://julialang.org/install/

Jupyter notebooks are optional. The Julia REPL and `.jl` scripts are enough for the basic exercises.

## Current contents

- [Quarto book manuscripts](book/)
- [Ising MCMC project exercise](exercises/ising_mcmc/)
- [Tight-Binding Density of States project exercise](exercises/tight_binding_dos/)

Older Markdown drafts are preserved under [`backups/`](backups/).
