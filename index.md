---
layout: lesson
root: .  # Is the only page that doesn't follow the pattern /:path/index.html
permalink: index.html  # Is the only page that doesn't follow the pattern /:path/index.html
---

In this lesson, you will learn how to automate an experimentation 
workflow using [Popper][pp], a [Github Actions][gha] (GHA) workflow 
execution engine. Using the GHA workflow language allows experimenters 
to implement automated and portable workflows that are easy to 
re-execute and extend by others.

<!-- this is an html comment -->

{% comment %} This is a comment in Liquid {% endcomment %}

> ## Prerequisites
>
> In this lesson we use the [Unix 
> Shell](http://swcarpentry.github.io/shell-novice), and previous 
> experience is highly recommended before taking this lesson. We 
> assume you are familiar with tasks such as listing directories, 
> creating, copying, remove and listing files, as well as modifying 
> file permissions and running scripts. We also make use of 
> [Git](http://swcarpentry.github.io/git-novice) (and Github) basic 
> features such as creating commits and pushing to remote repos. In 
> addition, we also make basic use of 
> [Python](http://swcarpentry.github.io/python-novice-inflammation) 
> and [Docker](https://ome.github.io/training-docker), which we also 
> encourage you to learn before taking this lesson.
{: .prereq}

{% include links.md %}

[gha]: https://developer.github.com/actions/managing-workflows/workflow-configuration-options/#example-workflow
[pp]: https://github.com/systemslab/popper
