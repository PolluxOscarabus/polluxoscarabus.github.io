---
layout: article
title: Manage a local Git repository
category: Dev_Tools
tag: [Git]
date: 2017-05-28
---

## *Set a local repository*

### Create a local Git repository
A **.git** subdirectory is used to set current working directory as Git Repository
```
git init
```
Clone a local copy of a remote Git repository
```
git clone **REPOSITORY_URL**  
```
The name of the copy can be set to differ from the original
```
git clone **REPOSITORY_URL** **NEW_NAME**
```

### Configure the local Git repository
Set username (use same as account in the server with remote repository)
```
git config user.name "USERNAME"
```
Set email (use GitHub noply email to not show email in logs)
```
git config user.email **EMAIL_ADDRESS**
```
Set editor
```
git config core.editor vim
```
Set commands aliases
```
git config alias.**ALIAS_NAME** "**COMMAND_NAME**"
```
Check the configuration settings
```
git config -l
```

***  

## *Check working directory and staging area*

### Check status of files in repository 
For a detailled status
```
git status
```
For a short status
```
git status -s
```
Status is displayed in two columns indicated by symbols :  
  > left-hand symbol : staging area  
  > right-hand : working directory  
  > **??** : untracked  
  > **M**  : modified  
  > **A**  : added  

### Check modifications made to files
Check difference between unstaged modified files and files from latest commit
```
git diff
```
Check difference between staged modified files and files from latest commit
```
git diff --staged
```

### Check commits history 
Display list commits by date
```
git log
```
Display graph of commits in a formatted list showing their hash, date, and subject
```
git log --pretty=format:"%H %ad %s" --graph --oneline
```
Display the Branch pointer referred to by the HEAD pointer
```
git log --oneline --decorate
```
Filter list of commit by date
```
git log --since=2.weeks
```
Filter list of commit by files containing modified code matching given string 
```
git log -S**STRING**
```
Filter list of commit where a given file was modified
```
git log -- **FILE_NAME**
```

***  

## *Manage staging area and commits*

### Stage modified or untracked files
Content indicated in the .gitignore file will be ignored
```
git add 
```

### Delete staged file from repository
Remove files from working directory, and stage the removal
```
git rm **FILE_NAME**
```
Remove modified file that were staged
```
git rm -f **FILE_NAME**
```
Remove file from staging area only, but keep it on the working repository
```
git rm --cached **FILE_NAME**
```

### Rename file
A file is moved to be renamed
```
git mv **OLD_NAME** **NEW_NAME** 
```

### Remove untracked files
Delete untracked files from working tree
```
git clean -i
```

### Reset file to state of its latest staging
```
git checkout -- **FILE_NAME**
```


### Unstaging a staged file
```
git reset HEAD **FILE_NAME**
```

### Commit changes to Git Repositoryb
Create a snapshot of the state of all the staged files of the repository
```
git commit -m "**COMMIT_MESSAGE**"
```
Command can be set to stage tracked modified files while doing the commit
```
git commit -a -m "**COMMIT_MESSAGE**"
```

### Replace latest commit with state of files in current staging area
```
git commit --amend
```

### Restore state of working directory and staging area from previous commit
```
git reset --hard **COMMIT_HASH**
```

### Reset files to state from previous commit 
```
git checkout **COMMIT_HASH** **FILE_NAME** 
```

### Create a new commit from state of previous commit given
The new commit is appended in the history and previous commits are not removed
```
git revert **COMMIT_HASH**
```

***  

## *Manage remote handles*


### List shortnames and associated urls of remotes
```
git remote -v
```

### Display details about remotes
```
git remote show **remote_name**
```

### List remote branches tracked by each local branches 
```
git branch -vv
```

### Add remote handle
```
git remote add **NEW_REMOTE_NAME** **REMOTE_URL**
```

### Create local remote-tracking branch of the branch of a tracked remote, and switch to new branch
Remote-tracking branch will need to be merged before being editable
```
git checkout -b **TRACKING_BRANCH_NAME** **REMOTE_NAME**/**REMOTE_BRANCH_NAME**
```

### Change the remote branch tracked by the current branch
```
git checkout -u **REMOTE_NAME**/**REMOTE_BRANCH_NAME**
```

