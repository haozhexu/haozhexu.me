---
title: "Git Notes"
date: 2020-05-08T11:36:00+11:00
draft: true
ads: true
categories:
  - Article
tags:
  - git
  - versioncontrol
---
# Git Notes

Data in Git is like a set of snapshots of a mini filesystem, every _commit_ is a snapshot of all the files at the moment, with reference stored for that snapsthot; Files that have not changed aren't stored - instead just a link to the previous identical file that has been stored already.

- nearly every operation is local
- everything is check-summed (SHA-1) to ensure integrity
- most operations only add data to Git database

## Three States

Git has three main states for files:
- committed: data is stored in local database
- modified: data has been changed but not yet committed to database
- staged: modified file in current version has been marked to go into next commit snapshot

Above states are reflected as three main sections of a Git project
- the Git directory: where Git stores the metadata and object database, clone a repository copies this
- the working directory: single checkout of one version of the project
- the staging area: a file that stores information about what goes into next commit

## Configuration

Configuration at each level overrides values in the previous level.

- __/etc/gitconfig__: contains settings for all users and all repositories, passing `--system` to git config to use this
- __~/.gitconfig__: user specific configuration file, passing `--global` to use this
- __.git/config__: configuration specific to a single repository

### Set Identity

Setting username and e-mail address:

```
git config --global user.name "Name"
git config --global user.email "test@email.com"
```

### Editor

`git config --global core.editor vim`

### Diff Tool

`git config --global merge.tool vimdiff`

### Check Settings

`git config --list`

Same keys in the config list may appear more than once because Git reads configurations from different files, the last value is used for each unique key.

Check the value for a particular key:

`git config user.name`

## Basics

Track existing project in Git:

`git init`

Adding files to version control:

```
git add *.swift
git add README.md
git commit -m "Initial commit"
```

Clone existing repository:

`git clone [REPO URL]`

Which clones the project into a direcotry with repository name by default, to clone to a different directory:

`git clone [REPO URL] [DIRECTORY]`

Each file in working directory can be:

- _tracked_: files that were in the last snapshot, can be unmodified, modified or staged
- _untracked_: files in working directory that weren't in last snapshot or staging area

Checking status:

`git status`

Track file:

`git add file`

It's a multipurpose command for tracking new files, staging files and other things like marking merge-conflicted files as resolved. If a file is modified after `git add`, it has to be added again to stage the latest version of the file.




