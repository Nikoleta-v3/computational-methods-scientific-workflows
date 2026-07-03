---
layout: workshop
title: Git Cheat Sheet
section: Reference
---

### Setup

To retrieve an entire repository from a hosted location via URL:

```shell
$ git clone [url]
```

### Status and History

To show modified files in working directory, staged for your next commit:

```shell
$ git status
```

To show all commits in the current branch’s history:

```shell
$ git log
```

### Staging and Committing

To add a file as it looks now to your next commit (stage):

```shell
$ git add [file]
```

or to add all:

```shell
$ git add .
```

To diff of what is changed but not staged:

```shell
$ git diff
```

To diff of what is staged but not yet committed:

```shell
$ git diff --staged
```

To commit your staged content as a new commit snapshot:

```shell
$ git commit -m “[descriptive message]”
```

or to open nano to write you commit message just simply:

```shell
$ git commit
```


### Branches

To list your branches. a * will appear next to the currently active branch:

```shell
$ git branch
```

To create a new branch: 

```shell
$ git branch [branch-name]
```

To switch to another branch and check it out into your working directory:

```shell
$ git checkout [branch-name]
```


### To retrieve and push updates

To fetch and merge any commits from the tracking remote branch:

```shell
$ git pull [remote-name] [branch-name]
```

To push local branch commits to the remote repository branch:

```shell
$ git push [remote-name] [branch-name]
```
