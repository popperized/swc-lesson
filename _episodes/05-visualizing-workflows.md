---
title: "Visualizing Workflows"
teaching: 20
exercises: 0
questions:
- "How can I quickly get an idea of what a workflow does?"
objectives:
- "Visualize an end-to-end simple experimentation pipeline."
- "Access workflows created by others."
keypoints:
- "The `popper dot` subcommand allows to visualize of a workflow."
- "Getting access to a workflow that someone else created is a matter 
  of git-cloning a repository."
---

A useful way of abstracting scientific explorations is by thinking in 
terms of a workflow. While `.workflow` files are relatively easy to 
read, it is useful to have a way of quickly visualizing what a 
pipeline does. Popper provides the option of generating a graph 
depicting what a workflow does.

To generate a graph for our example pipeline, execute the following:

```bash
popper dot
```

The above generates a graph in `.dot` format. To visualize it, you can 
install the [`graphviz`](https://graphviz.gitlab.io/) package and 
execute:

```bash
popper dot | dot -T png -o wf.png
```

The above generates a `wf.png` file containing the workflow for this 
pipeline. Alternatively you can use the <http://www.webgraphviz.com/> 
website to generate a graph by copy-pasting the output of the `popper 
workflow` command.

### A More Complex Example

The following shows how get access and print the graph for a more 
complex workflow:

```bash
git clone https://github.com/actions/example-gcloud

cd example-gcloud

popper dot | dot -T png -o wf.png
```

