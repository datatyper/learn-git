# Git in Powershell Command Cheat Sheet
> ## We'll be able to fly, don't fear the repo
*Author: Philip Mannering*  
*Date: 2020-04-05*

Git is the most popular ___version control system (VCS)___ that is especially useful in tracking changes to text documents. Learn git now to avoid naming files like `my_source_code_v4_final_final.py`

Below are a list of frequently used git commands that I use in Powershell to track my the development of code. This document was written by myself for myself as a reference.

If you do not have git you can download it from `https://git-scm.com`

## Check you have Git Installed
```
> git --version
git version 2.26.0.windows.1
```
Or simply type `git` to get a list of useful commands
```
> git
start a working area (see also: git help tutorial)
   clone             Clone a repository into a new directory
   init              Create an empty Git repository or reinitialize an existing one
   ⋮
```

For specific information on a git command (say, git status) use,
```
> git help status
```

## Starting a Project  
First navigate to the project folder using `> cd path/to/folder`.

Clone and existing repository, 
```
> git clone <remote directory>
```
Or to start a new project,
```
> mkdir my_new_project
> cd my_new_project
> git init
```
Check your project has the _.git_ folder by using either
```
> Get-ChildItem -Force    # for all files
> Get-ChildItem -Hidden   # Just the hidden files
> ls -h					  # shorthand
```

## Tracking Changes
The **most important command** to check which files are being tracked and which files have been modified to be _staged_ or _commited_ (or both).
```
> git status
```

The vast majority of your workflow,
```
(make changes)
> git add .
> git commit -m "A descriptive label of changes"
(make more changes)
> git add .
> git commit -m "Another descriptive label of changes"
(make even more changes)
> git add .
> git commit -m "Another descriptive label of changes"
```
Or add and commit changes in a single command,
```
git commit -am "Another descriptive label of changes
```
Or to add to previous commit,
```
> git add .
> git commit --amend
```
Tips,
* Use `> git status` often to check which files need to be staged and committed.
* Add similar edits together under a single commit.
* Use `> code` or `> code newfile.txt` to open up Visual Studio Code from the command line.

To see a history of changes,
```
> git log
> git log -n 3    						# Last 5 changes
> git log --since=2020-01-01    		# Changes since the start of 2020
> git log --author=philip				# Changes made by me
> git log --grep regex          		# Changes that match the regex
> git log --oneline 					# Succint format
```

To see exactly what the changes are,
```
> git diff 						# Changes between repository and working directory
> git diff --staged 			# Changes between repository and staged index.
> git diff --color-words 		# Changes colored by word (not line)
```

To see exact changes of previous commits
```
> git show 38afe0d
```
Where 38afe0d is a sha# which can be found when using `> git log`.

To delete or move a file,
```
> git rm file1.txt
> git commit -m "deleting file1.txt"
```
```
> git mv old.txt new.txt
> git commit -m "renaming from old.txt to new.txt"
```

## When you Accidentally Delete Everything

If you make a mistake (working directory is wrong and repo is correct)
```
> git restore file.txt --source head
```
To roll back to an earlier version (and lose any current changes),
```
> git reset --hard 38afe0d		# Very destructive. Use with caution!
```
To undo the last commit, but keep our current edits in the working directory _and_ keep the changes of the commits in the working directory (essentially files stay the same but it allows you to merge commits) use,
```
> git reset HEAD~1 						# this is --mixed by default
> git commit -m "descriptive label"		# add the commits back as one
```

Finally, if we want to undo a change we made further back in history,
```
> git revert 38afe0d
```


## Learning to Branch Out
Creating a new branch for you to work on
```
> git branch <new branch name>
```
See all branches in your remote repository
```
> git branch -a
```
Moving onto a branch
```
> git checkout <branch name>
```
Deleting a branch
```
> git branch -d <branch name>
```
Moving files while preserving git history
```
> git mv <source> <destination>
 ```
 
making a new branch whilst moving into it AT THE SAME TIME
```
> git checkout -b <new branch name>
```

## Push It

Finally, upload your changes to a remote repository on github,
```
git push
```

