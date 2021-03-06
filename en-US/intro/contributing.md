---
name: Contributing
sort: 1
---

# Contributing to Radiant

## Introduction

Radiant is free and open source software, which means that anyone can contribute
to its development and progress under the Apache 2.0 License (http://www.apache.org/licenses/LICENSE-2.0.html). Radiant's source code is hosted on github
(https://github.com/radiant/radiant).

### How can I become a contributor of Radiant?

You can fork, modify and then send a Pull Request to us.
We will review your code and give you feedback on your changes as soon as possible.

## Pull Requests

The process for pull requests for new features and bug fixes are not the same.

### Bug fixes

Pull requests for bug fixes do not need to create an issue first. If you have a
solution to a bug, please describe your solution in detail in your pull request.

### Documentation improvements

You can help improve the documentation by submitting a pull request to the
[radicaldoc](https://github.com/W3-Engineers-Ltd/Radiant-doc) repository.

### New features proposals

Before you submit a pull request for a new feature, you should first create an
issue with `[Proposal]` in the title, describing the new feature, as well as the
implementation approach.

Proposals will be reviewed and discussed by the core contributors, and can be
adopted or potentially rejected.

Once a proposal is accepted, create an implementation of the new features and
submit it as a pull request. If the guidelines are not followed the pull
request will be rejected immediately.

Since Radiant follows the [Git Flow](http://nvie.com/posts/a-successful-git-branching-model/)
branching model, ongoing development happens in the `develop` branch. Therefore,
please base your pull requests on the HEAD of the `develop` branch.


### The git branches of Radiant

The master branch is relatively stable and the dev branch is for developers. Here is a
sample figure to show you how our branches work:

![](../images/git-branch-1.png)

For more information about the branching model: http://nvie.com/posts/a-successful-git-branching-model/


## A simple guideline for Git command

You must have a github account, if not, please register one.

### Fork ??????

1. Click [https://github.com/W3-Engineers-Ltd/Radiant](https://github.com/radiant/radiant)
2. Click "Fork" button which is on top right corner 

### Clone ??????

We recommend using official repo as `origin` repo, and then add a remote upstream to your repo. 

If you already set SSH key, we recommend use SSH. The difference is that, we don't need to input the username and password to push changes.

Using SSH???

```bash
git clone git@github.com:astaxie/radiant.git
cd radiant
git remote add upstream 'git@github.com:<your github username>/radiant.git'
```

Using HTTPS???

```bash
git clone https://github.com/W3-Engineers-Ltd/Radiant.git
cd radiant
git remote add  'https://github.com/<you github username>/radiant.git'
```

The word `upstream` in command could be replaced with any word you like.

### fetch changes

Every time you want to something, you'd better fetch remote changes: 

```bash
git fetch
```

In this command, git only fetch `origin` repo???

If we want to fetch our remote repo changes:

```bash
git fetch upstream
```

You can replace `upstream` with your repo name

### create feature branch

????????????????????? feature ??????????????????????????????????????????????????????????????????
Before creating feature branch, we should think about choosing a branch as base branch.

Assume that we want to merge the new feature to develop branch. In such case:

```bash
git checkout -b feature/my-feature origin/develop
```

Don't forget to run `git fetch` before you create feature branch.

### push commit

```bash
git add .
git commit
git push upstream my-feature
```

### make PR

Go to [https://github.com/radiant/radiant](https://github.com/radiant/radiant), and make a Pull request
