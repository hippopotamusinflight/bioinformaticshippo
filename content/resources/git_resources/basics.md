---
title: "Basics"
linktitle: "Basics"
summary:
date: 2020-05-26T09:55:52-04:00
lastmod: 2020-05-26T09:55:52-04:00
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.
weight: 5
menu:
  git_resources:
    name: "Basics"
    # parent: pid
    weight: 5
---

<br>

## setup git
- in a project dir
```bash
mkdir example_repo                             create repo directory
git init                                       initialize local git repo, creates .git folder
git config --global user.name 'John Doe'
git config --global user.email 'jdoe@email.com'
```

<br>

## create .gitignore
- make sure you set up .gitignore before `git add`
```bash
vim .gitignore

   log.txt
   /dir1
   *.RMarkdown
```

<br>

## git add, rm, commit, status
```bash
git add file1 file2                add files to index/staging area before committing
git add file3 ...
git add *.html                     add all .html files to staging area
git add .                          add all files to staging area
git rm --cached file3              remove file from staging area

git status                         check status of working tree (what files added, modified, unmodified)
git commit                         commit files in staging area
git commit -m 'changed something'  to add comment to commit
```

<br>

***
## create branches
```bash
git branch somebranch                    add a branch
git checkout somebranch                  goes into a branch

    - create/edit some files
    - commit them

git checkout master                      goes into master branch

git merge somebranch                     while in master branch, merge somebranch
git merge somebranch -m 'some message'   to write a commit message
```

<br>

***
## working with GitHub
- create new repo on GitHub
```bash
git remote                               # to view remote repos
git remote add origin https://github.com/username/repo
git remote remove origin
git remote -v

git push -u origin master                # to push master to remote repo
# github login window will open to authenticate for the first time
```
- reload GitHub page and local repo should be there
```bash
git pull <url>        # pulls latest version from remote repo
git clone <url>       # copies a remote repo into current dir
```

<br>

***
## git reset
- to restore previous commits
```bash
git reflog
f322517 (HEAD -> master) HEAD@{0}: reset: moving to HEAD
f322517 (HEAD -> master) HEAD@{1}: pull https://github.com/username/repo.git: Merge made by the 'recursive' strategy.
9a71bf1 HEAD@{2}: commit: updated gitignore
36340f8 (origin/master) HEAD@{3}: commit: updated gitignore
5f98d77 HEAD@{4}: commit (initial): initial commit

git reset --hard 5f98d77
HEAD is now at 5f98d77 website initial commit
```
- lesson: add files to .gitignore before initial commit
- lesson: do not delete files on github, do everything with git

<br>

***
## misc
- create SSH keys so no need to login to github every time

## EOF