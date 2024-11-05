---
title: "Git Cheat Sheet"
seoTitle: "Essential Git Commands Cheat Sheet"
seoDescription: "Quick reference for commonly used Git commands, updated periodically. Perfect for beginners and advanced users alike"
datePublished: Tue Nov 05 2024 00:55:42 GMT+0000 (Coordinated Universal Time)
cuid: cm33qnxso000309l9gsfreb46
slug: git-cheat-sheet
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/842ofHC6MaI/upload/a4e5f53f14fe3d315caeea2b9853e351.jpeg
tags: git, devops, cheatsheet

---

This cheat sheet summarizes my commonly used git commands, I will update that periodically.

### Initialize

```bash
git config --global user.name "Name Username"
git config --global user.email name@example.com
```

I rarely initialize repo locally, so there is no need for me to use `git init` but keep in mind that it’s useful for `initializing an existing directory as a Git repository`

### Basic commands

```bash
git checkout -b ＜new-branch＞ # create new branch and switch to it, never work directly on master
git add . # add all files to next commit (stage them)
git status # check files added to commit
git commit -m "commit msg" # commit files to branch
git push # push changes to remote repository
git pull # pull changes pushhd to remote repository
```

### Advanced commands

```bash
git revert <commit> # creates NEW commit that reverts changes introduced in specified commit
git rebase <branch> # reapplies commits on the current branch onto the tip of the specified branch
```