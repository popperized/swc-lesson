---
title: "Introduction"
teaching: 20
exercises: 0
questions:
- "How can I make my results easier to reproduce?"
objectives:
- "Introduction to the practical aspects of reproducibility."
- "Explain the notition of a scientific workflow or pipeline."
keypoints:
- "Scientific explorations can be described in a workflow language so 
  that they can be automatically re-executed."
- "The Github Actions (GHA) Workflow Language can be used to express 
  workflows that are easier to re-execute since they assume they will 
  run on a containerized environment."
- "Popper is a GHA workflow execution engine that helps users create
  automated and portable experimentation workflows."
---

Many scientific exploration pipelines have the following structure:

![](../assets/img/sci_pipeline.png)

Each of the stages of the above pipeline involve carrying out some (or
all) of the following tasks:

  * Writing, compiling and packaging code.
  * Configuring computing environments.
  * Executing applications on multiple (local or remote) compute
    environments.
  * Writing documentation or any type of manuscript (tech reports or
    scholarly articles).
  * Archiving particular versions of a pipeline so it can be
    referenced in the future (e.g. by using a DOI generation service).

While one can manually execute these tasks, automating them can not
only significantly reduce the amount of effort required by others (or
ourselves) to re-execute a pipeline but, more importantly, allow
others to have an explicit and easy to understand record of what were
the series of high-level steps that were executed and in which order. 
This is also commonly referred to a "workflow" or a "pipeline".

### An Example

Assume that, as part of a study, we are analyzing historic data in 
order to obtain a statistical descriptor for this dataset. 
Specifically, we download [this table][co2table] containing a record 
of CO2 emissions per capita per country, and obtain the arithmetic 
mean for an arbitrary number of years.

We start by creating a folder that we'll name `myproject/`, and we'll 
first download the dataset: right click on [this link][lnk], select 
`Save as...` and save it in a `myproject/data/` folder (so file will 
reside in `myproject/data/global.csv`) as shown below:

```
tree .
.
└── data
    └── global.csv

1 directory, 1 file
```

> **NOTE**: The above uses the `tree` command that might not be 
available on your machine. You can either install it or instead run 
`ls -lR`.

Then, assume we have a `scripts/get_mean_by_group.py` Python script 
that calculates the mean for this dataset:

```python
#!/usr/bin/env python
import csv
import sys

try:
    # Python 3
    from itertools import zip_longest
except ImportError:
    # Python 2
    from itertools import izip_longest as zip_longest

fname = sys.argv[1]
group_size = int(sys.argv[2])
fout = fname.replace('.csv', '') + '_per_capita_mean.csv'


def grouper(iterable, n, fillvalue=None):
    args = [iter(iterable)] * n
    return zip_longest(*args, fillvalue=fillvalue)


with open(fname, 'r') as fi, open(fout, 'w') as fo:
    r = csv.reader(fi)
    w = csv.writer(fo)

    # get 0-based index of last column in CSV file
    last = len(next(r)) - 1

    for g in grouper(r, group_size):
        group_sum = 0
        year = 0

        for row in g:
            # check for empty value (and assume zero if not found)
            if row[last]:
                group_sum += float(row[last])
            else:
                group_sum += 0.0
            year = row[0]

        w.writerow([year, group_sum / group_size])
```

We can copy-paste the content of this script and put it in a 
`scripts/` folder. The folder structure now looks like this:


```bash
tree .
.
├── data
│   └── global.csv
└── scripts
    └── get_mean_by_group.py

2 directories, 2 files
```

This script is invoked by running the following command on the 
terminal:

```bash
python scripts/get_mean_by_group.py data/global.csv 5
```

Once we execute the above, a `data/global_per_capita_mean.csv` file is 
created with the result. We can eyeball this and verify that the data 
has been aggregated in groups of 5 years (the parameter we passed) and 
for each group the mean was obtained. Note how years before 1950 did 
not contain an entry for the `Per Capita` column and thus any group 
before this year has 0.0 as the resulting mean.

Thus, the workflow in this case is comprised of the following two 
steps:

 1. Download data.
 2. Execute analysis.

In order to automate these steps, we can write a Bash scripts:

```bash
#!/bin/bash
set -e

curl \
  --create-dirs \
  -Lo workflows/minimal-python/data/global.csv \
  https://github.com/datasets/co2-fossil-global/raw/master/global.csv

python scripts/get_mean_by_group.py data/global.csv 5
```

> ## What Might Cause This Script to Fail?
>
> The above example, although minimal, might not work properly when 
> executed in other environments. Can you think of what might cause 
> this Bash script to fail when executed in other environments?
>
> > ## Solution
> > 
> > Some things that might go wrong:
> >
> >  1. The `curl` command is not available.
> >  2. The Python script (lines 5-10) takes care of dealing with 
> >     portability issues across the two main versions of the Python 
> >     runtime that are in use today. If this wasn't the case (e.g. 
> >     line 7 was the only one in the script), this would fail when 
> >     executed on a Python version distinct to the one where the 
> >     workflow originally ran.
> {: .solution}
{: .challenge}

We can make it easier for others to re-execute what we have done by 
automating all these manual tasks in a way that is portable. By 
portability we mean that someone else re-running this workflow would 
do so without having to deal with issues arising from the fact that 
their computing environment is different than the one where this 
workflow originally executed. In this lesson, we will learn how to do 
this by writing workflows in the [Github Actions (GHA) Workflow 
language][gha]. We will also use [Popper][pp] to run GHA workflows in 
our machine and on CI services.

{% include links.md %}

[co2table]: https://github.com/datasets/co2-fossil-global
[gha]: https://developer.github.com/actions/managing-workflows/workflow-configuration-options/#example-workflow
[pp]: https://github.com/systemslab/popper
[lnk]: https://raw.githubusercontent.com/datasets/co2-fossil-global/master/global.csv
