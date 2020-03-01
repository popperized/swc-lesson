---
title: "Sharing Workflows"
teaching: 0
exercises: 0
questions:
- ""
objectives:
- ""
keypoints:
- ""
---

We will now create a repository to hold the workflow we just created:

```bash
cd myproject
git init
echo '# myproject' > README.md
git add .
git commit -m 'first commit'
```

Let's add the workflow file to the repository:

```bash
git add wf.yml

git commit -m 'my first workflow'
```

### Sharing workflows via Github

Create a repository [on Github][gh-create]. Once your Github 
repository has been created, register it as a remote repository on 
your local repository:

```bash
git remote add origin git@github.com:<user>/<repo>
```

where `<user>` is your username and `<repo>` is the name of the 
repository you have created. Then, push your local commits:

```bash
git push -u origin master
```

[gh-create]: https://help.github.com/articles/create-a-repo/
