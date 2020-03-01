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

The workflow we saw previously has two steps:

 1. Download data.
 2. Execute analysis.

It's time to write our first Github Actions workflow. To do so, create 
a `wf.yml` file and put it a `myproject/` folder. The content of this 
file are the following:

```hcl
steps:
- uses: docker://byrnedo/alpine-curl:0.1.8
  args: [-LO, https://github.com/datasets/co2-fossil-global/raw/master/global.csv]

- uses: docker://python:3.8.1-alpine
  runs: [scripts/get_mean_by_group.py, data/global.csv, "5"]
```

A workflow is specified with a `steps` keyword that denotes a [YAML 
list][ymllists]. Each item in that list is a step, which is a YAML 
dictionary with at least a `uses` key in it. For more information on 
the syntax, check the [official Popper documentation][ymlsyntax].

### Running `popper` commands

Before we go ahead and execute the above workflow, let's look at the 
syntax for running `popper` (sub) commands. The general pattern is the 
following:

```bash
popper <subcommand> [options]
```

Where `<subcommand>` is one of the available subcommands such as 
`run`, `ci`, `version`, etc. To get general help:

```bash
popper --help
```

And help information for each subcommand can also be displayed, for 
example:

```bash
popper run --help
```

### Running a workflow

Now that we know how to execute commands, let's run the workflow we 
showed above:

```bash
cd myproject
popper run -f wf.yml
```

The output of the above will inform you of what

[ymllists]: https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html
[ymlsyntax]: https://popper.readthedocs.io/en/latest/sections/cn_workflows.html#syntax

{% include links.md %}
