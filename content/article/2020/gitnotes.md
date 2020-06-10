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
git rm from_file
git add to_file
```

### Viewing the Commit History

```
git log
```

- by default without arguments, `git log` lists the commits in reverse chronological order

```
git log -p -2
```

- displays the same information but with a diff following each entry
- limit result to 2 entries

```
git log --stat
```

- prints a list of modified files below each entry
- how many files were changed and how many lines in those files were added and/or removed

```
git log --pretty=oneline
```

- specifies output format with a few prebuilt options available to use
- `oneline` prints each commit on a single line
- `format` lets you specify your own format, usually used for generating output for machine parsing
- above options are useful with `--graph` for ASCII graph history

__author__ vs __committer__

- _author_ is the person who originally wrote the work
- _committer_ is the person who last applied the work

#### Limiting Log Output

- `--since` lets you specify specific or relative date
- `--author` allows you filter on specific author
- `--grep` lets you search for keywords in the commit message
- path can be passed to `git log`, limiting log output to commits introduced changes to those files
- `--no-merges` exclude merge commits

e.g.

```
git log --pretty="%h:%s" --author=someone --since="2020-01-01" --before="2020-04-01" --no-merges -- sources/
```

### Undoing Things

```
git commit --amend
```

- `--amend` takes staging area and use it for the commit
- without change since last commit, the snapshot will look the same and only change commit message
- commit message editor pops up with existing message, edit the message to overwrite previous commit

#### Forgot Changes

```
git commit -m "my commit"
git add forgotten.txt
git commit --amend
```

- the second command replaces the results of the first with a single commit

#### Unstage a Staged File

```
git reset HEAD staged_file
```

or:

```
git restore --staged file
```

#### Reset a Modified File

```
git checkout -- modified_file
```

## Working with Remote

Remote repositories are versions of projects that are hosted on remote network.

### Show Remote

- `git remote`. `origin` is the default name Git gives to the server
- show remote with URL: `git remote -v`

### Add Remote Repositories

`git remote add shortname url`

### Fetching and Pulling

`git fetch shortname`

- fetch command pulls the data to local repository
- fetch doesn't automatically merge anything with current work

`git pull`

- fetch and merge a remote branch into local branch
- pre-requisite: local brach set up to track a remote branch
- by default, `git clone` automatically sets up local master to track remote master

### Pushing to Remotes

`git push [remote-name] [branch-name]`

e.g.

`git push origin master`

- only works if cloned from a server to which you have write access and nobody has pushed at the same time
- push is rejected if someone else pushed upstream
- you have to pull down their work first in incorporate it into your work

### Inspecting a Remote

`git remote show origin`

```
* remote origin
  URL: git://github.com/account/repo.git
  Remote branch merged with 'git pull' while on branch master
    master
  Tracked remote branches
    master
    test
```

More complex example:

```
$ git remote show origin
* remote origin
  URL: git@github.com:account/repo.git
  Remote branch merged with 'git pull' while on branch issues
    issues
  Remote branch merged with 'git pull' while on branch master
    master
  New remote branches (next fetch will store in remote/origin)
    caching
  Stale tracking branches (use 'git remote prune')
    legacy
  Tracked remote branches
    apiv2
    master
  Local branch pushed with 'git push'
    master:master
```

Above information shows:
- which branch is pushed when you run git push on certain branches
- which remote branches on the server you don't yet have
- which remote branches you have that have been removed from the server
- multiple branches that are automatically merged when you run git pull

### Removing and Renaming Remotes

`git remote rename old_name new_name`

- this would change remote branch names as well, e.g. `old_name/master` becomes `new_name/master`

`git remote rm name`

- remote a reference

## Tagging

- git has the ability to tag specific points in history
- this is often used to mark release points (e.g. v1.0)

### Listing Tags

`git tag`

- list tags in alphabetical order

You can also search tags:

`git tag -l v1.2.*`

### Createing Tags

- lightweight: like a branch that doesn't change, a pointer to a specific commit
- annotated: stored as full objects in Git database, check-summed, contains tagger name, e-mail and date, have a tagging message and can be signed and verified with GNU Privacy Guard (GPG)

#### Annotated Tags

`git tag -a v1.1 -m "release version 1.1"

