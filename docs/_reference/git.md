---
layout: default
title:  "Git"
description: Commonly used git commands
---

# git

Change a file's name from a previous commit

```
$ git log --oneline # to find the commit you want to edit
$ git rebase -i <PARENT_of_the_commit_you_want_to_edit>

edit bf9af7b # ★
pick cd3b06d # ⭐️
pick 8e746c6 # ★
...

$ git show REBASE_HEAD # confirm it’s the commit you want to edit
$ git mv a.py one.py
$ git commit --amend
$ git rebase --continue
$ git push --force-with-lease
```

Stage by hunks (if [magit](https://magit.vc) is unavailable)

```
git add -p
```

Create a new branch

```
git switch -c <branch>
```

Checkout a remote branch

```
git switch -c <branch> <origin/branch>
```

