---
layout: setup
title: Setup
---

Before starting the workshop, complete the setup below. Make sure the required
software is installed, that you can access your GitHub account, and that each
verification command runs successfully.

<div class="setup-controls" aria-label="Setup section controls">
  <button type="button" data-setup-toggle="expand">Expand all</button>
  <button type="button" data-setup-toggle="collapse">Collapse all</button>
</div>

<details class="setup-section" markdown="1">
<summary><input class="setup-checkbox" type="checkbox" data-setup-item="git" aria-label="Mark git as complete"> <h2>git</h2></summary>

`git` is the version control system we will use throughout the workshop. It lets
you track changes, save checkpoints, and collaborate through GitHub.

### Install git

Download `git` from: <https://git-scm.com/downloads>

Run the installer. Windows users can accept the default installation options.

### Verify git

On macOS or Linux, open the Terminal application and run:

```shell
git --version
```

You should see the installed git version.

On Windows, open **Git Bash** from the Start menu and run:

```shell
git --version
```

</details>

<details class="setup-section" markdown="1">
<summary><input class="setup-checkbox" type="checkbox" data-setup-item="terminal" aria-label="Mark Terminal as complete"> <h2>Command line</h2></summary>

The **command line** is the text-based way of giving instructions to your
computer. Instead of clicking buttons, you type commands such as `pwd`, `ls`, or
`git status`.

Windows and Unix-based systems use different command-line conventions. macOS
and Linux use a Unix-style command line. In this workshop, we will learn the
Unix-style command line, which is why Windows users should use Git Bash rather
than the standard Windows Command Prompt.

Use:

- **macOS/Linux:** the built-in Terminal application
- **Windows:** Git Bash

### Verify the terminal

Run:

```shell
pwd
ls
```

Both commands should run successfully.

</details>

<details class="setup-section" markdown="1">
<summary><input class="setup-checkbox" type="checkbox" data-setup-item="github-and-ssh" aria-label="Mark GitHub and SSH as complete"> <h2>GitHub and SSH</h2></summary>

We will use GitHub to share repositories, collaborate, and submit changes.

If you do not already have a GitHub account, create one at: <https://github.com>.

If you are a student or educator, you may also be eligible for GitHub
Education: <https://education.github.com>.

### Configure SSH

SSH lets your computer communicate securely with GitHub. You will need to generate
an SSH key if you don't already have one and then connect it to your GitHub account.
See this guide for instructions: <https://docs.github.com/en/free-pro-team@latest/authentication/connecting-to-github-with-ssh>.


Verify your configuration:

```shell
ssh -T git@github.com
```

If the connection works, GitHub should recognise your account.

</details>

<details class="setup-section" markdown="1">
<summary><input class="setup-checkbox" type="checkbox" data-setup-item="editor" aria-label="Mark Text editor as complete"> <h2>Text editor</h2></summary>

We recommend **Visual Studio Code (VS Code)**, although you are welcome to use
any editor or IDE that you are comfortable with.

### Install VS Code

Download VS Code from: <https://code.visualstudio.com>.

### Install useful extensions

Open VS Code, go to the Extensions view, and install:

- **Python**, published by Microsoft
- **LaTeX Workshop**, for editing and compiling `LaTeX` documents

</details>

<details class="setup-section" markdown="1">
<summary><input class="setup-checkbox" type="checkbox" data-setup-item="python" aria-label="Mark Python and packages as complete"> <h2>Python and packages</h2></summary>

Install the latest version of Python 3 from: <https://www.python.org/downloads/>.

Windows users should check **Add Python to PATH** before clicking **Install**.

### Verify Python

Run:

```shell
python --version
```

or:

```shell
python3 --version
```

### Install the required packages

The workshop uses Python packages for data analysis, plotting, and Dask
parallel computing.

Run:

```shell
pip install numpy pandas matplotlib seaborn dask distributed
```

or:

```shell
pip3 install numpy pandas matplotlib seaborn dask distributed
```

### Verify the packages

Run:

```shell
python -c "import numpy, pandas, matplotlib, seaborn, dask, distributed; print('Everything is installed!')"
```

If `python` is not recognised, replace it with `python3`.

</details>

<details class="setup-section" markdown="1">
<summary><input class="setup-checkbox" type="checkbox" data-setup-item="latex" aria-label="Mark LaTeX as complete"> <h2>LaTeX</h2></summary>

To compile the `LaTeX` exercises, install a `LaTeX` distribution.

Recommended distributions:

- **Windows:** MiKTeX: <https://miktex.org/howto/install-miktex>.
- **macOS:** MacTeX: <https://www.tug.org/mactex/mactex-download.html>.
  - Homebrew users can install it with: <https://formulae.brew.sh/cask/mactex>.
- **Linux:** TeX Live: <https://tug.org/texlive/quickinstall.html>.

A full `LaTeX` distribution is large (typically 1–4 GB), so the installation
may take a few minutes.

### Verify LaTeX

Run:

```shell
pdflatex --version
```

```shell
{
   "latex-workshop.latex.tools": [
       {
           "name": "pdflatex",
           "command": "pdflatex",
           "args": [
               "-synctex=1",
               "-interaction=nonstopmode",
               "-file-line-error",
               "%DOC%"
           ]
       },
       {
           "name": "bibtex",
           "command": "bibtex",
           "args": [
               "%DOCFILE%"
           ]
       }
   ],

   "latex-workshop.latex.recipes": [
       {
           "name": "pdflatex",
           "tools": [
               "pdflatex"
           ]
       },
       {
           "name": "pdflatex + bibtex",
           "tools": [
               "pdflatex",
               "bibtex",
               "pdflatex",
               "pdflatex"
           ]
       }
   ],

   "latex-workshop.latex.recipe.default": "pdflatex"
}
```

</details>


<details class="setup-section" markdown="1">
<summary><input class="setup-checkbox" type="checkbox" data-setup-item="checklist" aria-label="Mark Final checklist as complete"> <h2>Final checklist</h2></summary>

Before the workshop, make sure these commands work:

```shell
git --version
python --version
python -c "import numpy, pandas, matplotlib, seaborn, dask, distributed"
ssh -T git@github.com
pdflatex --version
```

If your system uses `python3` instead of `python`, run:

```shell
python3 --version
python3 -c "import numpy, pandas, matplotlib, seaborn, dask, distributed"
```

</details>

### Troubleshooting

If something does not work, make a note of:

* the command you ran
* the error message
* your operating system

If you are attending an in-person workshop, bring this information with you.
There will always be a pre-installation session where we can help you resolve
any issues.

If you are working through the material on your own, open an issue in the
[workshop repository](https://github.com/Nikoleta-v3/computational-methods-scientific-workflows/) and include the information above.