```
git show v1.1
tag v1.1
Tagger: Full Name <someone@test.com>
Date: xxx
release version 1.1
commit <commit hash>
Merge: merge hashes
Author: Developer Name <developer@test.com>
Date: <date>

  Merge branch 'test'
```

#### Signed Tags

```
git tag -s v1.2 -m "signed release 1.2 tag"
You need a passphrase to unlock the secret key for user: "User Name <username@test.com>"
1024-bit DSA key, ID F12C123, created 2020-02-01
```

```
git show v1.2
tag v1.2
Tagger: User Name <username@test.com>
Date: <Date>

signed release 1.2 tag
-----BEGIN PGP SIGNATURE-----
Version: GnuPG v1.4.8 (Darwin)

<long signature here>
-----END PGP SIGNATURE---
commit <commit hash>
Merge <commit hashes>
Author: Developer Name <developer@test.com>
Date: <Date>

  Merge branch 'test'
```

#### Lightweight Tags

Lightweight tags are basically commit checksums stored in a file without other information.

```
git tag v2.0
git tag
v2.0
```

```
git show v2.0
commit <commit hash>
Merge: <commit hashes>
Author: Developer Name <developer@test.com>
Date: <date>

  Merge branch 'test'
```

#### Verifying Tags

Use GPG to verify the signature:

`git tag -v [tag-name]`

#### Tagging Commits in the Past

`git tag -a v2.1 <commit hash>`

#### Sharing Tags

- `git push` by default doesn't transfer tags to remote servers
- have to explicitly push tags after creating them
- the process is like sharing remote branches

`git push origin v2.1`

To push all tags:

`it push origin --tags`

### Tips and Tricks

#### Auto-Completion

Git comes with auto-completion script in its source `contrib/completion` directory, copy it to home directory and add below to `.bashrc` file:

`source ~/.git-completion.bash`

To set up auto-completion for all users, copy the script to `/opt/local/etc/bash_completion` on Mac or `/etc/bash_completion.d/` on Linux.

#### Git Aliases

Set up alias for commands using git config, for example:

```
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
```
(instead of `git commit`, you can use `git ci`, etc)

`git config --global alias.unstage 'reset HEAD --'`

is equivalent to:

```
git unstage fileA
git reset HEAD fileA
```

`git config --global alias.last 'log -1 HEAD'`

this makes it easy to see last commit:

`git last`

## Git Branching

Branching is when you diverge from the main development line and continue to work without touching the main line.

- Git stores a commit object that contains a pointer to the snapshot of staged content
- also includes author and message, zero or more pointers to the commit or direct parent commits
- first commit has no parent
- normal commit has one parent
- a merge commit of two or more branches has multiple parents

- a branch in Git is a lightweight movable pointer to one of these commits
- the default branch name in Git is master
- every time you commit, master moves forward automatically pointing to the last commit made
- Git knows the **current branch** by keeping a special pointer called HEAD
- a branch in Git is a simple file that contains the 40-characters SHA-1 checksum of the commit it points to

Single Commit Diagram

```
C1 (tree, author, committer, messsage, etc)
|
+- tree (blob -> file/size)
    |
    +- blob1 (file content)
    +- blob2 (file content)
    +- blob3 (file content)
```

Multiple Commits Diagram

```
master
|
C3 -> C2 -> C1
|
testing
|
HEAD
```

After commiting in `testing` branch:

```
       master
       |
C4 ->  C3 -> C2 -> C1
|
testing
|
HEAD
```

After checking out `master`:

```
       HEAD
       |
       master
       |
C4 ->  C3 -> C2 -> C1
|
testing
```

After adding commit to `master`:

```
HEAD
|
master
|
C5
  \
   C3 -> C2 -> C1
  /
C4  
|
testing
```

### Basic Branching and Merging

#### Create Branch

```
git checkout -b feature
```

is the same as:

```
git branch feature
git checkout feature
```

- Git doesn't let you switch branch without a clean working state

Switching to `master` branch and merge `feature` branch in:

```
git checkout master
git merge feature
Updating <hash>..<hash>
Fast forward
 file | 1 -
 X file changed, Y insertions(+), Z deletions(-)
```

- _Fast forward_ means the commit pointed to by `feature` is directly upstream of `master`
- Git simplifies things by moving the pointer forward when merging commit A with commit B where B can be reached by following A's history

To delete branch:

`git branch -d feature`