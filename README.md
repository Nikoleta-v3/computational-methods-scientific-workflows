# Computational Methods for Scientific Workflows

Course material for **Computational Methods and Scientific Workflows**.

This course is designed for academic researchers who want to use the tools
around their code more effectively. It is not a general programming course. The
focus is on practical research workflows: using the command line, tracking
changes with git, sharing work through GitHub, organising projects, preparing
figures and tables, and writing reproducible scientific documents.

The material is organised around a small research project that grows over the
course.

## Course structure

- **Setup**: software, accounts, and local tools needed for the course.
- **Module 1**: software tools for computational research workflows.
- **Module 2**: scientific communication and reproducible research outputs.
- **Exercise**: an integrated workflow using the tools from both modules.

Day 1 focuses on software: the command line, git, GitHub, project organisation,
and parallel computing.

Day 2 focuses on communication: data visualisation, publication-ready tables,
and scientific writing with LaTeX.

## Material

The main pages are:

- [Setup](setup.md)
- [Module 1](module_one.md)
- [Module 2](module_two.md)
- [Exercise](exercise.md)
- [git cheat sheet](git_cheatsheet.md)

The detailed lesson pages live in the [`tabs/`](tabs/) folder.

## Local development

This site is built with Jekyll.

Install the Ruby dependencies:

```shell
bundle install
```

Build the site:

```shell
bundle exec jekyll build
```

Serve the site locally:

```shell
bundle exec jekyll serve
```

The generated site is written to `_site/`.

