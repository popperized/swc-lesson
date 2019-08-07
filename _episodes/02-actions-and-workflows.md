---
title: "Actions and Workflows"
teaching: 0
exercises: 0
questions:
- "How can I automate a scientific exploration using a Github Actions 
  workflow?"
objectives:
- "Explain what Github Actions workflows are."
- "Explain how a Github Action workflow differs from shell scripts."
keypoints:
- ""
---

The workflow we saw has two steps:

 1. Download data.
 2. Execute analysis.

It's time to write our first Github Actions workflow. To do so, create 
a `main.workflow` file and put it in the `myproject/` folder. The 
contents are the following:

```hcl
workflow "co2 emissions" {
  resolves = "run analysis"
}

action "download data" {
  uses = "actions/bin/sh@master"
  args = [
    "--create-dirs",
    "-Lo data/global.csv",
    "https://github.com/datasets/co2-fossil-global/raw/master/global.csv"
  ]
}

action "run analysis" {
  needs = "download data"
  uses = "actions/bin/sh@master"
  args = ["scripts/get_mean_by_group.py", "data/global.csv", "5"]
}
```

-------

**TODO** - Intro to GHA

-------

{% include links.md %}
