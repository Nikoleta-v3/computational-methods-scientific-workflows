---
layout: setup
title: Setup
---

Before starting the workshop, complete the setup below. Make sure all required
software is installed, that you can access your GitHub account, and that each
verification command runs successfully.

<div class="setup-controls" aria-label="Setup section controls">
  <button type="button" data-setup-toggle="expand">Expand all</button>
  <button type="button" data-setup-toggle="collapse">Collapse all</button>
</div>

<details class="setup-section" markdown="1">
<summary><input class="setup-checkbox" type="checkbox" data-setup-item="git" aria-label="Mark Git as complete"> <h2>Git</h2></summary>

Git is the version control system that you will use throughout the workshop.

### Installation

1. Download Git from <https://git-scm.com/downloads>.
2. Run the installer.

Windows users can accept the default installation options.

### Verify the installation

Open a terminal and run:

```shell
git --version
```

You should see the installed Git version.

### Windows

After installing Git:

1. Open **Git Bash** from the Start menu.
2. Run:

```shell
git --version
```

</details>

<details class="setup-section" markdown="1">
<summary><input class="setup-checkbox" type="checkbox" data-setup-item="terminal" aria-label="Mark Terminal as complete"> <h2>Terminal</h2></summary>

You will need a terminal to run commands throughout the workshop.

- **macOS/Linux:** Use the built-in Terminal application.
- **Windows:** Use Git Bash.

### Verify the terminal

```shell
pwd
ls
```

Both commands should run successfully.

</details>

<details class="setup-section" markdown="1">
<summary><input class="setup-checkbox" type="checkbox" data-setup-item="github-and-ssh" aria-label="Mark GitHub and SSH as complete"> <h2>GitHub and SSH</h2></summary>

We will use GitHub throughout the workshop to download material and submit
changes.

If you do not already have an account, create one at <https://github.com>.

If you are a student or educator, you may also be eligible for GitHub
Education: <https://education.github.com>.

### Configure SSH

Follow this guide:

<https://github.com/Nikoleta-v3/HitchCos/wiki/ssh-key>

Verify your configuration:

```shell
ssh -T git@github.com
```

</details>

<details class="setup-section" markdown="1">
<summary><input class="setup-checkbox" type="checkbox" data-setup-item="editor" aria-label="Mark Text editor as complete"> <h2>Text editor</h2></summary>

We recommend **Visual Studio Code (VS Code)**, although you are welcome to use
any editor or IDE that you are comfortable with.

### Installation

Download VS Code:

<https://code.visualstudio.com>

### Install the Python extension

1. Open VS Code.
2. Open the Extensions view.
3. Search for **Python**.
4. Install the extension published by Microsoft.

</details>

<details class="setup-section" markdown="1">
<summary><input class="setup-checkbox" type="checkbox" data-setup-item="python" aria-label="Mark Python and packages as complete"> <h2>Python and packages</h2></summary>

Install the latest version of Python 3 from:

<https://www.python.org/downloads/>

Windows users should check **Add Python to PATH** before clicking **Install**.

### Verify Python

```shell
python --version
```

or

```shell
python3 --version
```

### Install the required packages

```shell
pip install pandas matplotlib dask
```

or

```shell
pip3 install pandas matplotlib dask
```

### Verify the packages

```shell
python -c "import pandas, matplotlib, dask; print('Everything is installed!')"
```

If `python` is not recognised, replace it with `python3`.

</details>

<details class="setup-section" markdown="1">
<summary><input class="setup-checkbox" type="checkbox" data-setup-item="latex" aria-label="Mark LaTeX as complete"> <h2>LaTeX</h2></summary>

To compile the LaTeX exercises, install a LaTeX distribution.

- **Windows:** MiKTeX
- **macOS:** MacTeX
- **Linux:** TeX Live

### Verify the installation

```shell
pdflatex --version
```

</details>

<details class="setup-section" markdown="1">
<summary><input class="setup-checkbox" type="checkbox" data-setup-item="cpp" aria-label="Mark C++ compiler as complete"> <h2>C++ compiler (optional)</h2></summary>

If you plan to complete the optional C++ exercises, verify that your compiler
is available:

```shell
g++ --version
```

or

```shell
clang++ --version
```

</details>

<details class="setup-section" markdown="1">
<summary><input class="setup-checkbox" type="checkbox" data-setup-item="checklist" aria-label="Mark Final checklist as complete"> <h2>Final checklist</h2></summary>

Before the workshop, make sure the following commands all work:

```shell
git --version
python --version
python -c "import pandas, matplotlib, dask"
ssh -T git@github.com
pdflatex --version
```

If you are completing the optional C++ exercises, also verify:

```shell
g++ --version
```

or

```shell
clang++ --version
```

</details>

## Troubleshooting

If you encounter any problems during the installation process, please open an
issue or contact the workshop organizers before the workshop begins.