### Fetch data about branches of remote repository stored as local remote-tracking branches
Get remote-tracking branch from remote, named **REMOTE_NAME**/**BRANCH_NAME** and not editable until being merged
```
git fetch **REMOTE_NAME**  
```
To fetch data to update remote-tracking branches from all tracked remotes
```
git fetch --all
```

### Merge the data of a branch from a tracked remote previously fetched
Resulting local branch will have the same name as the name of the branch of the remote being merged
```
git merge **REMOTE_NAME**/**REMOTE_BRANCH_NAME**
```

### Push data from local branch to the branch with the same name in the tracked remote
```
git push **REMOTE_NAME** **LOCAL_BRANCH_NAME**
```

### Push data from local branch to the branch with a given different name in the tracked remote
```
git push **REMOTE_NAME** **LOCAL_BRANCH_NAME**:**REMOTE_BRANCH_NAME**
```

### Modify the shortname of a remote handle
```
git remote rename **OLD_NAME** **NEW_NAME**
```

### Remove a remote handle
```
git remote rm **REMOTE_NAME**
```

### Delete a remote branch
```
git push **REMOTE_NAME** --delete **REMOTE_BRANCH_NAME**
```

### Push modification to change remote commit history made locally
```
git push --force
```

***  

## *Manage Tags*

### List tags
```
git tag
```

### Check information about a tagged commit
```
git show **TAG_NAME**
```

### List tags matching the given pattern 
```
git tag -l "**STRING**"
```

### Create lightweight tag
```
git tag **TAG_NAME**
```

### Create annotated tags
```
git tag -a **TAG_NAME** -m "**TAG_MESSAGE**"
```

### Create annotated tags from old commits
```
git tag -a **TAG_NAME** **COMMIT_HASH**
```

### Push tags to remote server
```
git push **REMOTE_NAME** **TAG_NAME**  
```
To push all tags
```
git push **REMOTE_NAME** --tags
```

***  

## *Manage branches*

### List existing branches and last commit of each one
```
git branch -v
```

### List branches that were merged into the current branch
Branches listed are usually safe to delete without losing any work
```
git branch --merged
```

### List branches that were not yet merged into the current branch
Branches listed must use -D flag to force deletion and confirm resulting loss of data
```
git branch --no-merged
```

### Create a new Branch but doesn't switch to the new branch
```
git branch **BRANCH_NAME**
```

### Create a new branch and switch to it
```
git checkout -b **BRANCH_NAME**
```

### Create and switch to a new branch with a working directory in same state as the tagged commit
```
git checkout -b **BRANCH_NAME** **TAG_NAME**
```

### Switch to an existing branch
The working directory is changed to the state of the latest commit of the new branch
```
git checkout **BRANCH_NAME**
```

### Merge given branch into current branch
```
git merge **BRANCH_NAME**
```

### Delete a branch
```
git checkout -d **BRANCH_NAME**
```

### Rebase the current branch onto the given branch 
```
git rebase **TARGET_BRANCH_NAME**
```

### Rebase a branch onto another branch
```
git rebase **TARGET_BRANCH_NAME** **REBASED_BRANCH_NAME**
```

### Rebase a branch, diverging from a second branch, onto a target branch from which the second branch diverges
```
git rebase --onto **TARGET_BRANCH** **MIDDLE_BRANCH**  **REBASED_BRANCH**
```

### Rebase a remote branch onto current branch while fetching data from remote
```
git pull --rebase **BRANCH_TO_REBASE**
```

***  

## *Notes*
Three level of specificity for Git settings 
* using **--local** flag : set info specific to a project in the file ./.git/config
* using **--global** flag : set info to be specific to a user setting the file in his home ~/.gitconfig
* using **--system** flag : set info to be system-wide in the file ./.git/config
  
Git commands accept files, directories, and file-glob patterns
* Asterix must be preceded by a backslash because of git own filename extension 
```
git rm log/\*.log
```
  
Different types of tag
* lightweight tag : pointer to a specific commit
* annotated tag : full objects stored in Git database containing detailled information
  
Tags are not transferred in default push, but are fetched when repository is pulled or cloned
  
Commit command creates a Commit Object containing various information
* A pointer to a tree of checksums
  * Each checksum was created from each file in the staging area 
  * Checksums are stored in the .git repository as blob
* The author's name, and email as well as some other user profile information
* A pointer to the parent commit (they can be multiple in the case of merged branches)
  
Branches are lighweight pointers to Commit objects
* the current branch is accessed through an HEAD pointer referring to the pointer of the current branch
  
Switching between two branches is prevented if there are uncommited changes conflicting with the target branch
  
Different way of merging branches
* Fast-Forwarding : 
  * Done when the branch to merge is only one commit ahead of the current branch
  * Move the snapshot of the current branch forward to replace the snapshot of the merged branch
  * No new snapshot is created
* Three-Way Merge
  * Create a new snapshot in the current branch with as parents each latest snapshot of the two branches merging
  
To resolve a conflict during merge
* check status of interrupted merge for information to list conflicting files
* check files to see details about conflicts comments
* Staged files modified to fix conflict
  
Rebasing can be done as alternative to merge
* Rebasing a branch remove signs of its existence in the log of commits
* After a rebase, the current branch can be merged into the target branch after checking out onto
* The commit resulting from a rebasing allows for a fast-forwarding merge between the two branches
* Rebasing should never be done for commits of remote repository
  
Local remote references are pointer used to track information about state of remote repositories
* Local pointer pointing to the branch of a remote would be named **REMOTE_NAME**/**BRANCH_NAME**
  * Remote-tracking branches are references to the state of remote branches
  * Local branches are not automatically synchronized so data need to be fetched from tracked remotes
