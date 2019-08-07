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
  runs = [
    "curl", "--create-dirs", "-Lo data/global.csv",
    "https://github.com/datasets/co2-fossil-global/raw/master/global.csv"
  ]
}

action "run analysis" {
  needs = "download data"
  uses = "actions/bin/sh@master"
  runs = [
    "python", "scripts/get_mean_by_group.py", "data/global.csv", "5"
  ]
}
```

A workflow is composed by workflow and action blocks. Workflow blocks 
are specified in the following way:

```hcl
workflow "NAME" {
  <WORKFLOW ATTRIBUTES>
}
```

While action blocks have the same form but use the `action` keyword 
instead:

```hcl
action "NAME" {
  <ACTION ATTRIBUTES>
}
```

In the example above, the workflow block specifies that it needs to 
execute `run analysis` (specified in the `resolves` attribute), but 
because the `run analysis` action specifies that it depends on 
`download data` being executed first (via the `needs` attribute), the 
`download data` block executes first. The `run analysis` action will 
execute once `download data` has successfully completed.

### Workflow attributes

Check the official [action attributes documentation][wf-attr].

[wf-attr]: https://developer.github.com/actions/managing-workflows/workflow-configuration-options/#workflow-attributes

### Action attributes

Check the official [action attributes documentation][act-attr].

[act-attr]: https://developer.github.com/actions/managing-workflows/workflow-configuration-options/#actions-attributes


{% include links.md %}
