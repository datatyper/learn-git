# Useful Git Commands
Git is the most popular version control system (VCS).  
Not sure that you have it? You can check with
```
git --version
```
and see if you get a response. Otherwise, download it at [git-scm.com/downloads](https://git-scm.com/downloads)



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

These commands you will type over and over again. In fact, there so common that often it's more convenient to stage and commit in one go. To do this use the -a flag,
```
git commit -am "Adding and committing in one go"
```

Note that you don't always want to use `git add <file>` to add a change set to the staging tree. 
* If you're deleting a file use `git rm <file>`
* If you're renaming a file use `git mv <old_file> <new_file>`

## Amend
Now let's say that I've committed some changes but forgot something that I wanted to include in that commit. This can be done by first staging the changes and then using `git amend`. Or say there is a typo in one of my commit messages. I might do,
```
git commit -m "Initial comit"
git commit --amend -m "Initial commit"
```
Warning: this can only be done with the very latest commit.

## Seeing Changes
You can see a history of changes by typing,
```
git log
```
This will show you,
* The 40 character SHA value (a label for your set of changes)
* Metadata (including the Author and Date of the changes)
* The commit message (which is why it's important to write descriptive messages when committing)

My preferred view of the history can be got with,
```
git log --oneline --stat -n 3
```
where
* `--oneline` abbreviates the SHA value and hides metadata
* `--stat` shows the number of changes in each commit 
* `-n 3` limits the output to the last 3 commits only


You can see the changes since your last commit by typing,
```
git diff
```
Note that this shows differences between your 'working tree' and 'staged tree'. If you stage those changes with `git add <file>` then these differences will not be shown because the 'working tree' is the same as the 'staged tree'. To show differences between the staged tree and and repo use,
```
git diff --staged
```
To clarify, if you have staged *all* your changes with `git add .` then `git diff` won't return any changes. Instead use `git diff --staged`.

By default the changes are colored so that the entire lines are either green (for added) or red (for deleted). Often a more convenient view is to color the individual words. Do this with,
```
git diff --color-words
```

As with `git log`, I prefer the condensed view that only shows the lines that changed. This can be achieved with,
```
git diff -U0
```

## Delete a File
You can remove a file from a project with,
```
git rm <file>
git commit -m "Removing <file> from project"
```
Note that you still need to commit the removal of a file, in the same way you would the addition of a file.

## Undo
So why bother with all of this? One good reason is to revert back to an earlier state. 

For instance, if we accidentally delete some text in a file or the file in its entirety we can use,
```
git restore <file>
```
This will restore the state of the file since our last commit.

If instead we made a change several commits ago that we would like to undo, we can use,
```
git revert <sha value>
```
This will not delete this commit from our `git log` but instead adds another commit to undo whatever the change was. The default commit message will be something like:  *Revert "Original commit message"*



We can also use `git restore` to unstage a file
```
git restore --staged <file>
```

## Ignore
Finally, there are often lots of files that we don't want to track. For this we can create a .gitignore text file. Each line in the .gitignore file can specify a file or set of files to ignore tracking with git. An example file might be,
```.gitignore
# Ignore this temp file
.DS_Store
# Ignore all zip files
*.zip
# Ignore all log files
*.log
# Ignore all files in my img folder
img/
```







