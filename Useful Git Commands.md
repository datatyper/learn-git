# Useful Git Commands
Git is the most popular version control system (VCS).  
Not sure that you have it? You can check with
```
git --version
```
and see if you get a response. Otherwise, download it [here](https://git-scm.com/downloads)

## Starting your Project
First create a folder for your project. Move into it and initialize.
```
mkdir my_new_project
cd my_new_project
git init
```

## Tracking Changes with your Project
As your project grows, you'll want to make regular commits to keep track it. 

Think of this like having regular savepoints in a game. The more often you commit the less progress you lose when you die (or in this case your computer crashes).

This is usually how it goes,
* You make changes
* You add those changes
```
git add .
```
* You commit those changes
```
git commit -m "description of changes"
```

These commands you will type over and over again.

## Seeing Changes
You can see a history of changes by typing,
```
git log
```
This will show you,
* The 40 character SHA value (a label for your set of changes)
* Metadata (including the Author and Date of the changes)
* The commit message (which is why it's important to write descriptive messages when committing)

You can see the changes since your last commit by typing,
```
git diff
```
Note that this shows differences between your 'working tree' and 'staged tree'. If you stage those changes with `git add <filename>` then these differences will not be shown because the 'working tree' is the same as the 'staged tree'. To show differences between the staged tree and and repo use,
```
git diff --staged
```
To clarify, if you have staged *all* your changes with `git add .` then `git diff` won't return any changes. Instead use `git diff --staged`.







