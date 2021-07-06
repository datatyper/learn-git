# Useful Git Commands
Git is the most popular version control system (VCS).  
Not sure that you have it? You can check with
```
git --version
```
and see if you get a response. Otherwise, download it at [git-scm.com/downloads](https://git-scm.com/downloads)

If you're not sure if the current directory is a git project then you can either look for a .git directory or run,
```
git status
```
Anything other than,  
**`> fatal: not a git repository (or any of the parent directories): .git`**
means that your are in a git tracked project.

In fact you will use `git status` *all* the time to check to check the state of your project (such as which files have been modified, staged, and what to do next - more on this in the next section)

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
* The 40 character SHA-1 hash value (a unique label for your set of changes)
* Metadata (including the Author and Date of the changes)
* The commit message (which is why it's important to write descriptive messages when committing)

My preferred view of the history can be got with,
```
git log --oneline --stat -3
```
where
* `--oneline` abbreviates the SHA value and hides metadata
* `--stat` shows the number of changes in each commit 
* `-n3` or simply `-3` limits the output to the last 3 commits only

Another good way to filter the results is by datetime such as,
```
git log --since 2021-07-04T00:00        # Show all commits since the start of July 4th
git log --since "12 hours ago"          # Show all commits made in the last 12 hours
git log --until "3 days ago"            # Show all commits up until 3 days ago
git log --grep "data" -i                # Show all commits with 'data' in the message (case insensitive)
git log --author Philip                 # Show all commits made by 'Philip'
git log <file>                          # Show all commits with changes to <file>
```


If you want to see the exact changes made in a specific commit you can use,
```
git show <sha value>
```
Tip: when referring to the sha value you can use as few as four characters (first) to identify that commit.

There are more convenient ways to refer to a specific commit. A few examples are shown below with the command git show (though they would also work with `git revert`, `git diff`, etc)
```
git show head           # the latest commit
git show head^          # the parent of the latest commit (i.e. second last commit)
git show head^^         # the grandparent of the latest commit (i.e. third last commit)
git show head~3         # the great-grandparent of the latest commit (i.e fourth last commit)
```

You can see the changes between your working tree (current project) and your last commit by typing,
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
So why bother with all of this? One good reason is to revert back to an earlier state if you royally f*ck things up / make a mistake and can't be bothered to fix it manually.

For instance, if our whole project gets corrupted we can use,
```
git restore .
```
to reset our project to the state at last commit.

If we accidentally delete some text in a file or the file in its entirety we can use,
```
git restore <file>
```
This will restore the state of that file since our last commit.

Warning: this will only undo changes that are in our working tree... that is, it will not undo staged changes. So if `git status` shows staged changes that you also want to disregard, first use, `git restore --staged <file>` to unstage the file.


If instead we made a change several commits ago that we would like to undo, we can use,
```
git revert <sha value>
```
This will not delete this commit from our `git log` but instead adds another commit to undo whatever the change was. The default commit message will be something like:  *Revert "Original commit message"* 


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

## Branches
So far so good. We commit 'atomic' changes to out project incrementally, safe in the knowledge that we can undo any one of these with a single `git revert <sha value>` if necessary. But to really leverage the power of git, you'll want o become familiar with branches.

Let's say that you want to try something ___really___ experimental with your project. Here you will want to create a new branch (an alternate reality if you will) where you can try out new ideas without risk of affecting the master branch/version of the project.

Here's what to do,
1. Create a new branch
```
git branch alt_universe
```
2. Switch to that branch
```
git switch alt_universe
```
Now any changes that you make to this project will only apply to this branch! This can be quite liberating - like having infinite lives in a computer game.

3. Make changes (and commits) to the 'alt_universe' project branch.

When we've finished we can switch back to our master branch (`git switch master`) with everything preserved the way it was. 

4. If we want to incorporate those changes (while in the master branch)
```
git merge alt_universe
```
Other useful branch commands are,

```
git branch                  # to list branches (an * indicates the current branch)
git branch -m <old> <new>   # to rename a branch from <old> to <new>
git branch -d <new>         # to delete a branch
git branch --merged         # list all branches with commits in the current branch
```

If you come back to a project and forget why a branch was created you can view the differences with,
```
git diff <old> <new>
```
The command `git diff` works between branches like it would between commits in a single branch.

# Github
Up until now we've been working with *git* - the version control system. This is not to be confused with github which is a remote server that can host our git projects allowing for collaboration. [Github](https://github.com) is one such host for git projects; others include [GitLab](https://gitlab.com) and [Bitbucket](https://bitbucket.org)

## Save Repo to Github
To save a local repository to GitHub so that others can work on it
1. Create a repository on GitHub.
2. Add the remote repo to our git project
```
git remote add origin https://github.com/<username>/<project>.git
```
where `<username>` is your profile name, e.g. *datatyper* and `<project>` is the project name, e.g. *learn-git*

We can check that we've designated a remote repo by typing `git remote` and you should see *origin* pop up in your command line or by typing `git branch -r` or `git branch -a` to see the remote branch, e.g. *origin/master*. Note that you can call the remote repo whatever you like, but 'origin' is convention.

3. Push the changes up to github
```
git push -u origin master       # The first time push
git push                        # Subsequent pushes
```

This is a little confusing. You could use `git push origin master` every time. However, by adding the -u the first time, you set *master* as the 'tracking' or default branch. That means every subsequent command can be just `git push`.

## Copy Repo from GitHub

Finally, one of the most common commands you will want to use is `git clone` to steal (I mean contribute to) other people's code. This is very straight forward. First navigate to a folder you want to contain the new project and type,
```
git clone https://github.com/trekhleb/learn-python.git
```
where the project is the HTTPS url which can be found by clicking on the big green *Code* button on the github page. In fact this is identical to the browser url with `.git` appended.

This command will copy the master branch down onto your local computer.

## Summary
I hope a quick reference to some of my most used git commands have been helpful. There are literally hundreds more variations on these commands waiting to be explored which I will update with here should they become a regular part of my workflow.