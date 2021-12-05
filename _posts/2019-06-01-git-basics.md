---
layout: post
title: "Git Notes"
date: 2019-06-01
categories: git
---

## Refence

([Pro Git Book](https://book.git-scm.com/book/en/v2)/Getting-Started-About-Version-Control)

## Intro

The default branch is master.
Add is also used to stage the modified file, to resolve the conflicts.

## File Stages

### Untracked

The files that are not in the local repository.
You can stage to local repository by running `git add <file name>`, so that it getting into Staged (HEAD). To removed is from Staged(Unstage), run `git reset HEAD <file name>`

### UnModified

The files that are in local repository but not modified.

### Modified

The files that are in local repository but modified.
To discard the changes (or UnModified) run `git checkout #<file name>`.
To stage the modified file to local repository run `git add <file>`.
To unstage run `git reset HEAD <file name>`.

### Staged

The files are in local repository staging area but not commited to local repository
To commit to local repository run git
To unstage run `git reset HEAD <file name>`.

Index - Is same as Stagged

HEAD -

### Create git repository

`git init`

### Add to local repository

Add is also used to stage the modified file, to resolve the conflicts

`git add <file name>`

`git add .`

`git add *`

### Check the status for file: Untracked/UnModified/Modified/Staged

`git status`

### Commit to local repository

`git commit -m "Commint message"`

### Pushing changes to remote

`git push origin master`

### Add remote origin

`git remote add origin <server>`

### Updating from remote repository

`git pull`

### Reverting the local changes from local repository

`git checkout -- <filename>`

## Reverting the remote changes

### Below command fetch the history from remote

`git fetch origin`

### Below command resets the local repository

`git reset --hard origin/master`

## Cloning from server

When cloned all the file versions are downloaded. Below command clones to reactapps folder

(`git clone https://srinivasc@bitbucket.org/srinivasc/reactapps.git`)

### Clone to a different directory MyReactApps

(`git clone https://srinivasc@bitbucket.org/srinivasc/reactapps.git MyReactApps`)

## FIND DIFFERENCES

### Modified file and staged

`git diff`

### Staged and committed file

--staged and --cached are synonyms

`git diff --staged`

### Modified and committed file

`git diff head`

### Local branch and remote branch

`git diff <local branch> <remote>/<remote branch>`

### List file names only in differences

`git diff --name-only`

### List file names and change type

`git diff --name-status`

### Using winmerge

## Committing

### Committing to local repository

`git commit -m "Message for logging"`

### Skip staging area

`git commit -a -m "Message for logging"`

## Removing files

### Remove local working file first

`rm <file name>`

### Stage the removed file, use -f option if the file to be removed is modified and in the staging area

`git rm <file name>`

### Commit

`git commit -m "Message"`

### Remove file from local repository but have it on the hard drive

`git rm --cached <file name>`

### Renaming/Moving file

`git mv <old file name> <new file name>`

### Above command is same as

`mv <old file name> <new file name>`

`git rm <old file name>`

`git add <new file name>`

### Undoing the changes on staged files

`git reset HEAD <file name>`

### Undoing the changes on local modified copy

`git checkout -- <file name>`

## Remote repository commands

### List remote host. The default name git assigns to remote is origin when you clone.

`git remote`

### List the remote name and url.

`git remote -v`

### Add remote

`git remote add <RemoteName> <URL>`

### Fetch command gets the remote repository into the local repository

`git fetch <remote>`

### Pull does the fetch and merge process

### Push: commit to the remote repository

`git push <remote> <branch name>`

### Rename remote

`git remote rename <old name> <new name>`

### Remove remote

`git remote remove <remote name>`

### List git configs

`git config --list --show-origin`

### Git alias to add, commit and push. Add this line to `<primary drive>:\Users\<User login name>\.gitconfig`

```
[alias]
acp = "!f() { git add -A && git commit -m \"\$@\" && git push; }; f"
history = "log --all --decorate --oneline --graph"
```

## Branches

[Fetch and Merger Reference Link](https://longair.net/blog/2009/04/16/git-fetch-and-merge/)

### List local branch

`git branch`

### List remote branch

`git branch -r`

### Create new Branches

```git
git branch <branchname> -- only creates the branch
git checkout <branchname> -- head points to the branch
or
git checkout -b <branchname> -- creates the branch and head points to it

```

### Delete Branch

`git branch -d <branchname>`

### List branches which are merged

`git branch --merged`

### List branches which are not merged

`git branch --no-merged`

## Branch Remote

### List remote branches

`git ls-remote <remotename>`
`git remote show <remotename>`
`git branch -vv`
`git branch -r`

### Track to the remote branch

```git

When git is cloned, it automatically trackes the <remote>/<masterbranch>

git checkout -b <localbranch> <remote>/<remotebranch>
--Shortland of above
git checkout --track <remote>/<remotebranch>
--Shorthand to above Shorthand
git checkout <remotebranch>

When the you want a local branch to track remote branch or change the existing tracking then use below

git branch -u <remote>/<branch>
--referene the upstream by @{u}

--Below list current trackings
`git branch -vv`

```

### Delete remote branch

`git push <remote> --delete <remotebranch>`

### Merging changes to the remote repository

1. Do a fetch

   `git fetch <remote name> <branch name>`

2. Do a diff

   `git diff <local branch> <remote rep>/<remote branch>`
   example: git diff master origin/master

3. Do a merge

   `git merger <remote rep>/<remote branch>`
   example: git merge origin/master

[Git Workflow Reference Link](https://blog.osteele.com/2008/05/my-git-workflow/)

### History

- Get complete log

`git log --all --decorate --oneline --graph`

- Get file history

`git log -L:ConfigureServices:Startup.cs`

### SSH keys

```txt
Run `ssh-keygen` command and accept defaults to generate the keys.
This will generate id_rsa and id_rsa.pub files.
id_rsa is private key and don't share this. id_rsa.pub is public key and can be added to online git repositories settings.
```

### .git sample

```
[credential]
	helper = manager
[alias]
	acp = "!f() { git add -A && git commit -m \"$@\" && git push; }; f"
    history = "log --all --decorate --oneline --graph"

[user]
	email = firstname.lastname@gmail.com
	name = First Name Last Name

[mergetool]
    prompt = false
    keepBackup = false
    keepTemporaries = false

[merge]
    tool = winmerge

[mergetool "winmerge"]
    name = WinMerge
    trustExitCode = true
    cmd = "/c/Program\\ Files/WinMerge/WinMergeU.exe" -u -e -fm -wl -dl "Local" -wr -dr "Remote" $LOCAL $MERGED $REMOTE

[diff]
    tool = winmerge

[difftool "winmerge"]
    name = WinMerge
    trustExitCode = true
    cmd = "/c/Program\\ Files\\/WinMerge/WinMergeU.exe" -u -e $LOCAL $REMOTE
```
