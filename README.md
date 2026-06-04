# Agentic Scientific Coding

This repository contains teaching material for B4/M1 students who are beginning to use programming and AI agents for scientific computing.

The course text is a Quarto book. Manuscripts are in [`book/*.qmd`](book/).

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

Download or clone the whole repository. If you use an AI agent, start it from the repository root so it can read the course text, exercise files, and project structure together.

Small examples use Julia by default. Install Julia through `juliaup` using the official instructions:

- https://julialang.org/install/

Jupyter notebooks are optional. The Julia REPL and `.jl` scripts are enough for the basic exercises.

## Current contents

- [Quarto book manuscripts](book/)
- [Ising MCMC project exercise](exercises/ising_mcmc/)
- [Tight-Binding Density of States project exercise](exercises/tight_binding_dos/)

Older Markdown drafts are preserved under [`backups/`](backups/).
