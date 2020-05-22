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

### Ignoring Files

Ignore all files with suffix `.o` or `.a`, all files that end with a tilde:

```
cat .gitignore
*.[oa]
*~
!lib.a  # do track lib.a, even though it's ignored before
build/  # everything inside build directory are ignored
```

- blank lines or lines starting with # are ignored
- standard glob patterns work
- you can end patterns with a forward slash / to specify a dictionary
- you can negate a pattern by starting it with an exclamation point (!)

Glob pattern:

- asterisk (*) matches zero or more characters
- `[]` matches any character inside the brackets
- questio mark (?) matches a single character
- brackets enclosing characters separated by a hyphen (e.g. `[0-9]`) matches any character between them

### Viewing Exact Changes

`git diff` compares what is in working directory with what is in staging area, showing the changes that haven't been staged yet. If all changes are staged, `git diff` shows no output.

`git diff --cached` or `git diff --staged` (for Git 1.6.1 or later) compares staged changes to last commit.

### Committing Changes

To commit staged changes:

`git commit`

This would launch pre-specified editor. To type commit message inline:

`git commit -m "message"`

To stage every file that is already tracked without `git add`:

`git commit -a`

### Remove Files

To remove a file from Git, remove it from tracked files (staging area) and then commit. Removing the file (ie. `rm file`) would show up as "Changed but not updated", then `git rm file` makes it "Changes to be committed".

If a file has been modified and added to the index (ie. staged), Git need a force removal (-f) to remove the file, this is a feature to prevent accidental removal of data.

To keep the file in working tree but remove it from staging area:

`git rm --cached file`

It supports files, directories and file-glob patterns.

e.g. remove all .log files in log directory:

`git rm log/\*.log`
(the backslash is necessary because Git does its own filename expansion in addition to shell's filename expansion)

### Moving Files

Git doesn't track file movments, if a file is renamed, no metadata is stored for this fact but Git is smart enough to figure out.

Git has a `mv` command:

`git mv from_file to_file`

Git treats this as a rename, and this is equivalent to:

```
mv from_file to_file
git add to_file
```

