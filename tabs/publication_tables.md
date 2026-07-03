---
layout: workshop
title: Publication-Ready Tables
section: Scientific Communication
---

## 02: Publication-ready tables

Tables are often treated as something separate from the analysis. A common
workflow is to calculate values in Python, copy them into a document, adjust the
formatting by hand, and repeat the process whenever the data change.

That is fragile. It is easy to copy the wrong value, forget to update a number,
or lose track of how the table was produced.

Instead, we can generate tables directly from the analysis code. The table then
becomes part of the reproducible workflow, just like a figure.

In this section, we will use the same `tips` dataset as in the plotting section
and export a small table for use in a LaTeX document.

## Create a table script

Create a folder for scripts if it does not already exist:

```shell
mkdir -p scripts
```

Create a folder for generated tables:

```shell
mkdir -p tables
```

Create a file called `scripts/table_tips.py`.

Start with the imports and data preparation:

```python
from pathlib import Path

import pandas as pd


url = "https://raw.githubusercontent.com/mwaskom/seaborn-data/master/tips.csv"
tips = pd.read_csv(url)

tips["tip_percentage"] = 100 * tips["tip"] / tips["total_bill"]

Path("tables").mkdir(exist_ok=True)
```

This reads the data, creates a tip percentage column, and makes sure the output
folder exists.

## Create a summary table

Suppose we want a table showing how tip percentage varies with party size.

Add:

```python
table = (
    tips.groupby("size")
    .agg(
        observations=("tip_percentage", "size"),
        average_tip_percentage=("tip_percentage", "mean"),
        median_tip_percentage=("tip_percentage", "median"),
    )
    .reset_index()
)
```

This groups the data by party size and calculates:

- the number of observations
- the average tip percentage
- the median tip percentage

Before exporting, make the column names readable:

```python
table = table.rename(
    columns={
        "size": "Party size",
        "observations": "Observations",
        "average_tip_percentage": "Mean tip (%)",
        "median_tip_percentage": "Median tip (%)",
    }
)
```

Round numerical values deliberately:

```python
table["Mean tip (%)"] = table["Mean tip (%)"].round(1)
table["Median tip (%)"] = table["Median tip (%)"].round(1)
```

Print the table so that you can inspect it:

```python
print(table)
```

## Export the table

Export a CSV version:

```python
table.to_csv("tables/tips_by_party_size.csv", index=False)
```

CSV is useful because it is easy to inspect, version, and reuse.

Export a LaTeX version:

```python
latex_table = table.to_latex(
    index=False,
    caption="Tip percentage by party size.",
    label="tab:tips-party-size",
)

Path("tables/tips_by_party_size.tex").write_text(latex_table)
```

This creates a file that can be included in a LaTeX document.

## Complete script

The full `scripts/table_tips.py` script is:

```python
from pathlib import Path

import pandas as pd


url = "https://raw.githubusercontent.com/mwaskom/seaborn-data/master/tips.csv"
tips = pd.read_csv(url)

tips["tip_percentage"] = 100 * tips["tip"] / tips["total_bill"]

Path("tables").mkdir(exist_ok=True)

table = (
    tips.groupby("size")
    .agg(
        observations=("tip_percentage", "size"),
        average_tip_percentage=("tip_percentage", "mean"),
        median_tip_percentage=("tip_percentage", "median"),
    )
    .reset_index()
)

table = table.rename(
    columns={
        "size": "Party size",
        "observations": "Observations",
        "average_tip_percentage": "Mean tip (%)",
        "median_tip_percentage": "Median tip (%)",
    }
)

table["Mean tip (%)"] = table["Mean tip (%)"].round(1)
table["Median tip (%)"] = table["Median tip (%)"].round(1)

print(table)

table.to_csv("tables/tips_by_party_size.csv", index=False)

latex_table = table.to_latex(
    index=False,
    caption="Tip percentage by party size.",
    label="tab:tips-party-size",
)

Path("tables/tips_by_party_size.tex").write_text(latex_table)
```

Run the script:

```shell
python scripts/table_tips.py
```

or:

```shell
python3 scripts/table_tips.py
```

You should now have:

```text
tables/
+-- tips_by_party_size.csv
+-- tips_by_party_size.tex
```

## Save the change with git

After generating the table, check what changed:

```shell
git status
```

Add the script:

```shell
git add scripts/table_tips.py
```

Inspect what is staged:

```shell
git diff --staged
```

For a paper repository, it can also be useful to commit small generated table
files because the manuscript may depend on them.

If you want the generated outputs in the repository, add them too:

```shell
git add tables/tips_by_party_size.csv tables/tips_by_party_size.tex
```

Check the staged changes again:

```shell
git diff --staged
```

Commit the change:

```shell
git commit -m "Add reproducible tips summary table"
```

## Include the table in LaTeX

In a LaTeX document, the generated table can be included with:

```latex
\input{tables/tips_by_party_size.tex}
```

The exact path depends on where the LaTeX file is located. The important point
is that the document reads the generated table rather than relying on values
copied by hand.

## What makes a table publication-ready?

A useful table should be readable without requiring the reader to inspect the
code.

Check:

- Are the column names clear?
- Are units included?
- Are values rounded consistently?
- Are rows sorted in a meaningful order?
- Is the caption informative?
- Can the table be regenerated from the analysis code?

## When is a table better than a figure?

Use a table when exact values matter.

Use a figure when the main goal is to show a pattern, trend, distribution, or
comparison.

Many papers need both. The key is to generate both from the same analysis
workflow so that they stay consistent.

## Summary

In this section, we generated a table from analysis code instead of copying
values manually.

We:

- loaded the `tips` dataset
- calculated summary statistics
- renamed and rounded columns
- exported CSV and LaTeX versions
- discussed how to include the table in a LaTeX document

The main lesson is that tables should be reproducible research outputs, not
manual formatting exercises.
