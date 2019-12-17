---
title: "Keeping a fork up to date"
date: "2018-03-03"
template: "post"
draft: false
slug: "git-fork-up-to-date"
category: "git"
tags:
  - "git"
description: ""
socialImage: ""
---

### Clone your fork:

```
git clone git@github.com:YOUR-USERNAME/YOUR-FORKED-REPO.git
```

### Add remote from original repository in your forked repository:

```
cd into/cloned/fork-repo
git remote add upstream git://github.com/ORIGINAL-DEV-USERNAME/REPO-YOU-FORKED-FROM.git
git fetch upstream
```

### Updating your fork from original repo to keep up with their changes:

```
git pull upstream master
```
